# Router Security Checker — TODO

> 모든 실제 작업은 이 파일의 Task를 기준으로 수행한다.
> 작업 전에 반드시 `PROJECT_BOARD.md`에서 현재 Gate와 관련 문서 위치를 확인한다.
> 조사 결과나 아이디어가 곧바로 개발 Task가 되지 않는다.

## Current gate

- `PHASE 0 — Manual Research`: ACTIVE
- `PHASE 1 — Discovery MVP Development`: HOLD
- Manager decision: `CONDITIONAL PASS`

Reference:
`docs/audit/Router_Security_Checker_Audit_Report_v0.1.md`

## Status legend

`BACKLOG` → `READY` → `IN PROGRESS` → `REVIEW` → `DONE`

필요 시 `BLOCKED` 또는 Phase 수준의 `HOLD`를 사용한다.

---

# SEQUENCE 001 — ACTIVE

## TEST-001A — Archer C50 v6 Identity & Access Baseline

- Parent: `TEST-001`
- Owner: Researcher
- Device Operator: User / Project Lead
- Status: `READY`
- Priority: `P0-FIRST`
- Blocked By: None
- Work Order: `docs/tasks/TEST-001A.md`
- Output: `docs/testing/manual_test_001.md` Section A

이번 과제에서만 확인:

1. Default Gateway IPv4
2. Gateway MAC
3. TP-Link Vendor evidence
4. Archer C50 Model
5. Hardware Revision V6
6. Admin HTTP/HTTPS interface
7. Authentication requirement

Acceptance Criteria는 `docs/tasks/TEST-001A.md`를 따른다.

**Execution switch**

- Researcher: `ON / READY`
- Manager: `OFF / WAIT`
- Developer: `OFF / HOLD`

TEST-001A가 REVIEW/DONE으로 넘어가기 전에는 뒤 단계 Task를 실행하지 않는다.

---

## P0 — PARENT / PARALLEL REFERENCE

### TEST-001 — Archer C50 v6 Manual Inspection

- Owner: Researcher / Project Lead
- Status: READY
- Priority: P0
- Blocked By: None
- Output: `docs/testing/manual_test_001.md`
- Research Guide: `docs/research/Router_Security_Checker_Internet_Research_PRD_v0.1.md`
- Audit Guide: `docs/audit/Router_Security_Checker_Audit_Report_v0.1.md`

이 Task는 큰 Parent Task이며 세부 Work Order로 순차 분해하여 수행한다.

전체 검사 대상:

- Gateway IP
- Gateway MAC
- Vendor
- Model
- Hardware Revision
- Firmware Version
- Firmware Latest Check
- CVE Applicability
- Router DNS
- UPnP
- WPS
- DMZ
- Port Forwarding
- Remote Administration
- Admin HTTP / HTTPS
- LAN Devices

각 항목 최소 기록:

- Input / Required Information
- Method
- Result
- Acquisition: 가능 / 불가능
- Authentication: `AUTH_NONE / AUTH_USER_REQUIRED / AUTH_MANUAL_ONLY / UNAVAILABLE`
- Automation: `YES / PARTIAL / NO`
- Confidence
- Pain Point
- Fallback
- Requirement candidate

### RESEARCH-001 — Archer C50 v6 Identity Research

- Owner: Researcher
- Status: READY
- Priority: P0
- Blocked By: None
- Output: `docs/research/tplink_router_identification.md`
- Research Plan: `docs/research/Router_Security_Checker_Internet_Research_PRD_v0.1.md`

현재는 TEST-001A의 실제 장비 결과를 먼저 확보한다. 이후 인터넷 조사 결과와 교차검증한다.

---

## P1 — NEXT

### RESEARCH-002 — TP-Link Firmware Sources & Detection

- Owner: Researcher
- Status: BLOCKED
- Priority: P1
- Blocked By: TEST-001, RESEARCH-001
- Output: `docs/research/tplink_firmware_sources.md`

### REQ-001 — Derive Phase 0 Requirements

- Owner: Project Lead
- Status: BLOCKED
- Priority: P1
- Blocked By: TEST-001, RESEARCH-001
- Output: `docs/product/requirements.md`

### AUDIT-001 — Phase 0 Evidence Review / Gate 1 Decision

- Owner: Manager
- Status: BLOCKED
- Priority: P1
- Blocked By: TEST-001, RESEARCH-001, REQ-001
- Output: `docs/audit/phase0_evidence_review.md`

---

## P2 — AFTER MANAGER APPROVAL

### DEV-001 — Windows Default Gateway Detection

- Owner: Developer
- Status: HOLD
- Priority: P2
- Blocked By: AUDIT-001 Gate approval
- Output: `src/discovery/`

> Manager가 Gate 1을 승인하기 전에는 구현을 시작하지 않는다.

---

## BACKLOG — NOT NOW

- GUI
- Traffic Analyzer implementation
- External Scan Infrastructure
- Advanced Threat Intelligence
- Backdoor Detection
- Multi-vendor Support
- Automatic Remediation

---

## Task creation rule

새 Task를 추가하기 전에 확인한다.

1. PRD의 핵심 보안 질문을 더 정확하게 해결하는가?
2. 현재 Phase에 필요한가?
3. 근거 또는 선행 Task가 존재하는가?
4. Output과 Acceptance Criteria가 명확한가?
5. 현재 Manager Gate가 허용하는가?

하나라도 불명확하면 바로 개발 Task로 만들지 않고 Research / Proposal / Blocked 상태로 유지한다.
