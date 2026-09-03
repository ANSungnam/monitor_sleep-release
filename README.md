# monitor_sleep public releases

`monitor_sleep` is a Windows x64 monitor-sleep and display-recovery service written in Rust.
This repository contains public binary releases only. The source repository is private.

## v0.00.01 notice

- This is a device-specific early release for advanced users.
- The executable is not code-signed, so Windows may show a SmartScreen warning.
- Runtime data and logs are currently fixed to `D:\monitor_sleep`.
- The service can change Windows power, USB sleep, brightness, and display-topology settings.
- The packaged executable does not use Python, PowerShell, batch files, or helper executables at runtime.
- `--update-service` is not supported by this public package. Uninstall the old service before installing a later version.

## Installation

1. Download `monitor_sleep_v0.00.01_windows_x64.exe` from the release page.
2. Create `D:\monitor_sleep` and place the executable in that directory.
3. Open PowerShell as Administrator and run:

```powershell
D:\monitor_sleep\monitor_sleep_v0.00.01_windows_x64.exe --install
```

Check the service:

```powershell
D:\monitor_sleep\monitor_sleep_v0.00.01_windows_x64.exe --status
```

Uninstall it:

```powershell
D:\monitor_sleep\monitor_sleep_v0.00.01_windows_x64.exe --uninstall
```

Verify the downloaded file against `SHA256SUMS.txt` before installation.

