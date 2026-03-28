# Changelog

All notable changes to Copilot-Termux will be documented in this file.

---

## [1.0.0] - 2026-03-28

### Added
- Initial release of Copilot-Termux installer
- Support for arm64, x64, ia32, arm architectures
- node-pty native build for pseudo-terminal support
- keytar native build for secure credential storage
- sharp image processing module
- Termux clipboard API wrapper (ClipboardManager + ClipboardListener)
- Automatic ripgrep setup and symlink to Copilot expected path
- node-gyp Android compatibility patch (`~/.gyp/include.gypi`)
- `napi.h` enum patch for Android Clang compatibility
- libsecret integration for keytar
- Installation log at `~/.copilot-termux-install.log`
- Elapsed time tracking and final summary display
- ASCII art banner with cyan colour
- `SCRIPT_VERSION` variable for version tracking
- `log_success` / `log_error` helpers (non-counting) separate from module-level `print_success` / `print_error`
- `Modules fixed: X | Modules failed: Y` summary at end of run

### Fixed
- `set -u` unbound-variable errors caused by npm's internal use of unset variables — all `npm` calls are now wrapped with `set +u` / `set -u`
- Duplicate `files: null` key in `ClipboardListener` clipboardData object
- Step counter labels corrected from `X/10` to `X/11` (11 steps total)
- `PKG_CONFIG_PATH` export moved before `ensure_pkg libsecret` call so pkg-config can locate `libsecret-1.pc` during install
- sharp build failure now counted in `MODULES_FAILED` via `print_error` (was previously silent)
