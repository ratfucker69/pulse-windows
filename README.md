# Pulse for Windows

Unsigned portable builds of Pulse for Windows. Every release carries:

| Asset | What it is |
|---|---|
| `Pulse-<version>-windows-x64.zip` | the app: `Pulse.exe`, the bundled 64-bit LibVLC, no installer, no admin rights |
| `latest-windows.json` | the update feed the app checks on launch (`version`, `url`, `sha256`) |

## Install

1. Download the ZIP from the [latest release](../../releases/latest) and extract it anywhere you can write (for example `%LOCALAPPDATA%\Pulse`).
2. Run `Pulse.exe`.

**Windows will warn you.** The build is not code-signed â€” there is no certificate for this project â€” so SmartScreen shows "Windows protected your PC". Choose **More info â†’ Run anyway**. That warning is the expected cost of an unsigned build, not a sign that the download is wrong.

To check you got the file this repository published, compare the SHA-256 of the ZIP with the `sha256` field in `latest-windows.json`:

```powershell
Get-FileHash .\Pulse-0.1.0-windows-x64.zip -Algorithm SHA256
```

## Updates

The app checks `latest-windows.json` on launch, and offers to install anything newer from the tray menu. It downloads the package, verifies the SHA-256 from the feed, stages it, and swaps it in on restart â€” the running `.exe` is replaced by a helper script after the app exits.

What that proves and does not:

- **integrity** â€” the package matches the digest in the feed, so a truncated or corrupted download is refused;
- **authenticity** â€” nothing. The package is not signed. HTTPS trusted this host; it did not establish who built the binary.

If you need authenticity, build from source and run your own copy.

## Requirements

Windows 10/11 x64. Nothing else: the ZIP carries its own Java runtime and LibVLC, so you do not need Java, VLC, or admin rights installed.

## Notes

- Audio goes through libvlc; there is no DRM path, and this plays only what the underlying extractors can reach.
- Data (likes, playlists, play counts, logs) lives in `%LOCALAPPDATA%\Pulse`; deleting the folder resets the app, keeping the install intact.
