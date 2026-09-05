# monitor_sleep public releases

`monitor_sleep` is a Windows x64 monitor-sleep and display-recovery service written in Rust.
This repository contains public binary releases only. The source repository is private.

## v0.02.02: tray recovery and monitor brightness control

- Retries failed initial tray creation every five seconds.
- Receives `TaskbarCreated` through a hidden top-level window and retries icon registration after Explorer restarts. Periodic registration checks also recover a missed notification.
- Enumerates physical monitors without manufacturer/model filters and requests minimum brightness through DDC/CI VCP 0x10, the Windows brightness API, and the internal LCD interface.
- Saves brightness before changing it, dims monitors before disconnecting secondary display signals, and restores each supported monitor after reconnecting. Failed restorations remain pending and are retried.
- Retains the existing Windows sleep-prevention behavior.

### Validation and limits

17 automated tests and one interactive Windows tray-recovery test passed, along with strict Clippy checks and a release build. The internal panel completed a measured `85 -> 0 -> 85` brightness cycle.

On the tested Samsung S24F350 and LG FULL HD connection, both DDC/CI methods returned Windows error 122. Their physical brightness was **not changed**. Monitor/connection support is required; minimum brightness (0%) is not a guarantee that the backlight is physically off. Unsupported or ambiguous device identities are logged and left unchanged.

### Default startup

As in the previous version, `monitor_sleep.exe` now defaults to `monitor_sleep.exe --tray`. Double-clicking the downloaded executable also starts the tray agent; no command-line option or service installation is required. Use `--help` to view commands.

## v0.02.02 notice

- This is a device-specific early release for advanced users.
- The executable is not code-signed, so Windows may show a SmartScreen warning.
- Runtime settings, recovery markers, and logs are fixed to `D:\monitor_sleep_v0.02.02`.
- The service can change Windows power, USB sleep, brightness, and display-topology settings.
- The packaged executable does not use Python, PowerShell, batch files, or helper executables at runtime.
- `--update-service` is not supported by this public package layout. Uninstall the old service before installing a later version.

## Installation

1. Download `monitor_sleep_v0.02.02_windows_x64.exe` and `SHA256SUMS.txt` from the release page.
2. Create `D:\monitor_sleep_v0.02.02` and place the executable in that directory.
3. Verify the downloaded executable against `SHA256SUMS.txt`.
4. Double-click the executable to start the system-tray agent.

### Optional automatic Windows service

For automatic startup as a Windows service, open PowerShell as Administrator and run:

```powershell
D:\monitor_sleep_v0.02.02\monitor_sleep_v0.02.02_windows_x64.exe --install
```

Check the service:

```powershell
D:\monitor_sleep_v0.02.02\monitor_sleep_v0.02.02_windows_x64.exe --status
```

## System tray (default, or `--tray`)

The installed Windows service normally starts the system-tray agent automatically in the signed-in user session. To start or restore it manually, run:

```powershell
cd D:\monitor_sleep_v0.02.02
.\monitor_sleep_v0.02.02_windows_x64.exe --tray
```

The command starts the tray agent without leaving a console window open and returns immediately to PowerShell. Only one tray agent runs in a user session, so repeated commands do not create duplicate icons.

Tray menu functions:

- The notification area uses the Windows monitor/desktop-PC icon.
- Right-click the icon to pause or resume monitor control.
- Select `즉시 적용` above `1분` to run the monitor-off and black-screen action
  immediately without changing the saved idle timeout.
- Select an idle timeout of 1, 3, 5, 10, 15, 30, or 60 minutes.
- Double-click the icon to toggle pause and resume.
- Select `끝내기` to close the tray agent and monitor control. If the Windows service is installed and running, its background sleep prevention remains active.
- Run `.\monitor_sleep_v0.02.02_windows_x64.exe --tray` again to restore the tray agent after selecting `끝내기`.
- Restarting the service or Windows also restores the tray agent automatically.

## Uninstall

Open PowerShell as Administrator and run:

```powershell
D:\monitor_sleep_v0.02.02\monitor_sleep_v0.02.02_windows_x64.exe --uninstall
```

When upgrading from an earlier version, uninstall the old service with its executable before installing `v0.02.02`.
