# Task Work Orders

이 폴더는 Project Lead가 발행한 **실행 단위 과제(Work Order)** 를 보관한다.

## Rule

- `TODO.md`는 전체 작업 큐와 상태를 관리한다.
- `docs/tasks/`는 실제 담당자가 수행할 수 있도록 한 Task를 상세 지시서로 풀어쓴다.
- Task는 `PROJECT_BOARD.md`에 등록되어야 공식 과제로 취급한다.
- Manager Gate가 `HOLD`인 개발 Task는 Work Order가 있어도 실행하지 않는다.

## Status

`READY → IN PROGRESS → REVIEW → DONE`

필요 시 `BLOCKED / HOLD`를 사용한다.

## Naming

상위 TODO ID를 유지하면서 세부 과제는 suffix를 붙인다.

예:

- `TEST-001A.md`
- `TEST-001B.md`
- `RESEARCH-001A.md`

## Required fields

각 Work Order에는 최소한 다음을 포함한다.

- Parent Task
- Owner
- Status
- Objective
- Why now
- Required inputs
- Exact scope
- Procedure
- Required evidence
- Output path
- Acceptance criteria
- Stop conditions
- What this unlocks
