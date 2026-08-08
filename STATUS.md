# Project Status

> 이 파일은 프로젝트의 현재 위치만 짧고 명확하게 보여주는 상황판이다.
> 문서 위치와 최신 결정의 상세 내용은 `PROJECT_BOARD.md`를 먼저 확인한다.

## Current

- Product target: `v0.1`
- Stage: `PHASE 0 — Manual Research`
- Platform: `Windows`
- Initial vendor: `TP-Link`
- Test device: `TP-Link Archer C50 v6`
- Overall audit: `CONDITIONAL PASS`
- PHASE 0 entry: `PASS`
- PHASE 1 development: `HOLD`
- Overall state: `IN PROGRESS`

## Active execution sequence

**Sequence 001 — Device Identity & Access Baseline**

| Role | Switch | Current assignment |
|---|---|---|
| Researcher | `ON / READY` | `TEST-001A` 수행 |
| Manager | `OFF / WAIT` | TEST-001A 결과 전까지 Gate 재감사 대기 |
| Developer | `OFF / HOLD` | PHASE 1 Gate 승인 전 개발 금지 |
| Project Lead | `ON` | 결과 수집, 문서 등록, 다음 Task 결정 |

Work Order:
`docs/tasks/TEST-001A.md`

## Registered governing documents

| Type | Document | Status |
|---|---|---|
| Product baseline | `Router_Security_Checker_PRD_v0.1.md` | ACTIVE |
| Research plan | `docs/research/Router_Security_Checker_Internet_Research_PRD_v0.1.md` | ACTIVE |
| Manager audit | `docs/audit/Router_Security_Checker_Audit_Report_v0.1.md` | CURRENT |
| Project board | `PROJECT_BOARD.md` | MANDATORY |
| AI entry rules | `AGENTS.md` | MANDATORY |
| First work order | `docs/tasks/TEST-001A.md` | READY |

## Workstream status

| Workstream | Status | Note |
|---|---|---|
| Product definition | DONE | PRD v0.1 기준선 존재 |
| Team operating rules | DONE | NOTICE.md 적용 |
| Project document routing | DONE | PROJECT_BOARD.md / AGENTS.md 적용 |
| PHASE 0 research plan | DONE | Research PRD v0.1 등록 |
| Manual device inspection | READY | TEST-001A부터 순차 수행 |
| Router discovery research | READY | Identity 조사부터 수행 |
| Router identification | BLOCKED | 실제 장비 근거 필요 |
| Firmware intelligence | BLOCKED | Identity / 실제 펌웨어 정보 필요 |
| Configuration audit | BLOCKED | 수동 검사 결과 필요 |
| Risk engine | BLOCKED | SAFE/UNKNOWN/Coverage 규칙 미정 |
| PHASE 1 implementation | HOLD | Manager 재승인 전 시작 금지 |
| GUI | BACKLOG | 현재 우선순위 아님 |
| Traffic analysis | BACKLOG | feasibility research only |

## Current blockers

Manager Audit 기준으로 PHASE 1 진입 전 다음이 필요하다.

- Archer C50 v6 실제 수동 검사
- 검사 항목별 획득 가능 여부
- 인증 요구사항 (`AUTH_NONE / AUTH_USER_REQUIRED / AUTH_MANUAL_ONLY / UNAVAILABLE`)
- 자동화 수준과 신뢰도
- 실패 시 Fallback
- `docs/product/requirements.md`
- PHASE 1 Acceptance Criteria
- Manager 재감사

## Immediate milestone

`TEST-001A — Archer C50 v6 Identity & Access Baseline`을 완료하여 다음을 확정한다.

- Default Gateway
- Gateway MAC
- Vendor evidence
- Model
- Hardware Revision
- Admin interface
- Authentication requirement

그 뒤 Project Lead가 `TEST-001B` 또는 보완 Task 발행 여부를 판단한다.

## Update rule

중요한 Task가 완료되거나 Phase/Gate가 바뀌거나 새 공식 문서가 등록될 때 Project Lead가 이 파일과 `PROJECT_BOARD.md`를 함께 갱신한다.
