# Installation Guide

This guide walks you through installing GitHub Copilot CLI on Android using Termux.

---

## Prerequisites

| Requirement | Where to get it |
|---|---|
| Android phone (5.0+) | — |
| Termux | [F-Droid](https://f-droid.org/en/packages/com.termux/) — **not** the Play Store version |
| Termux:API app | [F-Droid](https://f-droid.org/en/packages/com.termux.api/) — required for clipboard |
| Active internet connection | Required during installation only |

> ⚠️ Do **not** use the Termux version from the Google Play Store — it is outdated and missing features required by this installer.

---

## One-liner Installation

Open Termux and run:

```bash
curl -fsSL https://raw.githubusercontent.com/mithun50/Copilot-Termux/main/install.sh | bash
```

This single command downloads and executes the installer. Everything else is automated.

---

## Manual Installation

If you prefer to inspect the script before running it:

```bash
# 1. Clone the repository
git clone https://github.com/mithun50/Copilot-Termux.git
cd Copilot-Termux

# 2. Make the script executable
chmod +x install.sh

# 3. Run the installer
./install.sh
```

---

## What Each Step Does

| Step | What happens |
|---|---|
| **[0/11]** Update & Node.js | Runs `pkg update`, installs nodejs, clang, make, python; creates `~/.gyp/include.gypi` |
| **[1/11]** Copilot CLI | Installs `@github/copilot` globally via npm |
| **[2/11]** System deps | Installs glib, xorgproto, rust, libvips, pkg-config, ripgrep, libsecret; sets `PKG_CONFIG_PATH` |
| **[3/11]** node-pty | Builds the pseudo-terminal native addon from source |
| **[4/11]** node-addon-api + keytar | Downloads keytar without running its build scripts |
| **[5/11]** Patch napi.h | Replaces `static_cast<napi_typedarray_type>(-1)` with `napi_uint8_array` for Clang compat |
| **[6/11]** sharp | Builds image processing module (non-fatal if it fails) |
| **[7/11]** Build keytar | Runs `npm run build` inside `node_modules/keytar` with the patched headers |
| **[8/11]** termux-api | Installs the `termux-api` package providing `termux-clipboard-get/set` |
| **[9/11]** Clipboard wrapper | Writes `clipboard/index.cjs` and `clipboard/android-impl.cjs` to the Copilot install dir |
| **[10/11]** Symlink binaries | Symlinks `keytar.node` and `pty.node` into `prebuilds/<arch>/` |
| **[11/11]** Verify | Loads each module in Node.js and reports pass/fail |

---

## Expected Output

A successful run ends with something like:

```
✓ keytar loaded successfully
✓ node-pty loaded successfully
✓ sharp loaded successfully
✓ clipboard wrapper available

Results: 4 passed, 0 failed

Modules fixed: 8  |  Modules failed: 0
Elapsed: 4 minutes 32 seconds
```

---

## Post-install Steps

```bash
# Start GitHub Copilot CLI
copilot

# Authenticate with your GitHub account
/login

# Follow the browser link to complete OAuth
```

Once authenticated, your token is stored securely via keytar/libsecret and persists across sessions.

---

## Updating

To update the installer itself, pull the latest version and re-run:

```bash
cd Copilot-Termux
git pull
./install.sh
```

The script is idempotent — re-running it is safe and will skip already-installed components.
