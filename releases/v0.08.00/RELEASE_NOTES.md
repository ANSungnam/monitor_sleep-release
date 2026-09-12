# Monitor Sleep v0.08.00

## 주요 동작

- 5분 동안 입력이 없으면 내장 화면을 검정 화면과 밝기 0으로 전환합니다.
- 외부 LG·Samsung 모니터에는 밝기나 절전 명령을 보내지 않고 Windows 표시 경로의 신호만 분리합니다.
- 입력이 돌아오면 안정적인 모니터 장치 경로, 위치, 해상도와 회전을 기준으로 독립 3경로를 복구하고 내장 화면을 저장한 밝기로 되돌립니다.
- 복구 snapshot과 journal을 원자적으로 저장하며, 복구가 남은 동안 서비스 watchdog이 STOP_PENDING 상태와 복구 책임을 유지합니다.
- LocalSystem 자동 서비스가 Windows 시스템 절전을 막고 네트워크·USB 유지 요청을 계속 보냅니다.
- 트레이에서 모니터 제어 일시정지·재개, 유휴 시간 변경과 즉시 적용을 사용할 수 있습니다.

## 경로

- 설치·실행: `D:\monitor_sleep_v0.08.00\monitor_sleep.exe`
- 기본 구버전 설정 이전: `D:\monitor_sleep_v0.07.01`
- 현재 서비스가 다른 구버전 위치에 있으면 Service Control Manager의 실제 경로를 읽기 전용 이전 원본으로 사용합니다.

## 검증

- `cargo fmt --all -- --check`: 통과
- `cargo check --offline --locked`: 통과
- `cargo test --offline --locked`: 39 통과, 실패 0, 실환경 의존 3개 제외
- `cargo clippy --offline --locked --all-targets -- -D warnings`: 통과
- `cargo build --release --offline --locked`: 통과

실제 v0.08.00 서비스 설치, 실물 3모니터 유휴·복구, 인터넷·USB 연속성, 로그오프·종료·재부팅과 설치 rollback은 아직 수행하지 않았습니다.

## 파일

- `monitor_sleep_v0.08.00_windows_x64.exe`
- `SHA256SUMS.txt`

SHA-256: `0FC32840228BDCB8E0C45DD77A61832A7914446D4EC32AA7D38F71AFAC2FF1D4`

이 EXE는 Authenticode 서명이 없습니다. 이 Release에서 직접 내려받고 동봉한 `SHA256SUMS.txt`로 무결성을 확인하세요.
