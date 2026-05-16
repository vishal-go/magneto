# MAGNETO on Mobile (Android & iOS) — Setup + Usage Guide

This guide explains how to install and use **MAGNETO** on **Obsidian Mobile**. MAGNETO works without Git by using the GitHub REST API.

## What you need

- **Obsidian Mobile** installed (Android or iOS)
- A vault stored locally on your phone (any location Obsidian can access)
- A **GitHub account**
- A **GitHub Personal Access Token** (PAT)

## Important notes (read first)

- **Repository name must be only the repo name** (example: `my-obsidian-vault`).
  - Do **not** enter `username/my-obsidian-vault` in the “Repository name” field.
- **Auto-sync only runs while Obsidian is open.** Mobile OSes pause background timers when apps are closed.
- **Do not sync your Obsidian config folder to GitHub.**
  - By default MAGNETO excludes `{{configDir}}/plugins` and `{{configDir}}/themes`.
  - Keep those exclusions. Your token is stored in the plugin’s data on-device; syncing `.obsidian/plugins` can accidentally publish secrets to GitHub.

---

## Step 1 — Install MAGNETO on your phone

### Option A (recommended): Install from Community Plugins

1. Open **Obsidian** on your phone.
2. Go to **Settings → Community plugins**.
3. Turn off **Restricted mode** (also called “Safe mode” in some versions).
4. Tap **Browse**.
5. Search for **MAGNETO**.
6. Tap **Install**.
7. Tap **Enable**.

After enabling, you should see a MAGNETO entry under installed community plugins.

### Option B: Manual install (if MAGNETO does not appear in Browse)

You need these 3 files from the latest release:

- `main.js`
- `manifest.json`
- `styles.css`

Create this folder inside your vault:

- `<YourVault>/.obsidian/plugins/magneto/`

Then copy the 3 files into that folder.

#### Android manual install tips

- If your vault is stored under `Android/data/...`, some file managers can’t access it due to Android storage restrictions.
- Manual installation is easiest if your vault is in a user-accessible location (for example `Documents/Obsidian/<VaultName>`), or if you copy the plugin folder from a computer.

#### iOS manual install tips

- The easiest path is using the **Files** app if your vault is stored in **iCloud Drive** or **On My iPhone/iPad**.
- After copying files, fully close Obsidian (swipe away) and reopen.

---

## Step 2 — Create a GitHub Personal Access Token (PAT)

Token generation is documented in one place:

- See **README → Setup**: [../README.md](../README.md#setup)

Recommended: a fine-grained token restricted to your repo (Contents RW + Metadata RO).

---

## Step 3 — Configure MAGNETO in Obsidian Mobile

1. In Obsidian, open **Settings**.
2. Go to **Community plugins → MAGNETO**.
3. Fill in:
   - **GitHub username**: your GitHub username
   - **Personal access token**: paste your PAT
   - **Repository name**: repo name only (example: `obsidian-vault`)
   - **Branch name**: usually `main` (use `master` if your repo uses it)
4. Tap **Test connection**.

### If “Test connection” fails but your credentials are correct

The **Test connection** button checks whether the repository already exists and is reachable.

- If you used a **fine-grained token**, the repo must already exist (create it first).
- If you used a **classic PAT** and the repo does **not** exist yet, MAGNETO may create it during **Push** or **Sync**, but **Test connection** may fail until after that first push.

---

## Step 4 — First-time sync (the safest workflow)

### If this is your first device (you’re creating the GitHub repo)

1. Configure MAGNETO (Step 3).
2. Use **Manual sync → Push to GitHub** (or **Full sync**).
3. Wait for the notice confirming how many files were uploaded.

If you’re using a classic PAT and the repo doesn’t exist, this may create a private GitHub repository automatically.

### If you’re setting up a second device (another phone/tablet/desktop)

1. Install MAGNETO on the second device.
2. Configure it with the **same** username/repo/branch.
3. Run **Pull from GitHub** first.
4. After the pull completes, you can use **Full sync** going forward.

---

## How to run sync on mobile (day-to-day)

You can run MAGNETO in any of these ways:

- Tap the **ribbon icon** (git-branch icon) to run “Sync with GitHub”.
- Use **Command Palette** and run:
  - `MAGNETO: Push to GitHub`
  - `MAGNETO: Pull from GitHub`
  - `MAGNETO: Sync with GitHub`
- Open **Settings → Community plugins → MAGNETO → Manual sync** buttons.

### Auto-sync on mobile

1. Enable **Auto sync** in MAGNETO settings.
2. Set an interval (5–120 minutes).

Notes:

- Auto-sync runs on a timer **only while Obsidian is open**.
- For large vaults, keep Obsidian in the foreground until the first sync completes.
- On Android, battery optimizations can interfere with long-running syncs. If syncs stop mid-way, try excluding Obsidian from battery optimization.

---

## What gets synced (and what does NOT)

- Synced: your vault files (notes, attachments, folders), except exclusions.
- Not synced by default: Obsidian plugin/theme/config folders (examples below).

Default excluded folders:

- `{{configDir}}/plugins`
- `{{configDir}}/themes`
- `.trash`

Default excluded files:

- `.DS_Store`
- `Thumbs.db`

---

## Troubleshooting

### “Connection failed” / 401 / 403

- Confirm the token is correct and not expired.
- Confirm token permissions match your token type (see [../README.md](../README.md#setup)):
  - Fine-grained PAT: Contents RW + Metadata RO (and restricted to the correct repo)
  - Classic PAT: `repo` scope
- If your GitHub account has SSO/SAML enforced (common in organizations), you may need to authorize the token for your organization.

### 404 / “Not Found”

- Check **GitHub username**.
- Check **Repository name** is only the name (not `owner/repo`).
- Check **Branch name** matches the repo.

### Sync seems slow or stuck on mobile

- Keep Obsidian open and the screen on during first sync.
- Try syncing on Wi‑Fi.
- Exclude very large folders (e.g., video/audio) if you don’t need them.

### I changed files on GitHub but they didn’t appear on my phone

- Run **Pull from GitHub**.
- Reminder: MAGNETO is designed to be **local-first** in practice; if the same path exists locally, your local copy may be uploaded again during sync.
  - Best practice: avoid editing the same note on two devices at the same time.

---

## Quick checklist (copy/paste)

- [ ] Install MAGNETO (Community plugins)
- [ ] Create GitHub token (see [../README.md](../README.md#setup))
- [ ] Configure username/token/repo/branch
- [ ] First device: Push or Sync (repo must exist if using fine-grained token)
- [ ] Second device: Pull first, then Sync
- [ ] Keep default exclusions for `{{configDir}}/plugins` to avoid leaking secrets
