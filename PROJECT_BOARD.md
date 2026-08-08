# Router Security Checker — PROJECT BOARD

> **Single source of project navigation.**
> 사람과 AI는 프로젝트 관련 작업을 시작하기 전에 반드시 이 파일을 확인한다.
> 이 문서는 "어떤 문서가 어디에 있으며, 현재 무엇이 승인되었고, 다음에 무엇을 해야 하는가"를 연결하는 중앙 게시판이다.

## Mandatory reading order

1. `AGENTS.md`
2. `PROJECT_BOARD.md`
3. `NOTICE.md`
4. `STATUS.md`
5. `TODO.md`
6. 현재 실행 중인 `docs/tasks/` Work Order
7. 관련 역할 문서
8. 필요 시 `Router_Security_Checker_PRD_v0.1.md`

> `TODO.md`에 없는 개발 기능을 임의로 구현하지 않는다.
> 조사 문서의 내용은 검증·승인 전까지 개발 Requirement로 자동 승격되지 않는다.

---

## Current project gate

| Item | Current state |
|---|---|
| Target | `v0.1` |
| Phase | `PHASE 0 — Manual Research` |
| Test device | `TP-Link Archer C50 v6` |
| Product PRD | `PASS` |
| Phase 0 entry | `PASS` |
| Phase 1 development | `HOLD` |
| Overall audit | `CONDITIONAL PASS` |

## ACTIVE SEQUENCE — 001

### TEST-001A — Archer C50 v6 Identity & Access Baseline

- Status: `READY`
- Owner: `Researcher`
- Device Operator: `User / Project Lead`
- Work Order: `docs/tasks/TEST-001A.md`
- Output: `docs/testing/manual_test_001.md` Section A

### Role switches

| Role | Switch | Meaning |
|---|---|---|
| Researcher | `ON / READY` | 첫 과제 수행 가능 |
| Manager | `OFF / WAIT` | 결과 제출 전 재감사하지 않음 |
| Developer | `OFF / HOLD` | PHASE 1 Gate 전 코드 개발 금지 |
| Project Lead | `ON` | 결과 수집 및 다음 Sequence 결정 |

이번 Sequence의 완료 조건은 장비의 Default Gateway, Gateway MAC, Vendor, Model, Hardware Revision, 관리자 Interface, 인증 요구사항을 실제 장비 근거로 기록하는 것이다.

---

## Document registry

| ID | Role / Type | Document | Repository path | Authority / Status | Use |
|---|---|---|---|---|---|
| PROD-001 | Product Baseline | Router Security Checker PRD v0.1 | `Router_Security_Checker_PRD_v0.1.md` | **Canonical baseline** | 제품 목적, 범위, roadmap 판단 기준 |
| OPS-001 | Team Rules | Project Notice | `NOTICE.md` | **Mandatory** | 역할·운영·범위 통제 규칙 |
| RES-PLAN-001 | Researcher | Internet Research PRD v0.1 | `docs/research/Router_Security_Checker_Internet_Research_PRD_v0.1.md` | **Active research plan** | PHASE 0 조사 기준 |
| AUD-001 | Manager | Project Progress Audit Report v0.1 | `docs/audit/Router_Security_Checker_Audit_Report_v0.1.md` | **Current audit decision** | PHASE 0 승인, PHASE 1 HOLD |
| TASK-001 | Work Order | TEST-001A Identity & Access Baseline | `docs/tasks/TEST-001A.md` | **READY / ACTIVE SEQUENCE** | 첫 실행 과제 |
| TEST-001 | Testing | Archer C50 v6 Manual Inspection | `docs/testing/manual_test_001.md` | **Required / not completed** | 실제 장비 근거 |
| REQ-001 | Product Requirements | Phase 0 derived requirements | `docs/product/requirements.md` | **Required / not created** | 검증된 조사 → 개발 Requirement |

---

## Role routing

### Researcher

작업 시작:

1. `PROJECT_BOARD.md`
2. `TODO.md`
3. 현재 `docs/tasks/` Work Order
4. `docs/research/Router_Security_Checker_Internet_Research_PRD_v0.1.md`

현재 Task:
`TEST-001A`

### Manager

현재 상태: `WAIT`

TEST-001A 결과만으로 PHASE 1을 승인하지 않는다. 전체 Gate 조건이 충족되었을 때 재감사한다.

### Developer

현재 상태: `HOLD`

`DEV-001`을 포함한 PHASE 1 구현은 Manager Gate 승인 전 시작하지 않는다.

### Project Lead

새로운 결과가 들어오면:

`결과 확인 → 저장 위치 결정 → PROJECT_BOARD 등록 → STATUS/TODO 갱신 → 다음 Task ON/OFF 결정`

---

## Latest decisions

### DEC-001 — 2026-08-08

Manager Audit: `CONDITIONAL PASS`

- PHASE 0: PASS
- PHASE 1 development: HOLD

### DEC-002 — 2026-08-08

Research 순서:

`DEVICE IDENTITY → FIRMWARE → VULNERABILITY → CONFIGURATION → NETWORK → TRAFFIC`

### DEC-003 — 2026-08-08

모든 AI 작업자는 `AGENTS.md → PROJECT_BOARD.md`를 필수 진입점으로 사용한다.

### DEC-004 — 2026-08-08

첫 실행 과제를 `TEST-001A — Archer C50 v6 Identity & Access Baseline`으로 발행한다.

전체 TEST-001을 한 번에 수행하지 않고 작은 Work Order로 순차 분해한다.

이유:

- 잘못된 Device Identity가 뒤의 Firmware/CVE 판단을 오염시키는 것을 방지
- 결과 단위별 Review 가능
- 각 역할의 ON/OFF 조건을 명확히 관리

---

## Do not start yet

- Firmware/CVE 판정
- Configuration 전체 감사
- PHASE 1 Discovery implementation
- GUI
- Traffic Analyzer implementation
- External Scan Infrastructure
- Backdoor Detection
- Multi-vendor expansion

---

## Board update rule

다음 상황에서는 Project Lead가 이 파일을 업데이트한다.

- Work Order 생성/완료/차단
- 새로운 Research / Audit / Test / Requirement 문서 추가
- Manager가 Phase Gate 변경
- 주요 Task 상태 변경
- canonical 문서 교체

새 문서나 Task가 저장소에 존재해도 `PROJECT_BOARD.md`에 등록되지 않았다면 **프로젝트에 공식 편입되지 않은 항목**으로 취급한다.
