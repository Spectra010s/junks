# junks

A lightweight trash bin CLI for Linux. Because `rm` doesn't forgive.

```
$ junks report.pdf notes.txt old_build/
junks: 'report.pdf' moved to trash  [id: 1716300000-4821]
junks: 'notes.txt' moved to trash   [id: 1716300000-9134]
junks: 'old_build' moved to trash   [id: 1716300000-3307]

$ junks list
Trash (~/.junks)  [3 item(s)]
──────────────────────────────────────────────────
📄  report.pdf                    2024-05-21 14:32
    240K        ← /home/user/report.pdf

📁  old_build                     2024-05-21 14:32
    47 items    ← /home/user/projects/old_build

$ junks restore report.pdf
junks: 'report.pdf' restored to '/home/user/report.pdf'
```

## Why

Linux has no recycle bin. `rm` is permanent. `junks` gives you a safety net — move files and directories to a local trash bin, restore them when you change your mind, or purge when you're sure.

There is a community standard — `trash-cli` — which implements the FreeDesktop Trash Spec and integrates with desktop file managers like Nautilus and Thunar. If you need that, use it.

`junks` is for everywhere else: servers, headless systems, and Termux on Android. No desktop dependencies. No spec overhead. Just bash, anywhere bash runs.

Built and maintained from Termux on Android.

## Install

```bash
git clone https://github.com/Spectra010s/junks.git
cd junks
chmod +x junks
sudo cp junks /usr/local/bin/
```

**Termux:**
```bash
cp junks $PREFIX/bin/
```

## Usage

```
junks <file|dir> [...]        Move one or more files/dirs to trash
junks restore <name>          Restore item to its original location
  --rename                    Auto-rename if restore path already exists
  --overwrite                 Overwrite if restore path already exists
  --to <dir>                  Restore into a specific directory
junks list [--plain]          List all items in the trash
junks purge                   Permanently delete all trash (confirms first)
junks purge <name>            Permanently delete a specific item
junks --version               Show version
junks --help                  Show help
```

## Examples

```bash
# Trash multiple targets at once
junks *.log old_folder/

# Restore to a different location
junks restore config.json --to ~/backup

# Restore even if name already exists
junks restore config.json --rename

# Plain ASCII list (safe for narrow/old terminals)
junks list --plain

# Purge a specific item
junks purge old_folder
```

## How it works

- Trash lives at `~/.junks/`
- Each item gets a unique ID (`timestamp-random`) to avoid collisions
- Metadata (`~/.junks/.meta/<id>`) stores original path and trash timestamp
- `restore` always puts things back where they came from
- `list` shows trashed-at time and original path for every item
- `--plain` mode auto-activates on non-UTF-8 terminals

## Custom trash location

```bash
export JUNKS_HOME=/path/to/custom/trash
```

Add to `~/.bashrc` or `~/.profile` to persist.

## License

MIT
