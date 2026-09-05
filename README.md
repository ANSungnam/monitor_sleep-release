# monitor_sleep public releases

Windows x64 monitor and display control service written in Rust. This repository contains public binary releases; the source repository remains private.

## v0.02.03: install on double-click, uninstall on Exit

- Double-clicking the EXE (no arguments) installs and starts the automatic Windows service. The service starts one tray agent in the signed-in session.
- Approve the Windows UAC prompt when administrator access is required.
- Selecting `끝내기` in the tray restores the display state and runs an administrator helper to stop and remove the service. Cancelling UAC keeps the tray running.
- The removal helper runs asynchronously so the tray can process its service-stop event without a circular wait.
- Explicit `--tray` still starts only the tray agent. `--uninstall` requests elevation when needed, accepts an already absent service, and reports a failure if the service cannot stop.
- Tray icon retries and monitor brightness recovery from v0.02.02 are retained.

## Download and run

1. Download `monitor_sleep_v0.02.03_windows_x64.exe` and `SHA256SUMS.txt` from [v0.02.03](https://github.com/ANSungnam/monitor_sleep-release/releases/tag/v0.02.03).
2. Create `D:\monitor_sleep_v0.02.03` and place the EXE there. Settings, logs, and recovery paths are fixed to this directory.
3. Verify its SHA-256 against `SHA256SUMS.txt`.
4. Double-click the EXE and approve UAC. It installs/starts the service and tray together.

When upgrading, close any standalone older tray agent first. The new default startup stops an existing service before registering and starting the new executable. Files and saved settings are retained.

```powershell
& 'D:\monitor_sleep_v0.02.03\monitor_sleep_v0.02.03_windows_x64.exe' --status
```

## Tray and removal

Right-click the icon to pause/resume monitor control, use `즉시 적용`, choose the idle timeout, or select `끝내기`.
Pausing preserves background sleep prevention. **Exit now stops and removes the service**, ending this program's background sleep-prevention requests.
Application files and settings are not deleted. To install/start again, double-click the EXE.

Alternatively, run:

```powershell
& 'D:\monitor_sleep_v0.02.03\monitor_sleep_v0.02.03_windows_x64.exe' --uninstall
```

`--install` explicitly installs/starts the service. `--tray` explicitly launches only the agent. `--update-service` is not supported by this public package layout.

## Validation and limits

- All 18 tests passed, including the Windows notification-area icon recovery test; strict Clippy and the release build passed.
- The changed lifecycle was tested against an actual Windows service: default startup with one agent, removal through the same helper entry point used by Tray Exit, repeated removal when already absent, and reinstallation.
- The final v0.02.03 was started with no arguments from a normal user process and verified as Running/Auto with one session agent and successful tray initialization.
- Actual mouse selection of Tray Exit and cancellation of UAC were not separately exercised.
- Previously tested internal-panel brightness completed `85 -> 0 -> 85`. The tested Samsung S24F350 and LG FULL HD connections returned DDC/CI error 122; their physical brightness was not changed. This release does not establish brightness support for all monitors.
- 0% means the device-reported minimum, not guaranteed physical backlight shutdown.

This is an unsigned, device-specific early release. It can change power, USB sleep, brightness, and display-topology settings. The program uses native Windows APIs and does not require a Python/PowerShell runtime or helper scripts.
