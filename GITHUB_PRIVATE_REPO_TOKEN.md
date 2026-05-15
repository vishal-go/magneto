# GitHub Setup for GitSync (Private Repo + Restricted Token)

This guide shows how to:

1. Create a **private GitHub repository** for your Obsidian vault
2. Generate a **restricted (least-privilege) token** that can access **only that repository**

This is the most secure setup for GitSync.

---

## Why “restricted token” matters

GitSync authenticates to GitHub using a Personal Access Token (PAT). A token is like a password.

To keep access limited, use a **fine-grained PAT** and restrict it to a **single repository**.

---

## Part A — Create a private GitHub repository

You must create the repository first. (A restricted token can only be scoped to repositories that already exist.)

1. Sign in to GitHub.
2. Go to https://github.com/new
3. Fill in:
   - **Owner**: your account (or the org that owns the repo)
   - **Repository name**: example `obsidian-vault`
   - **Visibility**: select **Private**
4. Optional (recommended): check **Add a README file**.
   - This initializes the default branch (usually `main`) which avoids branch-name confusion later.
5. Click **Create repository**.

### Keep the repo name handy

In GitSync settings, the field **Repository name** expects **only the repo name**, not the full `owner/repo`.

- Correct: `obsidian-vault`
- Incorrect: `your-username/obsidian-vault`

---

## Part B — Generate a fine-grained token (restricted to that repo)

### 1) Open token settings

1. GitHub → **Settings**
2. **Developer settings**
3. **Personal access tokens**
4. **Fine-grained tokens**
5. Click **Generate new token**

### 2) Configure token basics

- **Token name**: `Obsidian GitSync (restricted)`
- **Expiration**: choose a reasonable limit (recommended).
- **Resource owner**: choose the account/org that owns the repository.

### 3) Restrict repository access

Under **Repository access**:

- Select **Only select repositories**
- Choose your repo (example: `obsidian-vault`)

This is the key step that restricts the token to a single repo.

### 4) Set minimal permissions (least privilege)

Under **Repository permissions**, set:

- **Contents**: **Read and write**
  - Required for upload/download notes and attachments.
- **Metadata**: **Read-only**
  - Usually required by GitHub for repo access; sometimes auto-selected.

Nothing else should be necessary for GitSync.

### 5) Generate and copy the token

1. Click **Generate token**.
2. Copy it immediately and store it safely.

If you lose it, you must generate a new token.

---

## Part C — Configure GitSync in Obsidian (mobile or desktop)

1. Obsidian → **Settings → Community plugins → GitSync**
2. Set:
   - **GitHub username**: your GitHub username
   - **Personal access token**: paste the fine-grained token
   - **Repository name**: repo name only (example: `obsidian-vault`)
   - **Branch name**: `main` (or whatever your repo uses)
3. Click **Test connection**.

Then run your first sync:

- If this device already has your notes: use **Push** (or **Full sync**)
- If you want to download from GitHub into a new vault: use **Pull** first

---

## Troubleshooting

### Test connection fails (but you created the repo)

- Confirm **Repository name** is only the name (not `owner/repo`).
- Confirm the fine-grained token is restricted to the correct repo.
- Confirm token permissions include:
  - Contents: Read and write
  - Metadata: Read-only

### 404 Not Found

- Wrong username or repo name
- Token restricted to a different repo

### 403 Forbidden

- Token missing required permission(s)
- Token expired
- Organization repo requires SSO authorization for the token (common in company orgs)

---

## Security tips

- Prefer fine-grained PATs over classic PATs for restricted access.
- Use token expiration and rotate tokens periodically.
- If you suspect compromise, revoke the token immediately:
  - GitHub → Settings → Developer settings → Personal access tokens
- Avoid syncing your Obsidian config folder to GitHub (plugins can store secrets there).

---

## Alternative (less restricted): classic PAT

GitSync can auto-create a repository if it doesn’t exist, but that typically requires a **classic PAT** with broad `repo` scope.

If you want the most restricted access, stick to:

- Create repo manually (Part A)
- Use fine-grained token restricted to that repo (Part B)
