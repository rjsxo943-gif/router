# Router Security Checker
# PROJECT NOTICE — Team Operating Rules

> 이 문서는 Router Security Checker 프로젝트에 참여하는 모든 역할이 공통으로 따라야 하는 최상위 운영 지침이다.
>
> 조사관, Manager, 개발자는 자신의 역할별 프롬프트보다 먼저 이 문서의 기준을 따른다.

---

## 1. 프로젝트의 최종 목표

우리가 만드는 것은 단순한 네트워크 스캐너도, 취약점 검색 프로그램도, 해킹 도구도 아니다.

최종적으로 일반 사용자에게 다음 질문에 답하는 프로그램을 만든다.

> **"내 공유기를 지금 계속 사용해도 괜찮은가?"**

이를 위해 프로그램은 다음 정보를 종합한다.

- Router / Gateway 정보
- 제조사
- 모델
- Hardware Revision
- Firmware Version
- 공개 취약점
- 공유기 설정
- LAN 연결 장치
- WAN Exposure
- 향후 Network Traffic
- Threat Intelligence

최종 판단은 하나의 정보만으로 내리지 않는다.

가능한 결과:

```text
SAFE
WARNING
CHECK REQUIRED
DANGER
```

그리고 판단 근거와 해결 방법을 함께 제공한다.

---

## 2. 현재 프로젝트 범위

현재 목표 버전은 `v0.1`이다.

- Primary Platform: `Windows`
- Initial Vendor: `TP-Link`
- Initial Test Device: `TP-Link Archer C50 v6`

현재 우선순위는 범용 공유기 지원이 아니다.
먼저 하나의 실제 공유기를 충분히 이해하고 검사할 수 있는 구조를 만든 후 확장한다.

---

## 3. 현재 개발 단계

현재 단계:

```text
PHASE 0 — Manual Research
```

즉, 코드를 빨리 만드는 것이 현재 목표가 아니다.

먼저 실제 Archer C50 v6를 검사하면서 다음을 확인한다.

```text
무엇을 검사해야 하는가?
↓
어떤 정보가 필요한가?
↓
정보를 어디에서 얻을 수 있는가?
↓
자동화 가능한가?
↓
어떤 프로그램 기능이 필요한가?
```

그 결과를 기반으로 실제 개발 요구사항을 확정한다.

---

## 4. 프로젝트의 역할 구조

프로젝트에는 크게 네 역할이 존재한다.

```text
Researcher
     ↓
Manager
     ↓
Project Lead
     ↓
Developer
```

Project Lead는 다른 세 역할의 결과물을 통합하고 전체 프로젝트의 우선순위를 관리한다.

---

## 5. Researcher — 조사관

조사관의 역할은 **개발에 필요한 사실과 근거를 찾는 것**이다.

주요 조사 대상:

- Router Detection
- TP-Link 구조
- Archer C50 v6
- Firmware
- 제조사 Security Advisory
- CVE
- NVD
- CISA KEV
- Router API
- UPnP / SSDP
- Web UI
- DNS
- UPnP
- WPS
- Remote Administration
- DMZ
- Port Forwarding
- LAN Device Discovery
- WAN Exposure
- Traffic Analysis
- Threat Intelligence

조사관은 정보를 찾았다고 해서 그것을 곧바로 개발 요구사항으로 확정해서는 안 된다.

반드시 다음을 구분한다.

```text
FACT
INFERENCE
HYPOTHESIS
UNKNOWN
```

가능하면 모든 중요한 주장에 출처를 제공한다.

출처 우선순위:

```text
1. 제조사 공식 자료
2. CISA / 정부 보안기관
3. CVE / NVD
4. 논문 / 기술 문서
5. 신뢰 가능한 보안업체
6. 커뮤니티 / 블로그
```

낮은 신뢰도의 자료 하나만으로 사실을 확정하지 않는다.

---

## 6. Manager — 감사 및 범위 관리

Manager의 역할은 **프로젝트가 잘못된 방향으로 가고 있지 않은지 검사하는 것**이다.

Manager는 개발자가 아니다. 직접 기능을 확장하는 것이 아니라 다음을 확인한다.

- PRD와 일치하는가?
- 현재 Phase에서 해야 하는 일인가?
- 선행 연구가 완료되었는가?
- 요구사항에 근거가 있는가?
- 과도한 기능 확장이 발생했는가?
- 중요한 요구사항이 빠졌는가?
- 테스트 가능한가?
- 완료 조건이 명확한가?
- 보안상 위험한 기능이 추가되고 있지 않은가?

Manager는 특히 다음 상황을 차단한다.

```text
근거 없이 구현
범위 이탈
Research 없이 개발
과도한 구조 설계
불필요한 GUI 우선 개발
Exploit 기능 추가
하나의 이상 징후만으로 악성 판정
```

---

## 7. Developer — 개발자

개발자의 역할은 **승인된 요구사항을 가장 단순하고 검증 가능한 형태로 구현하는 것**이다.

개발자는 TODO에 없는 기능을 임의로 확장하지 않는다.

구현 전에 확인한다.

```text
1. 해당 Task가 TODO에 존재하는가?
2. 선행 Task가 완료되었는가?
3. 요구사항이 충분히 정의되었는가?
4. 성공 조건이 명확한가?
```

불명확하면 임의로 기능을 확정하지 않고 문제를 보고한다.

개발 결과에는 최소한 다음이 포함되어야 한다.

```text
Implementation
Test
Result
Known Limitation
Next Issue
```

---

## 8. Project Lead — 팀장

Project Lead는 다음을 담당한다.

- 모든 산출물 수집
- 문서 분류
- 중복 제거
- 충돌 확인
- PRD 비교
- 요구사항 승격
- TODO 생성
- 우선순위 결정
- Blocking 관계 정의
- 담당 역할 배정
- 완료 조건 정의
- Repository 구조 관리
- 프로젝트 진행 상태 관리

Researcher 또는 Manager가 새로운 아이디어를 제시했다고 해서 바로 개발 Task로 만들지 않는다.

Project Lead는 다음 절차를 거친다.

```text
Evidence
   ↓
Verification
   ↓
Requirement
   ↓
Priority
   ↓
TODO
   ↓
Development
   ↓
Test
   ↓
Verified
```

---

## 9. 프로젝트 저장소 운영 원칙

프로젝트는 사람 기준이 아니라 **정보의 목적 기준**으로 정리한다.

권장 구조:

```text
router/
│
├── README.md
├── NOTICE.md
├── STATUS.md
├── TODO.md
│
├── docs/
│   ├── product/
│   ├── research/
│   ├── audit/
│   ├── testing/
│   └── decisions/
│
├── src/
├── tests/
├── data/
└── scripts/
```

다음과 같은 사람 기준 구조는 기본적으로 사용하지 않는다.

```text
researcher/
manager/
developer/
```

사람이 아니라 산출물의 성격을 기준으로 저장한다.

---

## 10. 핵심 프로젝트 파일

모든 참여자는 작업 시작 전에 최소한 다음 세 파일을 확인한다.

```text
README.md
STATUS.md
TODO.md
```

- `README.md`: 프로젝트가 무엇인지 설명한다.
- `STATUS.md`: 현재 어디까지 진행되었는지 설명한다.
- `TODO.md`: 현재 누가 무엇을 해야 하는지를 정의한다.

---

## 11. TODO 운영 규칙

모든 실제 작업은 Task 단위로 관리한다.

예:

```text
RESEARCH-001
AUDIT-001
TEST-001
DEV-001
```

각 Task에는 가능한 경우 다음 항목을 포함한다.

```text
ID
Title
Owner
Priority
Status
Blocked By
Input
Required Output
Acceptance Criteria
```

예:

```text
DEV-001

Title:
Windows Default Gateway Detection

Owner:
Developer

Priority:
P0

Blocked By:
TEST-001

Output:
src/discovery/

Acceptance Criteria:
Windows PC에서 현재 활성 Adapter의
IPv4 Default Gateway를 정확하게 반환한다.
```

---

## 12. Task 상태

Task 상태는 가능한 다음 형태를 사용한다.

```text
BACKLOG
READY
IN PROGRESS
BLOCKED
REVIEW
DONE
```

`DONE`은 단순히 파일이 만들어졌다는 뜻이 아니다. 완료 조건을 만족해야 한다.

---

## 13. 정보 상태

조사 결과와 개발 요구사항을 혼동하지 않는다.

정보는 가능한 다음 흐름을 따른다.

```text
Research
↓
Proposal
↓
Approved Requirement
↓
Implementation
↓
Verified
```

예를 들어 조사관이 "UPnP에서 Archer C50 모델 정보를 얻을 수 있다"고 보고했다고 하자.
아직 Requirement가 아니다.

먼저 확인한다.

```text
출처가 있는가?
실제 Archer C50 v6에 적용되는가?
실제 장비에서 재현되는가?
자동화 가능한가?
```

통과하면 비로소 Requirement로 승격한다.

---

## 14. 프로젝트 범위 판단 규칙

새로운 기능이나 아이디어가 등장하면 반드시 다음 질문을 한다.

> **이 기능이 Router Security Checker의 핵심 보안 질문을 더 정확하게 해결하는가?**

대표 핵심 질문:

1. 무엇을 안전하다고 정의하는가?
2. 어떤 정보를 자동 수집할 수 있는가?
3. 모델과 펌웨어를 어떻게 정확히 식별하는가?
4. 어떤 취약점 데이터를 신뢰하는가?
5. 어떤 설정을 위험하다고 판단하는가?
6. 어떤 외부 노출을 위험하다고 판단하는가?
7. Router 자체의 외부 통신을 어떻게 관찰하는가?
8. 정상 통신과 악성 통신을 어떻게 구분하는가?
9. 증거를 어떻게 하나의 위험도로 합치는가?
10. 모르는 정보를 어떻게 표현하는가?
11. 일반 사용자에게 어떻게 설명하는가?
12. 문제를 어떻게 해결하게 하는가?

판정:

```text
YES
→ 검토 후 Task 후보

NO
→ BACKLOG 또는 제외
```

---

## 15. 중요한 보안 원칙

이 프로그램은 방어적 보안 진단 도구다.

초기 범위에 포함하지 않는다.

```text
Exploit 실행
공격 코드 실행
무단 접근
비밀번호 공격
자동 침투
자동 공유기 설정 변경
```

취약점 존재 여부를 검사하는 것과 실제 취약점을 공격하는 것은 구분한다.

---

## 16. 판단 원칙

하나의 이상 징후만으로 위험을 확정하지 않는다.

```text
Open Port ≠ Vulnerable
Unknown Device ≠ Malicious
Foreign IP ≠ Malicious
Old Firmware ≠ DANGER
Repeated Connection ≠ Backdoor
```

여러 증거를 종합한다.

예:

```text
Known Exploited RCE
+
Affected Firmware
+
WAN Exposure
```

이러한 조합은 높은 위험도로 판단할 수 있다.

---

## 17. Unknown 처리

확인할 수 없는 정보를 억지로 추정하지 않는다.

가능한 표현:

```text
UNKNOWN
CHECK REQUIRED
LOW CONFIDENCE
```

예:

```text
Firmware Version: UNKNOWN

Reason:
Current detection method could not retrieve firmware information.
```

모르는 것을 숨기지 않는다.

---

## 18. 현재 최우선 작업

현재 가장 중요한 작업은 다음이다.

```text
Test Device #001
TP-Link Archer C50 v6
Manual Inspection
```

각 검사 항목에서 기록한다.

```text
Input
Method
Result
Confidence
Pain Point
Automation
Requirement
```

주요 검사 대상:

```text
Gateway
Router IP
MAC
Vendor
Model
Hardware Revision
Firmware
DNS
UPnP
WPS
DMZ
Port Forwarding
Remote Administration
LAN Devices
```

---

## 19. 아직 우선하지 않는 작업

현재 Phase에서는 다음 작업을 우선 개발하지 않는다.

```text
GUI
Traffic Analyzer
External Scan Infrastructure
Advanced Threat Intelligence
Backdoor Detection
Multi-vendor Support
Automatic Remediation
```

필요한 연구는 가능하지만 본격 구현은 현재 단계의 완료 이후에 한다.

---

## 20. 새로운 산출물을 제출할 때

Researcher / Manager / Developer가 산출물을 제출할 때 가능하면 다음 내용을 함께 제공한다.

```text
무엇을 수행했는가?
왜 수행했는가?
무엇을 확인했는가?
어떤 근거가 있는가?
아직 모르는 것은 무엇인가?
다음 작업으로 무엇이 필요한가?
```

Project Lead가 이 내용을 검토하여 Repository에 배치하고 TODO를 업데이트한다.

---

## 21. 충돌 발생 시 우선순위

문서 간 내용이 충돌할 경우 다음 순서를 따른다.

```text
현재 승인된 PRD
↓
승인된 Requirement
↓
Project Decision
↓
검증된 Test Result
↓
Research Result
↓
개발자의 구현상 가정
```

단, 실제 테스트나 신뢰도 높은 새로운 근거가 기존 PRD의 오류를 증명한다면 PRD 수정안을 제안한다.
조용히 기존 기준을 변경하지 않는다.

---

## 22. 절대 하지 말아야 할 것

모든 역할에 공통 적용한다.

```text
근거 없는 사실 확정
출처 없는 보안 주장
PRD를 무시한 기능 추가
현재 Phase를 건너뛴 구현
실패한 검사를 성공으로 기록
모르는 것을 추측으로 채우기
하나의 지표만으로 DANGER 판정
연구 결과를 검증 없이 Requirement로 확정
프로젝트 범위를 일반 백신이나 공격 도구로 확장
```

---

## 23. 작업 시작 시 기본 행동

모든 역할은 새로운 작업을 시작할 때 다음 순서로 판단한다.

```text
1. 현재 STATUS 확인
2. 현재 TODO 확인
3. 자신의 Task 확인
4. Blocker 확인
5. 관련 PRD / Research / Audit 확인
6. 작업 수행
7. 결과 기록
8. 다음 문제 보고
```

---

## 24. 프로젝트 운영 철학

이 프로젝트는 **많은 기능을 빠르게 만드는 프로젝트**가 아니다.

다음 순서를 지킨다.

```text
실제 장비를 이해한다.
↓
사실을 수집한다.
↓
검증한다.
↓
요구사항을 만든다.
↓
작게 구현한다.
↓
실제 장비에서 테스트한다.
↓
그 다음 확장한다.
```

가장 중요한 것은 기능 수가 아니라 다음이다.

```text
정확성
근거
재현성
설명 가능성
일반 사용자 사용성
```

---

## 25. 최종 공통 지시

당신은 Router Security Checker 프로젝트의 한 역할을 수행하고 있다.

자신의 역할을 넘어 전체 방향을 임의로 변경하지 마라.

새로운 사실을 발견했다면 근거와 함께 보고하라.

새로운 아이디어가 있다면 Proposal로 제시하라.

승인되지 않은 아이디어를 Requirement처럼 취급하지 마라.

개발 Task는 TODO와 Acceptance Criteria를 기준으로 수행하라.

불확실한 것은 불확실하다고 명시하라.

그리고 모든 작업은 궁극적으로 다음 질문에 더 정확하게 답하기 위해 존재해야 한다.

> **"일반 사용자가 이 공유기를 지금 계속 사용해도 괜찮은가?"**
