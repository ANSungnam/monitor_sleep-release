# monitor_sleep public releases

`monitor_sleep` is a Windows x64 monitor-sleep and display-recovery service written in Rust.
This repository contains public binary releases only. The source repository is private.

## v0.01.00 notice

- This is a device-specific early release for advanced users.
- The executable is not code-signed, so Windows may show a SmartScreen warning.
- Runtime settings, recovery markers, and logs are fixed to `D:\monitor_sleep_v0.01.00`.
- The service can change Windows power, USB sleep, brightness, and display-topology settings.
- The packaged executable does not use Python, PowerShell, batch files, or helper executables at runtime.
- `--update-service` is not supported by this public package layout. Uninstall the old service before installing a later version.

## Installation

1. Download `monitor_sleep_v0.01.00_windows_x64.exe` and `SHA256SUMS.txt` from the release page.
2. Create `D:\monitor_sleep_v0.01.00` and place the executable in that directory.
3. Verify the downloaded executable against `SHA256SUMS.txt`.
4. Open PowerShell as Administrator and run:

```powershell
D:\monitor_sleep_v0.01.00\monitor_sleep_v0.01.00_windows_x64.exe --install
```

Check the service:

```powershell
D:\monitor_sleep_v0.01.00\monitor_sleep_v0.01.00_windows_x64.exe --status
```

## System tray (`--tray`)

The installed Windows service normally starts the system-tray agent automatically in the signed-in user session. To start or restore it manually, run:

```powershell
cd D:\monitor_sleep_v0.01.00
.\monitor_sleep_v0.01.00_windows_x64.exe --tray
```

The command starts the tray agent without leaving a console window open and returns immediately to PowerShell. Only one tray agent runs in a user session, so repeated commands do not create duplicate icons.

Tray menu functions:

- Right-click the icon to pause or resume monitor control.
- Select an idle timeout of 1, 3, 5, 10, 15, 30, or 60 minutes.
- Double-click the icon to toggle pause and resume.
- Select `끝내기` to close the tray agent and monitor control. Windows, network, USB, and background-program sleep prevention remains active in the service.
- Run `.\monitor_sleep_v0.01.00_windows_x64.exe --tray` again to restore the tray agent after selecting `끝내기`.
- Restarting the service or Windows also restores the tray agent automatically.

## Uninstall

Open PowerShell as Administrator and run:

```powershell
D:\monitor_sleep_v0.01.00\monitor_sleep_v0.01.00_windows_x64.exe --uninstall
```

When upgrading from `v0.00.01`, uninstall the old service with the old executable before installing `v0.01.00`.

