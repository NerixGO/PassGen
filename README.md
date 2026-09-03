# PassGen 🔐

A small, dependency-free Bash CLI that generates random passwords, right in
your terminal.

```
╔══════════════════════════════════════════════╗
║            Password Generator 🔐             ║
╚══════════════════════════════════════════════╝
```

## Features

- **Configurable length** — generate passwords of any length (default: 8)
- **Guaranteed character diversity** — every password includes at least one
  lowercase letter, one uppercase letter, and one digit (plus a symbol when
  `-A` is used), so you never get an all-lowercase or all-numeric result
- **Optional symbols** — throw in `!@#$%^&*()-_=+[]{};:,.?` with `-A`
- **Clipboard copy** — pipe the result straight to your clipboard with `-c`
- **Cross-platform clipboard support** — auto-detects `xclip` (X11),
  `wl-copy` (Wayland), or `pbcopy` (macOS)
- **Zero dependencies** — pure Bash + coreutils, no runtime or package
  manager required

## Installation

`passgen` is a single self-contained Bash script.

```bash
git clone https://github.com/NerixGO/PassGen.git
cd PassGen
chmod +x passgen
```

Optionally, put it on your `PATH` so you can call it from anywhere:

```bash
sudo cp passgen /usr/local/bin/passgen
```

## Usage

```
passgen [OPTIONS]
```

### Options

| Flag | Description |
|------|-------------|
| `-x <length>` | Set password length (default: `8`) |
| `-A` | Include symbols (`!@#$%^&*...`) |
| `-c` | Copy the generated password to the clipboard |
| `-h`, `--help` | Show the help message |

### Examples

```bash
./passgen                 # 8-character alphanumeric password
./passgen -x 16           # 16-character alphanumeric password
./passgen -x 24 -A        # 24 characters, letters + numbers + symbols
./passgen -x 20 -A -c     # generate, print, and copy to clipboard
```

## How it works

1. Builds a character pool from lowercase letters, uppercase letters, and
   digits — plus symbols when `-A` is passed.
2. Seeds the password with one guaranteed character from each required
   category, so the character-class mix isn't left to chance.
3. Fills the remaining length by sampling from the full pool.
4. Shuffles the final string (via `shuf`) so the guaranteed characters
   aren't always in the same position.
5. Optionally sends the result to the clipboard using whatever clipboard
   tool is available on your system.

## Requirements

- Bash
- Standard coreutils (`fold`, `shuf`, `head`, `tr`)
- *(Optional, for `-c`)* one of `xclip`, `wl-copy`, or `pbcopy`

## Disclaimer

`passgen` uses `shuf`, which is convenient but not a cryptographically
secure random source. For generating passwords protecting high-value
accounts or secrets, consider a dedicated password manager or a CSPRNG-based
tool.
