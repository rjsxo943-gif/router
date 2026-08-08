# Router Security Checker

일반 사용자가 자신의 공유기 보안 상태를 이해하고 대응할 수 있도록 하는 방어적 보안 진단 프로젝트입니다.

## Start here

프로젝트에 참여하는 사람과 AI는 작업 전에 아래 순서로 확인합니다.

1. [`NOTICE.md`](./NOTICE.md) — 공통 운영 규칙
2. [`STATUS.md`](./STATUS.md) — 현재 프로젝트 상태
3. [`TODO.md`](./TODO.md) — 현재 작업 지시와 우선순위
4. [`Router_Security_Checker_PRD_v0.1.md`](./Router_Security_Checker_PRD_v0.1.md) — 현재 제품 기준선(PRD v0.1)

## Current baseline

- Version target: `v0.1`
- Current phase: `PHASE 0 — Manual Research`
- Primary platform: `Windows`
- Initial vendor: `TP-Link`
- Test device: `TP-Link Archer C50 v6`

## Repository layout

```text
router/
├─ README.md
├─ NOTICE.md
├─ STATUS.md
├─ TODO.md
├─ Router_Security_Checker_PRD_v0.1.md
├─ docs/
│  ├─ product/      # 승인된 제품/요구사항 문서
│  ├─ research/     # 조사관의 근거·조사 결과
│  ├─ audit/        # Manager의 감사·범위 점검
│  ├─ testing/      # 실제 장비 검사 및 검증 기록
│  └─ decisions/    # 중요한 설계/운영 의사결정
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

새로운 조사 결과는 곧바로 개발 요구사항이 아닙니다. 근거 확인과 승인 과정을 거쳐 `TODO.md`의 Task로 승격된 뒤 구현합니다.
