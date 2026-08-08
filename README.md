# Router Security Checker

일반 사용자가 자신의 공유기 보안 상태를 이해하고 대응할 수 있도록 하는 방어적 보안 진단 프로젝트입니다.

> **사람과 AI 모두 작업 시작 전 [`PROJECT_BOARD.md`](./PROJECT_BOARD.md)를 확인해야 합니다.**

## Mandatory start path

### AI / Coding Agent

1. [`AGENTS.md`](./AGENTS.md) — AI 진입 규칙
2. [`PROJECT_BOARD.md`](./PROJECT_BOARD.md) — 중앙 게시판 / 문서 지도 / 최신 결정
3. [`NOTICE.md`](./NOTICE.md) — 공통 운영 규칙
4. [`STATUS.md`](./STATUS.md) — 현재 Phase 및 Gate
5. [`TODO.md`](./TODO.md) — 현재 실행 가능한 Task
6. 관련 Research / Audit / Testing / Product 문서

### Human contributor

1. [`PROJECT_BOARD.md`](./PROJECT_BOARD.md)
2. [`NOTICE.md`](./NOTICE.md)
3. [`STATUS.md`](./STATUS.md)
4. [`TODO.md`](./TODO.md)

현재 제품 기준선:

[`Router_Security_Checker_PRD_v0.1.md`](./Router_Security_Checker_PRD_v0.1.md)

## Current gate

- Version target: `v0.1`
- Current phase: `PHASE 0 — Manual Research`
- Primary platform: `Windows`
- Initial vendor: `TP-Link`
- Test device: `TP-Link Archer C50 v6`
- Overall audit: `CONDITIONAL PASS`
- PHASE 0 entry: `PASS`
- PHASE 1 development: `HOLD`

따라서 현재 핵심 작업은 **Archer C50 v6 실제 수동 검사와 PHASE 0 조사**이며, PHASE 1 구현은 아직 시작하지 않습니다.

## Repository layout

```text
router/
├─ README.md
├─ AGENTS.md                 # AI mandatory entry
├─ PROJECT_BOARD.md          # 중앙 게시판 / 문서 registry
├─ NOTICE.md                 # 공통 운영 규칙
├─ STATUS.md                 # 현재 상태
├─ TODO.md                   # 실행 Task
├─ Router_Security_Checker_PRD_v0.1.md
├─ .github/
│  └─ copilot-instructions.md
├─ docs/
│  ├─ product/               # 승인된 제품/요구사항 문서
│  ├─ research/              # 조사관의 근거·조사 결과
│  ├─ audit/                 # Manager의 감사·범위 점검
│  ├─ testing/               # 실제 장비 검사 및 검증 기록
│  └─ decisions/             # 중요한 설계/운영 의사결정
├─ src/
│  ├─ discovery/
│  ├─ adapters/
│  ├─ firmware/
│  ├─ vulnerability/
│  ├─ config_audit/
│  ├─ device_scanner/
│  ├─ risk/
│  └─ report/
├─ tests/
│  ├─ unit/
│  └─ integration/
├─ data/
│  └─ reference/
└─ scripts/
```

사람별 폴더가 아니라 **산출물의 목적과 시스템 책임 기준**으로 정리합니다.

## Role flow

```text
Researcher → Manager → Project Lead → Developer → Verification
```

새로운 조사 결과는 곧바로 개발 요구사항이 아닙니다.

```text
Research
↓
Verification
↓
Approved Requirement
↓
TODO
↓
Implementation
↓
Verification
```

새 문서의 공식 위치와 현재 authority는 항상 [`PROJECT_BOARD.md`](./PROJECT_BOARD.md)에서 확인합니다.
