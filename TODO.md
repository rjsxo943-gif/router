# Router Security Checker — TODO

> 모든 실제 작업은 이 파일의 Task를 기준으로 수행한다. 조사 결과나 아이디어가 곧바로 개발 Task가 되지 않는다.

## Status legend

`BACKLOG` → `READY` → `IN PROGRESS` → `REVIEW` → `DONE`

필요 시 `BLOCKED`를 사용한다.

---

## P0 — NOW

### TEST-001 — Archer C50 v6 Manual Inspection

- Owner: Researcher / Project Lead
- Status: READY
- Priority: P0
- Blocked By: None
- Output: `docs/testing/manual_test_001.md`

검사 대상:

- Gateway
- Router IP
- MAC
- Vendor
- Model
- Hardware Revision
- Firmware
- DNS
- UPnP
- WPS
- DMZ
- Port Forwarding
- Remote Administration
- LAN Devices

각 항목 기록 형식:

- Input
- Method
- Result
- Confidence
- Pain Point
- Automation: `YES / PARTIAL / NO`
- Requirement

Acceptance Criteria:

- 실제 장비에서 각 항목의 확인 방법이 기록되어 있다.
- 자동화 가능 여부가 구분되어 있다.
- 확인 불가능한 항목은 `UNKNOWN`으로 명시되어 있다.
- 후속 개발 요구사항 후보가 도출되어 있다.

### RESEARCH-001 — TP-Link Router Identification Research

- Owner: Researcher
- Status: READY
- Priority: P0
- Blocked By: None
- Output: `docs/research/tplink_router_identification.md`

Acceptance Criteria:

- Archer C50 v6에서 제조사/모델/하드웨어 버전 식별 후보 방법을 정리한다.
- 각 방법을 `FACT / INFERENCE / HYPOTHESIS / UNKNOWN`으로 구분한다.
- 공식 자료를 최우선 근거로 사용한다.

---

## P1 — NEXT

### RESEARCH-002 — TP-Link Firmware Sources

- Owner: Researcher
- Status: BLOCKED
- Priority: P1
- Blocked By: TEST-001, RESEARCH-001
- Output: `docs/research/tplink_firmware_sources.md`

### AUDIT-001 — Phase 0 Evidence Review

- Owner: Manager
- Status: BLOCKED
- Priority: P1
- Blocked By: TEST-001, RESEARCH-001
- Output: `docs/audit/phase0_evidence_review.md`

Acceptance Criteria:

- PRD와 조사 결과의 충돌을 확인한다.
- 근거가 부족한 요구사항을 표시한다.
- PHASE 1 진입 가능 여부를 판단한다.

---

## P2 — AFTER APPROVAL

### DEV-001 — Windows Default Gateway Detection

- Owner: Developer
- Status: BLOCKED
- Priority: P2
- Blocked By: TEST-001, AUDIT-001
- Output: `src/discovery/`

Acceptance Criteria:

- Windows 활성 네트워크 어댑터에서 IPv4 Default Gateway를 반환한다.
- 실패 시 명확한 오류/UNKNOWN 상태를 반환한다.
- 테스트가 존재한다.

---

## BACKLOG — NOT NOW

- GUI
- Traffic Analyzer
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

하나라도 불명확하면 바로 개발 Task로 만들지 않고 Research 또는 Proposal 상태로 유지한다.
