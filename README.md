# Groundwork Simulator Balance Editor

A small Python script that changes the starting balance in a **Groundwork Simulator** save file.

> **Unofficial.** This project is not affiliated with or endorsed by the game's developer. It was written against Early Access build **v0.1** and may stop working after an update. Use it on single-player saves only, and always keep a backup.

## Requirements

- Python 3.8 or newer ([python.org](https://www.python.org/downloads/), tick "Add python.exe to PATH" on Windows)
- The `msgpack` library

```
py -m pip install msgpack
```

On macOS/Linux, use `python3` and `pip` instead of `py -m pip`.

## Usage

1. **Close the game completely** and back up your save folder.
2. Put `edit_balance.py` next to your save file.
3. Run:

```
py edit_balance.py <input-save> <output-save> <new-balance>
```

Example:

```
py edit_balance.py slot-01.groundwork-simulator slot-01-new.groundwork-simulator 5000000
```

This writes a new file and leaves your original untouched.

4. Replace your original save with the new file (same file name as the original).
5. If you use Steam Cloud, turn it off in the game's settings (and in the game's Steam properties) before testing, or Steam may restore the old file.
6. Launch the game and load that slot.

## Troubleshooting

- **The game moved my save to a `quarantine` file:** the game rejected the edit. Restore your backup. This can happen if the game was updated and the save format changed. Please open an issue and include the game build version.
- **"Saved world with no catalog entry":** the save was placed in a slot the game doesn't list yet. Put the edited file in a slot that already has a save.
- **`ModuleNotFoundError: msgpack`:** run the install command in the Requirements section.

## How it works

A save file is a [MessagePack](https://msgpack.org/) document with these fields:

| Field | Contents |
|---|---|
| `magic`, `format_version`, `baseline` | Format header and build identifier |
| `checksum_sha256` | Integrity checksum (see below) |
| `metadata` | Display name, play time, site, and so on |
| `compressed_world_payload` | The zstd-compressed world |
| `extension_payloads` | Named blocks; `groundwork-simulator-campaign-v1` is plain JSON holding the company funds |

The balance is `funds.opening_balance_eur` inside the campaign JSON. The game verifies the checksum on load, so the script recomputes it:

```
SHA-256(
    compressed_world_payload
    + for each extension, sorted by name:
        len(name) as u64 little-endian + name
        + len(data) as u64 little-endian + data
)
```

The script edits the JSON, recomputes the checksum, and re-saves the file. Nothing else is modified.

## Limitations

- Only edits `opening_balance_eur`. If your save already has purchases, hires or gold sales recorded, your displayed balance is the opening balance adjusted by those entries.
- Tested on a single save from build v0.1. Other versions may differ.
- No support for multiplayer saves.

## Disclaimer

Editing save files can corrupt them. The author is not responsible for lost progress. Back up your saves before using this tool.

## License

Add a license of your choice (MIT is a common default for small tools like this).
