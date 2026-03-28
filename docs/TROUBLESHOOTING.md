# Troubleshooting

Common problems and how to fix them.

---

## keytar build fails

**Symptom:**
```
gyp: Call to 'pkg-config --cflags libsecret-1' returned exit status 1
```

**Fix:**
```bash
pkg install libsecret
export PKG_CONFIG_PATH="${PREFIX}/lib/pkgconfig:${PREFIX}/share/pkgconfig"
# Then re-run the installer
./install.sh
```

The installer sets `PKG_CONFIG_PATH` automatically, but if you are building manually you must export it first.

---

## node-pty fails to compile

**Symptom:**
```
error: use of undeclared identifier 'openpty'
```
or clang version errors.

**Fix:**
```bash
pkg install clang
clang --version   # should be 14 or higher
```

If clang is already installed at the wrong version:
```bash
pkg upgrade clang
```

---

## sharp fails to install

**Symptom:**
```
✗ Failed to install sharp
```

**Impact:** Non-fatal. Copilot CLI still works; only image-rendering features are affected.

**Fix (optional):**
```bash
pkg install libvips rust
npm install sharp
```

If Rust is outdated, update it:
```bash
pkg upgrade rust
```

---

## `set -u` unbound variable errors

**Symptom:**
```
/usr/bin/env: bash: unbound variable
```
or errors like `npm: unbound variable: NPM_CONFIG_...`

**What the installer does:** All `npm` calls in `install.sh` are already wrapped with `set +u` before and `set -u` after. If you are calling npm directly in a `set -u` shell, wrap your calls:

```bash
set +u
npm install something
set -u
```

---

## Permission denied when running install.sh

**Symptom:**
```
bash: ./install.sh: Permission denied
```

**Fix:**
```bash
chmod +x install.sh
./install.sh
```

---

## TERMUX_VERSION not set

**Symptom:**
```
Error: This setup script must be run inside Termux (TERMUX_VERSION not set). Exiting.
```

**Fix:** Make sure you are running the script **inside the Termux app**, not in a regular Linux terminal or SSH session that is not Termux. The `TERMUX_VERSION` environment variable is set automatically by Termux.

---

## npm install fails / network errors

**Symptom:**
```
npm ERR! network request to https://registry.npmjs.org failed
```

**Fix:**
```bash
# Update pkg first
pkg update && pkg upgrade

# Check connectivity
curl -I https://registry.npmjs.org

# Retry the installer
./install.sh
```

---

## rg (ripgrep) not found after install

**Symptom:** Copilot reports it cannot find `rg`, or:
```
⚠ No system rg found; Copilot may fail to use ripgrep
```

**Fix:**
```bash
pkg install ripgrep
which rg          # should output a path
./install.sh      # re-run to create the Copilot symlink
```

---

## Clipboard paste does nothing

**Symptom:** `/paste` in Copilot CLI returns empty string.

**Checks:**
1. Make sure the **Termux:API** companion app is installed (from F-Droid).
2. Grant Termux:API the clipboard permission in Android Settings → Apps → Termux:API → Permissions.
3. Test manually:
   ```bash
   termux-clipboard-get
   echo "hello" | termux-clipboard-set
   termux-clipboard-get   # should print "hello"
   ```

---

## Log file location

The full installation log is written to:
```
~/.copilot-termux-install.log
```

Attach this file when reporting issues.
