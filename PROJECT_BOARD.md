# Router Security Checker — PROJECT BOARD

> **Single source of project navigation.**
> 사람과 AI는 프로젝트 관련 작업을 시작하기 전에 반드시 이 파일을 확인한다.
> 이 문서는 "어떤 문서가 어디에 있으며, 현재 무엇이 승인되었고, 다음에 무엇을 해야 하는가"를 연결하는 중앙 게시판이다.

## Mandatory reading order

1. `AGENTS.md` — AI 작업 진입 규칙
2. `PROJECT_BOARD.md` — 문서 지도와 최신 결정
3. `NOTICE.md` — 팀 공통 운영 규칙
4. `STATUS.md` — 현재 Phase / Gate 상태
5. `TODO.md` — 실행 가능한 Task
6. 관련 역할 문서 — Research / Audit / Testing / Product
7. `Router_Security_Checker_PRD_v0.1.md` — 제품 기준선이 필요한 경우 확인

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

### Current gate meaning

현재 승인된 작업은 **실제 Archer C50 v6 수동 검사와 PHASE 0 조사**다.

`PHASE 1 — Discovery MVP` 개발은 아직 시작하지 않는다.

Phase 1 진입 전 최소 조건:

- Archer C50 v6 실제 수동 검사 완료
- 주요 검사 항목의 획득 가능 여부 기록
- 인증 필요 여부 기록
- 자동화 가능성 `YES / PARTIAL / NO` 분류
- 실패 시 Fallback 정의
- `requirements.md` 작성
- PHASE 1 Acceptance Criteria 작성
- Manager 재감사

---

## Document registry

| ID | Role / Type | Document | Repository path | Authority / Status | Use |
|---|---|---|---|---|---|
| PROD-001 | Product Baseline | Router Security Checker PRD v0.1 | `Router_Security_Checker_PRD_v0.1.md` | **Canonical baseline** | 제품 목적, 범위, roadmap 판단 기준 |
| OPS-001 | Team Rules | Project Notice | `NOTICE.md` | **Mandatory** | 역할·운영·범위 통제 규칙 |
| RES-PLAN-001 | Researcher | Internet Research PRD v0.1 | `docs/research/Router_Security_Checker_Internet_Research_PRD_v0.1.md` | **Active research plan** | PHASE 0 조사 순서·근거·산출물 기준 |
| AUD-001 | Manager | Project Progress Audit Report v0.1 | `docs/audit/Router_Security_Checker_Audit_Report_v0.1.md` | **Current audit decision** | PHASE 0 승인, PHASE 1 HOLD 및 Gate 조건 |
| TEST-001 | Testing | Archer C50 v6 Manual Inspection | `docs/testing/manual_test_001.md` | **Required / not completed** | 실제 장비 근거 확보 |
| REQ-001 | Product Requirements | Phase 0 derived requirements | `docs/product/requirements.md` | **Required / not created** | 검증된 조사 → 개발 Requirement 승격 |

---

## Role routing

### Researcher

작업 시작:

1. `PROJECT_BOARD.md`
2. `TODO.md`
3. `docs/research/Router_Security_Checker_Internet_Research_PRD_v0.1.md`
4. 관련 기존 조사 문서

산출물 기본 위치:

`docs/research/`

실제 장비 결과는:

`docs/testing/`

### Manager

작업 시작:

1. `PROJECT_BOARD.md`
2. `STATUS.md`
3. `TODO.md`
4. Product PRD
5. 관련 Research / Testing 산출물

산출물 기본 위치:

`docs/audit/`

Manager는 Phase Gate를 판정하며, 현재 결정은 **PHASE 1 HOLD**다.

### Developer

작업 시작:

1. `PROJECT_BOARD.md`
2. `STATUS.md`
3. `TODO.md`
4. Task가 참조하는 승인 Requirement / Test Result

개발자는 `TODO.md`에서 `READY` 또는 명시적으로 승인된 개발 Task만 구현한다.

산출물:

- Code → `src/`
- Unit tests → `tests/unit/`
- Integration tests → `tests/integration/`

### Project Lead

새로운 산출물이 들어오면:

`분류 → 올바른 폴더 배치 → PROJECT_BOARD 등록 → STATUS 반영 → TODO 반영 → 필요 시 Gate 변경`

순서로 처리한다.

---

## Latest decisions

### DEC-001 — 2026-08-08

Manager Audit 결과:

`CONDITIONAL PASS`

- Product direction: PASS
- Scope definition: PASS
- PHASE 0: PASS
- PHASE 1 development: HOLD

따라서 현재 개발자가 대규모 기능 구현을 시작하면 안 된다.

### DEC-002 — 2026-08-08

Researcher의 Internet Research PRD v0.1을 PHASE 0의 활성 조사 계획으로 등록한다.

Research 우선순위:

`DEVICE IDENTITY → FIRMWARE → VULNERABILITY → CONFIGURATION → NETWORK → TRAFFIC`

### DEC-003 — 2026-08-08

모든 AI 작업자는 루트 `AGENTS.md`를 통해 이 `PROJECT_BOARD.md`를 필수 진입점으로 사용한다.

---

## Current immediate work

### P0

- `TEST-001` — Archer C50 v6 실제 수동 검사
- `RESEARCH-001` — Archer C50 v6 Identity 조사

### Do not start yet

- PHASE 1 Discovery implementation
- GUI
- Traffic Analyzer
- External Scan Infrastructure
- Backdoor Detection
- Multi-vendor expansion

---

## Board update rule

다음 상황에서는 Project Lead가 이 파일을 업데이트한다.

- 새로운 Research / Audit / Test / Requirement 문서가 추가됨
- 문서 버전이 변경됨
- Manager가 Phase Gate를 변경함
- 주요 Task가 DONE 또는 BLOCKED로 변경됨
- canonical 문서가 교체됨

새 문서를 저장소에 추가했는데 `PROJECT_BOARD.md`에 등록되지 않았다면 **프로젝트에 공식 편입되지 않은 문서**로 취급한다.
