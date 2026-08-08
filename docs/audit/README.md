# Audit Documents

> 이 폴더에 직접 들어왔다면 먼저 저장소 루트의 `PROJECT_BOARD.md`를 확인한다.

Manager의 진행 감사, 범위 점검, Phase Gate 판정, 누락 및 리스크 검토 문서를 보관한다.

## Current audit decision

- `Router_Security_Checker_Audit_Report_v0.1.md`
  - Overall: `CONDITIONAL PASS`
  - PHASE 0 entry: `PASS`
  - PHASE 1 development: `HOLD`
  - Test device: TP-Link Archer C50 v6

## Current Gate 0 → Gate 1 requirements

- 실제 Archer C50 v6 수동 검사
- 검사 항목 획득 가능 여부
- 인증 요구사항
- 자동화 수준 및 Confidence
- 실패 시 Fallback
- `docs/product/requirements.md`
- PHASE 1 Acceptance Criteria
- Manager 재감사

## Rules

- 감사 문서는 PRD와 Research/Test 결과를 비교한다.
- `PASS / CONDITIONAL PASS / FAIL(HOLD)`로 Gate를 명시한다.
- Gate가 HOLD인 Phase의 개발을 승인된 것으로 취급하지 않는다.
- 최신 공식 감사 문서와 Gate 상태는 루트 `PROJECT_BOARD.md`에서 확인한다.
