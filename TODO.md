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

## P0 — NOW

### TEST-001 — Archer C50 v6 Manual Inspection

- Owner: Researcher / Project Lead
- Status: READY
- Priority: P0
- Blocked By: None
- Output: `docs/testing/manual_test_001.md`
- Research Guide: `docs/research/Router_Security_Checker_Internet_Research_PRD_v0.1.md`
- Audit Guide: `docs/audit/Router_Security_Checker_Audit_Report_v0.1.md`

검사 대상:

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

Acceptance Criteria:

- 실제 Archer C50 v6에서 각 항목의 확인 방법이 기록되어 있다.
- 획득 가능 여부와 인증 필요 여부가 기록되어 있다.
- 자동화 가능 여부와 신뢰도가 구분되어 있다.
- 실패 시 Fallback이 정의되어 있다.
- 확인 불가능한 항목은 `UNKNOWN / NOT_TESTED / NOT_SUPPORTED` 중 적합한 상태로 명시되어 있다.
- 후속 개발 Requirement 후보가 도출되어 있다.

### RESEARCH-001 — Archer C50 v6 Identity Research

- Owner: Researcher
- Status: READY
- Priority: P0
- Blocked By: None
- Output: `docs/research/tplink_router_identification.md`
- Research Plan: `docs/research/Router_Security_Checker_Internet_Research_PRD_v0.1.md`

Research order:

1. 공식 제품 페이지 / Hardware Revision 체계
2. MAC OUI
3. UPnP / SSDP Device Description
4. HTTP / HTTPS / Web UI fingerprint
5. TP-Link API / Web UI data source
6. 실제 V6 장비 검증

Acceptance Criteria:

- Vendor / Model / Hardware Version 식별 후보 방법이 정리되어 있다.
- 각 근거를 `SOURCE CLAIM / VERIFIED FACT / TEST DEVICE RESULT`로 구분한다.
- Source confidence가 기록되어 있다.
- 인증 요구사항과 자동화 난이도가 기록되어 있다.
- 실제 장비 검증이 필요한 항목이 명시되어 있다.
- 공식 자료를 최우선 근거로 사용한다.

---

## P1 — NEXT

### RESEARCH-002 — TP-Link Firmware Sources & Detection

- Owner: Researcher
- Status: BLOCKED
- Priority: P1
- Blocked By: TEST-001, RESEARCH-001
- Output: `docs/research/tplink_firmware_sources.md`

Acceptance Criteria:

- 현재 Firmware Version 획득 방법이 정리되어 있다.
- TP-Link 공식 최신 Firmware 출처가 확정되어 있다.
- Hardware Revision / Region / Build 차이를 기록한다.
- 자동화 수준과 인증 요구사항이 기록되어 있다.
- Firmware 비교 시 잘못된 region/revision 매칭을 방지할 기준이 있다.

### REQ-001 — Derive Phase 0 Requirements

- Owner: Project Lead
- Status: BLOCKED
- Priority: P1
- Blocked By: TEST-001, RESEARCH-001
- Output: `docs/product/requirements.md`

Acceptance Criteria:

- Research finding과 actual device result가 구분되어 있다.
- 검증된 내용만 Requirement로 승격되어 있다.
- 각 Requirement가 근거 문서를 추적할 수 있다.
- 자동 / 반자동 / 수동 / 미지원 상태가 구분되어 있다.
- PHASE 1 Acceptance Criteria 초안이 포함되어 있다.

### AUDIT-001 — Phase 0 Evidence Review / Gate 1 Decision

- Owner: Manager
- Status: BLOCKED
- Priority: P1
- Blocked By: TEST-001, RESEARCH-001, REQ-001
- Output: `docs/audit/phase0_evidence_review.md`

Acceptance Criteria:

- PRD와 조사/테스트 결과의 충돌을 확인한다.
- 근거가 부족한 Requirement를 표시한다.
- SAFE / UNKNOWN / Coverage 관련 미정 사항을 추적한다.
- PHASE 1 Acceptance Criteria가 테스트 가능한지 확인한다.
- `PHASE 1 PASS / CONDITIONAL PASS / FAIL(HOLD)`을 명시한다.

---

## P2 — AFTER MANAGER APPROVAL

### DEV-001 — Windows Default Gateway Detection

- Owner: Developer
- Status: HOLD
- Priority: P2
- Blocked By: AUDIT-001 Gate approval
- Output: `src/discovery/`

Acceptance Criteria:

- Windows 활성 네트워크 어댑터에서 IPv4 Default Gateway를 반환한다.
- 실패 시 명확한 오류/UNKNOWN 상태를 반환한다.
- unit test가 존재한다.
- 승인된 Requirement를 추적할 수 있다.

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

Traffic feasibility 연구는 가능하지만 **Traffic Analyzer 구현은 현재 하지 않는다.**

---

## Task creation rule

새 Task를 추가하기 전에 확인한다.

1. PRD의 핵심 보안 질문을 더 정확하게 해결하는가?
2. 현재 Phase에 필요한가?
3. 근거 또는 선행 Task가 존재하는가?
4. Output과 Acceptance Criteria가 명확한가?
5. 현재 Manager Gate가 허용하는가?

하나라도 불명확하면 바로 개발 Task로 만들지 않고 Research / Proposal / Blocked 상태로 유지한다.
