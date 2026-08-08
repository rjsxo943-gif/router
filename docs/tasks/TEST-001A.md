# TEST-001A — Archer C50 v6 Identity & Access Baseline

## Task Control

- Parent Task: `TEST-001 — Archer C50 v6 Manual Inspection`
- Phase: `PHASE 0 — Manual Research`
- Owner: `Researcher`
- Device Operator: `User / Project Lead`
- Status: `READY`
- Priority: `P0`
- Gate: `PHASE 0 PASS / PHASE 1 HOLD`
- Output: `docs/testing/manual_test_001.md` (Section A)

## Objective

실제 Test Device #001이 **TP-Link Archer C50 v6**임을 근거와 함께 확인하고, 이후 자동화 연구에 필요한 최소 네트워크/접근 Baseline을 확보한다.

이 Task에서는 Firmware 취약점 판정이나 보안 점수 계산을 하지 않는다.

## Why this is first

현재 조사 순서는 다음과 같다.

`DEVICE IDENTITY → FIRMWARE → VULNERABILITY → CONFIGURATION → NETWORK → TRAFFIC`

장비 식별이 틀리면 Firmware 및 CVE Applicability가 연쇄적으로 잘못될 수 있으므로 Device Identity를 먼저 고정한다.

## Required Inputs

실제 Archer C50 v6에 연결된 Windows PC와 공유기 접근 가능 상태.

가능하면 다음을 확보한다.

1. Windows 활성 네트워크 어댑터 정보
2. Default Gateway IPv4
3. Gateway MAC Address
4. 공유기 본체 라벨의 Model / Ver 표기
5. 관리자 페이지 접속 주소
6. 로그인 전 확인 가능한 정보
7. 로그인 필요 여부

비밀번호나 인증 토큰은 보고서에 기록하지 않는다.

## Exact Scope

이번 Task에서 확인할 항목은 아래 7개뿐이다.

| ID | Item | Required result |
|---|---|---|
| A1 | Default Gateway | IPv4 주소 |
| A2 | Gateway MAC | MAC 주소 |
| A3 | Vendor Evidence | TP-Link 여부 및 근거 |
| A4 | Model | Archer C50 여부 |
| A5 | Hardware Revision | V6 여부 |
| A6 | Admin Interface | HTTP/HTTPS 및 접속 주소 |
| A7 | Authentication | 인증 필요 여부 |

## Procedure

### Step 1 — Windows Network Baseline

Windows에서 현재 활성 Adapter와 Default Gateway를 확인한다.

권장 명령 후보:

```powershell
ipconfig
```

필요 시:

```powershell
Get-NetIPConfiguration
```

기록:

- Adapter name
- IPv4 address
- Default Gateway

### Step 2 — Gateway MAC

Gateway에 대한 ARP entry를 확인한다.

권장 명령 후보:

```powershell
arp -a
```

필요한 경우 Gateway와 한 번 통신한 뒤 다시 확인한다.

기록:

- Gateway IP
- Gateway MAC

### Step 3 — Physical Device Evidence

공유기 본체 라벨에서 다음을 확인한다.

- Manufacturer
- Model
- Hardware Version / Ver

민감 정보는 기록하지 않는다.

다음은 보고서에 넣지 않는다.

- Wi-Fi password
- Admin password
- Serial number 전체값 (필요하지 않으면 기록 금지)
- WPS PIN

### Step 4 — Admin Interface Access

Default Gateway를 브라우저에서 열어 확인한다.

기록:

- HTTP 접속 여부
- HTTPS 접속 여부
- Redirect 여부
- 로그인 화면 여부
- 로그인 전 모델 정보 노출 여부

### Step 5 — Evidence Classification

각 결과를 다음 중 하나로 분류한다.

- `TEST DEVICE RESULT`
- `SOURCE CLAIM`
- `UNKNOWN`

실제 장비에서 직접 확인한 값만 `TEST DEVICE RESULT`로 기록한다.

## Required Evidence Record

각 항목은 다음 형식을 사용한다.

```text
Item
A1 — Default Gateway

Method
Windows ipconfig

Result
<observed value>

Acquisition
AVAILABLE / UNAVAILABLE

Authentication
AUTH_NONE / AUTH_USER_REQUIRED / AUTH_MANUAL_ONLY / UNAVAILABLE

Automation
YES / PARTIAL / NO

Confidence
HIGH / MEDIUM / LOW

Evidence Type
TEST DEVICE RESULT / SOURCE CLAIM / UNKNOWN

Pain Point
<if any>

Fallback
<if needed>

Requirement Candidate
<only if supported>
```

## Acceptance Criteria

Task는 아래 조건을 모두 만족해야 `REVIEW`로 이동한다.

- [ ] Default Gateway IPv4가 기록됨
- [ ] Gateway MAC 획득 여부가 기록됨
- [ ] 제조사 TP-Link 여부의 근거가 기록됨
- [ ] Model = Archer C50 여부가 기록됨
- [ ] Hardware Revision = V6 여부가 기록됨
- [ ] 관리자 Interface의 HTTP/HTTPS 접근 결과가 기록됨
- [ ] 인증 필요 여부가 기록됨
- [ ] 각 결과에 Confidence가 있음
- [ ] 각 결과에 Automation 가능성이 있음
- [ ] 알 수 없는 값은 추정하지 않고 `UNKNOWN`으로 기록함
- [ ] 비밀번호/WPS PIN 등 비밀정보가 문서에 포함되지 않음

## Stop Conditions

아래 상황에서는 억지로 진행하지 않고 `BLOCKED`로 보고한다.

- 실제 Test Device가 Archer C50 v6가 아님
- 공유기 네트워크에 연결할 수 없음
- Default Gateway를 확인할 수 없음
- 관리자 페이지 접근이 불가능하고 원인을 확인할 수 없음

비밀번호 공격, Brute Force, 인증 우회는 금지한다.

## Out of Scope

이번 과제에서는 하지 않는다.

- 최신 Firmware 확인
- CVE 검색 및 매칭
- WPS/UPnP/DMZ/Port Forwarding 감사
- 포트 스캔
- Traffic Capture
- Risk Score 계산
- 개발 코드 작성

## Completion Report

완료 시 Researcher는 Project Lead에게 다음만 요약 보고한다.

```text
TASK: TEST-001A
STATUS: REVIEW / BLOCKED
DEVICE CONFIRMED: YES / NO / PARTIAL
GATEWAY: CONFIRMED / UNKNOWN
MODEL: CONFIRMED / UNKNOWN
HW REVISION: CONFIRMED / UNKNOWN
ADMIN ACCESS: AVAILABLE / PARTIAL / UNAVAILABLE
AUTH REQUIRED: YES / NO / UNKNOWN
AUTOMATION OUTLOOK: HIGH / MEDIUM / LOW
OUTPUT: docs/testing/manual_test_001.md
BLOCKERS: <none or list>
RECOMMENDED NEXT TASK: <one task only>
```

## What this unlocks

`TEST-001A = DONE`이면 다음 작업의 근거가 생긴다.

- `RESEARCH-001 — Archer C50 v6 Identity Research`의 실제 장비 검증
- `TEST-001B — Firmware Baseline` 발행 가능성 검토

`PHASE 1` 개발은 여전히 `HOLD` 상태를 유지한다.
