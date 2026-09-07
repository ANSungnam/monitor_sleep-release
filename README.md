# monitor_sleep

Rust로 작성된 Windows 10/11 x64 모니터 제어 프로그램의 공개 배포 저장소입니다. 소스는 비공개 저장소에서 관리합니다.

## 최신 버전: v0.03.04

[릴리즈 다운로드](https://github.com/ANSungnam/monitor_sleep-release/releases/tag/v0.03.04)

DPI 호환 처리, D: 고정 경로 제거, 정적 C 런타임 링크를 적용했습니다.
**Windows 11에서 빌드·실행을 확인했으며, Windows 10 실기기 검증은 아직 하지 않았습니다.**

## 설치

1. 릴리즈의 `monitor_sleep_v0.03.04_windows_x64.exe`와 `SHA256SUMS.txt`를 받습니다.
2. `C:\Tools\monitor_sleep_v0.03.04`처럼 계속 사용할 고정 로컬 폴더에 EXE를 저장합니다.
3. SHA-256을 비교한 뒤 EXE를 더블클릭하고 UAC를 승인합니다.

```powershell
Get-FileHash .\monitor_sleep_v0.03.04_windows_x64.exe -Algorithm SHA256
.\monitor_sleep_v0.03.04_windows_x64.exe --compatibility-check
```

같은 폴더에 `monitor_sleep.exe`가 생성되고 자동 시작 서비스와 트레이가 실행됩니다.
D: 드라이브는 필요하지 않습니다. 설치 후 폴더나 `monitor_sleep.exe`를 이동/삭제하지 마세요.
설정과 로그는 설치 폴더에 저장되므로 사용자와 서비스가 접근 가능한 고정 로컬 폴더를 사용합니다.
기존 서비스가 있으면 설치 과정에서 중지한 뒤 새 경로로 등록합니다.

## 트레이 및 주요 명령

트레이가 보이지 않으면 작업 표시줄의 숨겨진 아이콘 영역을 확인합니다.
우클릭 메뉴에서 `즉시 적용`, 대기 시간 변경, 일시정지/재개, `끝내기`를 사용할 수 있습니다.
`끝내기`는 화면 복원 후 UAC 승인으로 서비스를 중지/제거합니다. 파일과 설정은 남습니다.

```powershell
.\monitor_sleep.exe --version
.\monitor_sleep.exe --status
.\monitor_sleep.exe --tray
.\monitor_sleep.exe --uninstall
```

## 동작 및 제한

- 입력이 없으면 지원 모니터의 밝기를 낮추고 보조 화면 신호를 분리하며 기본 화면을 검정 창으로 가립니다.
- 사용자 입력 시 화면 구성과 지원 모니터의 밝기를 복원합니다.
- Windows 실행 유지 요청은 계속 유지하며 절전/최대 절전을 요청하지 않습니다.
- 모니터/연결 경로의 DDC/CI 지원이 필요합니다. 하드웨어 밝기 0이 백라이트의 완전한 전원 차단을 보장하지는 않습니다.
- 서비스는 USB 선택적 절전 해제를 요청합니다.
- 64비트 전용, 코드 서명 없음. Windows 10의 실제 설치/재부팅/밝기 복원은 미검증입니다.
- 특정 Surface/LG/Samsung 배치와 3화면 복구 설정은 배포에 포함하지 않습니다.

자동 테스트 27개 통과, 실패 0개, Explorer 통합 테스트 1개 제외.
자세한 변경 내용과 검증 범위는 [v0.03.04 릴리즈 설명](https://github.com/ANSungnam/monitor_sleep-release/releases/tag/v0.03.04)을 참고하세요.