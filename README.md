# MAGNETO — Markdown And Git Notes Export, Track Origin

Obsidian GitHub Sync Plugin

[![GitHub release](https://img.shields.io/github/v/release/vishal-go/magneto)](https://github.com/vishal-go/obsidian-magneto/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Sync your Obsidian vault to a GitHub repository. **Works on both mobile and desktop** without requiring Git to be installed locally.

## Features

- 🔄 **Full Sync**: Push local changes and pull remote changes
- ⬆️ **Push to GitHub**: Upload all local vault files to your repository
- ⬇️ **Pull from GitHub**: Download all files from your repository
- ⏰ **Auto Sync**: Optionally sync at regular intervals (5-120 minutes)
- 📱 **Mobile Compatible**: Uses GitHub REST API, no Git installation needed
- 🔒 **Private Repos**: Works with private repositories (auto-create is possible when your token allows it)
- 📁 **Exclusions**: Configure folders and files to exclude from sync
- 🖼️ **Binary Files**: Supports images, PDFs, and other binary files

## Installation

### From Community Plugins (Recommended)

1. Open **Settings → Community plugins**
2. Disable **Restricted mode**
3. Click **Browse** and search for "MAGNETO"
4. Click **Install**, then **Enable**

### Manual Installation

1. Download `main.js`, `manifest.json`, and `styles.css` from the [latest release](https://github.com/vishal-go/obsidian-magneto/releases)
2. Create folder: `<YourVault>/.obsidian/plugins/magneto/`
3. Copy the downloaded files into the folder
4. Reload Obsidian and enable the plugin

Note: the folder name stays `magneto` because the plugin id is still `magneto`.

## Setup

MAGNETO works with either:

- **Recommended (most secure):** a **private repo** + a **fine-grained PAT** restricted to that single repo
- **Alternative (easiest / auto-create repo):** a **classic PAT** with broad `repo` scope

### Recommended setup (private repo + restricted token)

#### 1) Create a private GitHub repository

You must create the repo first (fine-grained tokens can only be restricted to repos that already exist).

1. Go to https://github.com/new
2. Create a **Private** repository (example: `obsidian-vault`)
3. Optional (recommended): check **Add a README** so GitHub initializes the default branch (usually `main`)

#### 2) Generate a fine-grained token (restricted to that repo)

1. GitHub → **Settings** → **Developer settings**
2. **Personal access tokens** → **Fine-grained tokens**
3. Click **Generate new token**
4. Configure:
  - **Token name**: `Obsidian MAGNETO (restricted)`
  - **Expiration**: set a reasonable expiry
  - **Resource owner**: the account/org that owns the repo
5. Under **Repository access**:
  - Select **Only select repositories**
  - Choose your repo (example: `obsidian-vault`)
6. Under **Repository permissions**, set:
  - **Contents**: **Read and write**
  - **Metadata**: **Read-only**
7. Click **Generate token** and copy it immediately

#### 3) Configure MAGNETO in Obsidian

1. Obsidian → **Settings** → **Community plugins** → **MAGNETO**
2. Set:
  - **GitHub username**: your GitHub username
  - **Personal access token**: paste the fine-grained token
  - **Repository name**: repo name only (example: `obsidian-vault`, not `owner/obsidian-vault`)
  - **Branch**: usually `main`
3. Click **Test connection**

### Alternative setup (classic PAT with `repo` scope)

Use this if you want MAGNETO to potentially auto-create the repo during **Push/Sync**, or if fine-grained tokens aren’t an option.

1. Go to https://github.com/settings/tokens
2. **Tokens (classic)** → **Generate new token (classic)**
3. Select scope: **`repo`**
4. Copy the token immediately

Then configure MAGNETO the same way in Obsidian settings.

### Start syncing

- Use the **ribbon icon** (git-branch icon) for quick sync
- Or use the **Command Palette** (Ctrl/Cmd + P):
  - `MAGNETO: Push to GitHub`
  - `MAGNETO: Pull from GitHub`
  - `MAGNETO: Sync with GitHub`
- Or use the buttons in the settings tab

## Settings

| Setting | Description |
|---------|-------------|
| **GitHub Username** | Your GitHub username |
| **Personal Access Token** | Recommended: fine-grained token restricted to your repo (Contents RW + Metadata RO). Classic PAT with `repo` scope also works. |
| **Repository Name** | Repo name only (fine-grained tokens require the repo to exist already) |
| **Branch** | Git branch to use (default: `main`) |
| **Auto Sync** | Enable automatic syncing |
| **Auto Sync Interval** | How often to auto-sync (5-120 minutes) |
| **Commit Message** | Template with `{{date}}` placeholder |
| **Excluded Folders** | Folders to skip (one per line) |
| **Excluded Files** | Files to skip (one per line) |

## Default Exclusions

By default, these folders are excluded:
- `.obsidian/plugins` - Plugin files (too large, use plugin manager)
- `.obsidian/themes` - Theme files
- `.trash` - Deleted files

## How It Works

This plugin uses the **GitHub REST API** to sync files. Unlike traditional Git sync plugins that require Git to be installed, MAGNETO:

1. Reads all files in your vault
2. Uploads them to GitHub using the Git Data API (efficient batch uploads)
3. Downloads any remote-only files to your vault

This makes it perfect for **Obsidian Mobile** where Git isn't available.

## Commands

| Command | Description |
|---------|-------------|
| `Push to GitHub` | Upload all local files to GitHub |
| `Pull from GitHub` | Download all files from GitHub |
| `Sync with GitHub` | Push first, then pull (full sync) |

## Troubleshooting

### "Connection failed"
- Verify your GitHub username is correct
- Ensure your token has the right permissions:
  - Fine-grained PAT: Contents RW + Metadata RO (and restricted to the correct repo)
  - Classic PAT: `repo` scope
- Check that the token hasn't expired

### Files not syncing
- Check the **Excluded Folders** and **Excluded Files** settings
- Make sure the file isn't in `.obsidian/plugins` (excluded by default)

### Large vaults
- The plugin uses batch uploads for efficiency
- Very large vaults (10,000+ files) may take a few minutes for the first sync

## Development

```bash
# Install dependencies
npm install

# Build for development (watch mode)
npm run dev

# Build for production
npm run build

# Run linter
npm run lint
```

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed development guidelines.

## Support

- 🐛 [Report a bug](https://github.com/vishal-go/obsidian-magneto/issues/new?template=bug_report.md)
- 💡 [Request a feature](https://github.com/vishal-go/obsidian-magneto/issues/new?template=feature_request.md)
- 📖 [Documentation](https://github.com/vishal-go/obsidian-magneto#readme)

## Author

**Vishal Sharma**
- GitHub: [@vishal-go](https://github.com/vishal-go)
- Email: sharma39vishal@gmail.com

## License

MIT License - see [LICENSE](LICENSE) for details.
