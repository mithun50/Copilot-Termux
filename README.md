# 🤖 Copilot-Termux

![Platform](https://img.shields.io/badge/platform-Android%2FTermux-3DDC84?logo=android&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-blue)
![Shell](https://img.shields.io/badge/shell-bash-89e051?logo=gnubash&logoColor=white)

> **One-command installer to get GitHub Copilot CLI running natively on Android via Termux.**

---

## ✨ Features

- **node-pty** — Builds the pseudo-terminal native module from source using Android Clang
- **keytar** — Builds the secure credential storage module with libsecret integration
- **sharp** — Builds the image processing module against Termux's libvips
- **clipboard** — Pure-JS Termux API wrapper replacing the native clipboard module
- **ripgrep** — Symlinks the system `rg` binary to Copilot's expected path
- **Auto-patching** — Fixes `napi.h` enum handling incompatible with Android Clang
- **node-gyp config** — Creates `~/.gyp/include.gypi` to silence Android NDK path warnings
- **`set -u` safety** — All `npm` invocations are wrapped to prevent unbound-variable errors
- **Install log** — Full output written to `~/.copilot-termux-install.log`
- **Elapsed time** — Reports total installation time on completion

---

## 📋 Requirements

| Requirement | Notes |
|---|---|
| Android phone | ARM64 recommended; x64, ia32, ARM also supported |
| [Termux](https://f-droid.org/en/packages/com.termux/) | Install from **F-Droid**, not the Play Store |
| [Termux:API](https://f-droid.org/en/packages/com.termux.api/) | Companion app required for clipboard support |
| Node.js | Installed automatically by the script |
| Internet connection | Required during installation |

---

## 🚀 Installation

### One-liner (recommended)

```bash
curl -fsSL https://raw.githubusercontent.com/mithun50/Copilot-Termux/main/install.sh | bash
```

### Clone & run

```bash
git clone https://github.com/mithun50/Copilot-Termux.git
cd Copilot-Termux
chmod +x install.sh
./install.sh
```

The script is idempotent — it is safe to run multiple times.

---

## 💡 Usage

After installation completes:

```bash
# Launch the Copilot CLI
copilot

# Sign in with your GitHub account
/login

# Ask questions, get suggestions
/explain this function
/fix the bug on line 42
/tests for this module
```

---

## 🏗 Architecture

| Module | Purpose | Build method |
|---|---|---|
| `node-pty` | Pseudo-terminal — lets Copilot spawn and interact with shells | `npm install` + native `node-gyp` build |
| `keytar` | Secure credential storage for GitHub tokens | `npm install --ignore-scripts` + manual `npm run build` after napi.h patch |
| `sharp` | Image processing (panel renders, thumbnails) | `npm install` against Termux libvips |
| `clipboard` | Read/write system clipboard | Pure-JS wrapper around `termux-clipboard-get/set` |
| `ripgrep` | Fast code search used by Copilot internally | Symlink from `pkg install ripgrep` |

---

## 🔧 Troubleshooting

| Symptom | Fix |
|---|---|
| `keytar build fails` | Ensure `libsecret` is installed: `pkg install libsecret` |
| `node-pty fails` | Check clang version: `clang --version` (needs ≥ 14) |
| `sharp fails` | Usually non-fatal; Copilot still works without it |
| `TERMUX_VERSION not set` | Script must run **inside** Termux, not in a regular Linux shell |
| `Permission denied` | Run `chmod +x install.sh` before executing |
| `npm install fails` | Run `pkg update` first, then retry |
| `rg not found` | Run `pkg install ripgrep` manually |

See [`docs/TROUBLESHOOTING.md`](docs/TROUBLESHOOTING.md) for detailed fixes.

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feat/my-improvement`
3. Commit your changes with a clear message
4. Open a pull request

Please test on at least one Android device before submitting.

---

## 📄 License

MIT © [mithun50](https://github.com/mithun50)

---

<p align="center">Made with ❤️ for Android developers</p>
