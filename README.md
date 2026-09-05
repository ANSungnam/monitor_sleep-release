# monitor_sleep public releases

Windows x64 monitor and display control service written in Rust. This repository contains public binaries; the source repository remains private.

## v0.03.00

- Verify hardware brightness zero through DDC/CI read-back. Correct nonstandard write/read scales for the tested ZEUSLAP-TYPEC / BOE2556 panel and restore the original value after screen control.
- Double-click the release EXE, approve UAC, and the program creates its runtime directory, installs/starts the automatic Windows service, and waits for its user-session tray agent to become ready.
- **The PC remains running. The program does not request Windows sleep or hibernation.** Existing system/execution power requests and USB sleep prevention remain active.
- Explicit `--install` and `--tray` remain available. Tray Exit restores displays and stops/removes the service after UAC approval.

## Download and run

1. Download `monitor_sleep_v0.03.00_windows_x64.exe` and `SHA256SUMS.txt` from [v0.03.00](https://github.com/ANSungnam/monitor_sleep-release/releases/tag/v0.03.00).
2. Verify the executable SHA-256 against `SHA256SUMS.txt`.
3. Double-click the EXE and approve Windows UAC.

Installation and runtime files use `D:\monitor_sleep_v0.03.00`; a D: drive is required. The installed executable is `D:\monitor_sleep_v0.03.00\monitor_sleep.exe`. The downloaded file can be moved after installation. Close independently launched older tray agents before upgrading if they hold the shared agent mutex.

## Tray visibility and controls

Windows can hide a registered icon in the overflow area. Open **Settings > Personalization > Taskbar > Other system tray icons** and enable the current `monitor_sleep.exe` entry. Older versions may have separate entries with the same name. This visibility setting is separate from service installation and tray registration.

Right-click the icon to pause/resume monitor control, select `즉시 적용`, change the idle timeout, or select `끝내기`. Pausing retains background sleep prevention. Exit removes the service and ends this program's power requests; files and settings remain. Double-click the release EXE to install/start again.

## Validation and limits

- 19 automated tests passed; one Explorer integration test was skipped.
- Physical brightness read-back on the tested ZEUSLAP/BOE2556 monitor verified `34 -> 0 -> 34`.
- The actual black-overlay activation path produced five consecutive hardware samples of zero and restored brightness to 34.
- No-argument release execution was verified with a running automatic service and a ready service-owned tray agent.
- Hardware zero requires monitor/connection DDC/CI support. Zero is the device-reported minimum and does not guarantee that the backlight is physically off.
- This is an unsigned, device-specific early release. Existing optional Surface/LG/Samsung startup-layout presets must match the intended display setup. The program changes power, USB, brightness, and display-topology settings using native Windows APIs.
