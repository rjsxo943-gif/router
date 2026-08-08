# Router Security Checker
## Project Progress Audit Report v0.1

**Audit Target:** `Router_Security_Checker_PRD_v0.1`  
**Project Stage:** Product Definition / Manual Research  
**Audit Role:** Project Progress Audit Manager  
**Audit Result:** `CONDITIONAL PASS`  
**Next Stage Decision:** `PHASE 0 진입 승인 / PHASE 1 개발 시작 HOLD`

---

# 1. Executive Summary

현재 `Router Security Checker PRD v0.1`은 프로젝트의 **목적, 사용자, 핵심 보안 질문, 기능 범위, 제외 범위, 개발 단계**를 비교적 명확하게 정의하고 있다.

특히 본 프로젝트를 단순 네트워크 스캐너, 일반 백신, 공격 도구로 확장시키지 않고,

> **일반 사용자가 자신의 공유기를 지금 계속 사용해도 괜찮은지 판단할 수 있도록 근거 기반으로 검사하는 방어적 보안 진단 도구**

라는 방향으로 고정한 점은 적절하다.

또한 단일 이상 징후만으로 위험을 단정하지 않고 여러 근거를 결합하며, 프로그램이 알 수 없는 정보는 숨기지 않고 `UNKNOWN` 또는 별도의 확인 필요 상태로 표현한다는 원칙도 프로젝트의 신뢰성을 높이는 핵심 요소다.

따라서 현 PRD는 **제품 정의 및 수동 연구 단계의 기준 문서로 사용 가능**하다고 판단한다.

다만 아직 실제 개발 명세서로 사용하기에는 다음 항목이 충분히 정의되지 않았다.

- `SAFE`의 정확한 정의
- 검사하지 못한 항목의 처리 방식
- Risk Engine의 실제 판정 구조
- 관리자 인증이 필요한 검사와 그렇지 않은 검사의 구분
- Phase별 정량적 PASS / FAIL 조건
- 자동 검사 / 반자동 검사 / 수동 검사 범위

따라서 현재 단계에서는 **PHASE 0 — Manual Research 진입은 승인하지만 PHASE 1 개발 시작은 보류한다.**

---

# 2. Audit Decision

```text
PRD v0.1
→ PASS

Product Direction
→ PASS

Scope Definition
→ PASS

PHASE 0 — Manual Research
→ PASS

PHASE 1 — Discovery MVP 개발 시작
→ HOLD

Overall
→ CONDITIONAL PASS
```

## 판정 의미

`CONDITIONAL PASS`는 현재 문서가 잘못되었다는 의미가 아니다.

현재 단계에서 필요한 수준의 제품 정의는 충분하지만, 실제 공유기를 검사하여 얻어야 하는 정보까지 이론적으로 먼저 결정하려고 하면 잘못된 요구사항이 만들어질 가능성이 높다.

따라서 다음 단계는 추가 기능 기획이나 코드 작성이 아니라 **실제 TP-Link Archer C50 v6 수동 검사**가 되어야 한다.

---

# 3. Strengths

## 3.1 Product Goal이 명확하다

프로젝트가 최종적으로 답해야 하는 질문을 다음과 같이 정의한 것은 매우 적절하다.

> **내 공유기를 지금 계속 사용해도 괜찮은가?**

이 질문을 중심으로 다음 하위 보안 질문이 파생된다.

- 펌웨어가 최신인가?
- 현재 펌웨어에 알려진 취약점이 존재하는가?
- 위험한 공유기 설정이 존재하는가?
- 인터넷에 불필요한 서비스가 노출되어 있는가?
- 알 수 없는 장치가 네트워크에 연결되어 있는가?
- 공유기가 수상한 외부 서버와 통신하는가?
- 알려진 악성 인프라와 연결되는가?
- 외부에서 원격 제어될 가능성이 있는가?

향후 새로운 기능이 제안될 경우 반드시 위 질문 중 하나를 더 정확하게 해결하는지 확인해야 한다.

---

## 3.2 Scope Control이 잘 되어 있다

초기 버전에서 다음 기능을 제외한 것은 적절하다.

- PC 악성코드 검사
- 일반 백신 기능
- 자동 공유기 설정 변경
- 공격 코드 실행
- 실제 Exploit 수행

이를 통해 프로젝트가 다음과 같은 방향으로 이탈하는 것을 방지할 수 있다.

```text
Router Security Checker
        ↓ 범위 이탈 방지
일반 백신
해킹 도구
네트워크 관리 플랫폼
패킷 분석 전문 도구
```

본 프로젝트의 기본 성격은 계속 **방어적 진단 도구**로 유지해야 한다.

---

## 3.3 오탐 방지 철학이 적절하다

다음 원칙들은 향후 Risk Engine 및 보고서 설계에서도 유지해야 한다.

```text
Open Port ≠ Vulnerable

Country ≠ Malicious

Unknown Device ≠ Danger

Outdated Firmware ≠ Immediate Danger

Repeated Connection ≠ Malicious
```

즉 하나의 특징만 가지고 악성 또는 위험이라고 단정하지 않는다.

판정은 반드시 다중 근거를 기반으로 해야 한다.

---

## 3.4 UNKNOWN을 허용하는 구조가 좋다

보안 검사 도구에서 가장 위험한 오류 중 하나는

```text
확인할 수 없음
```

을

```text
문제 없음
```

으로 처리하는 것이다.

현재 PRD는 알 수 없는 정보를 숨기지 않는다는 원칙을 가지고 있으므로 이 방향은 유지해야 한다.

향후에는 다음 상태 체계를 검토할 필요가 있다.

```text
PASS
FAIL
UNKNOWN
NOT_TESTED
NOT_SUPPORTED
```

그리고 최종 판단에서는 최소한 다음 3개 개념을 분리할 필요가 있다.

```text
Risk
Confidence
Coverage
```

---

# 4. Major Audit Findings

# Finding 01 — SAFE 정의 부족

## 발견된 문제

현재 최종 결과는 다음 네 단계로 정의되어 있다.

```text
SAFE
WARNING
CHECK REQUIRED
DANGER
```

하지만 어떤 조건을 충족해야 `SAFE`로 판정할 수 있는지는 아직 명확하지 않다.

예를 들어 다음 결과가 나왔다고 가정한다.

```text
Firmware              PASS
Known CVE             PASS
WPS                    PASS
Remote Administration PASS
Unknown Device         PASS
Traffic Analysis       NOT_TESTED
```

이 상황에서 최종 결과를

```text
SAFE
```

로 할 것인지,

```text
CHECK REQUIRED
```

로 할 것인지 현재 PRD만으로는 결정할 수 없다.

## 왜 문제인가

검사하지 않은 항목을 안전으로 간주하면 실제 검사 범위보다 과도하게 강한 안전 판정이 나올 수 있다.

## 수정 요구사항

Risk Engine 개발 전 다음 개념을 명확히 정의해야 한다.

```text
PASS
FAIL
UNKNOWN
NOT_TESTED
NOT_SUPPORTED
```

그리고 최종 결과가 검사 Coverage를 고려하도록 해야 한다.

## 재검증 조건

다음 질문에 명확한 답이 존재해야 한다.

> 어떤 검사 항목이 PASS여야 SAFE를 선언할 수 있는가?

---

# Finding 02 — Risk Engine이 개념 수준에 머물러 있음

## 발견된 문제

현재 PRD에는 다음과 같은 점수 예시가 존재한다.

```text
Start: 100

Critical CVE       -30
Remote Admin       -15
Outdated Firmware  -10
UPnP                -3
Unknown Device      -2
Known Malicious C2 -50
```

동시에 단순 합산만 사용하지 않는다고 정의되어 있다.

하지만 실제 Evidence들이 어떤 구조로 결합되는지는 아직 정해지지 않았다.

## 왜 문제인가

예를 들어 다음 두 상황은 같은 UPnP 상태라도 위험도가 다르다.

### Case A

```text
UPnP ON
```

### Case B

```text
UPnP ON
+
Actual External Port Mapping
+
Requesting Device Unknown
```

단일 점수만으로 두 상황을 동일하게 평가하면 오탐 또는 과소평가가 발생할 수 있다.

## 수정 요구사항

PHASE 5 이전까지 최소한 다음 판정 구조를 정의한다.

```text
Evidence
    ↓
Finding
    ↓
Rule
    ↓
Risk
    ↓
Final Verdict
```

예:

```text
Evidence
UPnP = ON

Evidence
External Mapping = TCP 50000

Evidence
Request Device = Unknown

        ↓

Finding
Unexpected UPnP Exposure

        ↓

Rule
UPnP + External Mapping + Unknown Device

        ↓

Risk
HIGH
```

## 재검증 조건

Risk Engine의 입력, 판정 규칙, 출력 상태가 데이터 구조 수준에서 정의되어야 한다.

---

# Finding 03 — 인증 요구사항이 정의되지 않음

## 발견된 문제

현재 PRD에는 다음 수집 방식이 존재한다.

- Router API
- Web UI
- UPnP
- 관리자 페이지 Parsing
- 사용자 입력

하지만 어떤 정보가 관리자 인증 없이 획득 가능하고 어떤 정보가 로그인을 요구하는지 아직 분리되어 있지 않다.

예를 들어 다음 정보는 비교적 인증 없이 획득할 가능성이 있다.

```text
Gateway IP
Gateway MAC
ARP Devices
OUI Vendor
```

반면 다음 정보는 관리자 인증이 필요할 가능성이 높다.

```text
Firmware Version
WPS
DMZ
Port Forwarding
Remote Administration
Router DNS
```

## 왜 문제인가

프로그램의 핵심 사용자 경험이 달라진다.

```text
[Scan Router]
```

한 번으로 모든 검사가 가능한 프로그램과,

```text
[Scan Router]
        ↓
관리자 계정 입력
        ↓
추가 검사
```

가 필요한 프로그램은 제품 구조 자체가 다르다.

## 수정 요구사항

PHASE 0 수동 검사에서 모든 검사 항목에 다음 인증 상태를 기록한다.

```text
AUTH_NONE
AUTH_USER_REQUIRED
AUTH_MANUAL_ONLY
UNAVAILABLE
```

## 재검증 조건

Archer C50 v6 기준으로 주요 검사 항목의 인증 필요 여부가 기록되어야 한다.

---

# Finding 04 — 전체 Vision과 v0.1 Scope 혼동 위험

## 발견된 문제

전체 PRD에는 다음 고급 기능도 포함되어 있다.

- Traffic Analyzer
- Threat Intelligence
- External Port Scan
- Router-Originated Traffic Detection
- Suspicious Command Channel Detection
- Backdoor Behavior Heuristics

하지만 실제 v0.1 범위는 다음으로 제한되어 있다.

```text
Windows
+
TP-Link
+
Gateway Detection
+
Router Identification
+
Firmware Check
+
CVE Check
+
Configuration Audit
+
LAN Device Discovery
+
Risk Engine
+
Security Report
```

## 왜 문제인가

개발 과정에서 흥미로운 기능부터 구현하기 시작하면 Roadmap을 건너뛰어 프로젝트 규모가 급격하게 커질 수 있다.

특히 Traffic Analyzer는 기술적으로 매력적이지만 현재 v0.1의 필수 기능이 아니다.

## 수정 요구사항

프로젝트 감사 시 `MVP Definition`을 최상위 범위 기준으로 사용한다.

새로운 기능 요청 발생 시 다음 규칙을 적용한다.

```text
v0.1 MVP에 포함됨?
        │
        ├─ YES → 현재 개발 후보
        │
        └─ NO
             ↓
Final 12 Questions 중 하나를 반드시 해결하는가?
        │
        ├─ YES → Future Backlog
        └─ NO  → 제외
```

---

# Finding 05 — Phase 성공 기준이 추상적임

## 발견된 문제

현재 Roadmap에는 Phase별 성공 기준이 존재하지만 일부 기준은 실제 테스트에 사용하기에는 추상적이다.

예:

> Archer C50 v6를 자동 또는 반자동으로 정확히 식별한다.

## 왜 문제인가

`정확히 식별한다`의 구체적인 PASS / FAIL 조건이 없다.

예를 들어 다음 결과의 처리 기준이 필요하다.

```text
Vendor = TP-Link
Model = Archer C50
Hardware Version = Unknown
Firmware = Unknown
```

이 결과가 PHASE 2 성공인지 실패인지 현재는 명확하지 않다.

## 수정 요구사항

각 Phase 개발 전에 다음 구조의 Acceptance Criteria를 만든다.

```text
Expected Result
Allowed Fallback
Failure Condition
Unknown Handling
Minimum Confidence
```

예:

```text
PHASE 2 — Router Identification

Expected
Vendor = TP-Link
Model = Archer C50
Hardware = V6

Allowed Fallback
Hardware Version 자동 탐지 실패
→ 사용자 직접 선택 허용

FAIL
Archer C50 V4를 V6로 확정

FAIL
정보가 없는데 Confidence HIGH

UNKNOWN
펌웨어 버전을 획득할 수 없음
```

---

# 5. Current Project Maturity Assessment

현재 프로젝트 상태를 다음과 같이 평가한다.

```text
제품 철학        ██████████  90%
범위 정의        ██████████  90%
기능 정의        █████████░  85%
Architecture    ████████░░  75%
데이터 모델      ████░░░░░░  40%
판정 기준        ████░░░░░░  40%
테스트 기준      ███░░░░░░░  30%
실제 구현 요구   ██░░░░░░░░  20%
```

이는 현재 프로젝트 단계에서는 비정상적인 상태가 아니다.

현재 프로젝트는

```text
Product Definition
        ↓
Manual Research
```

단계에 있으므로 실제 장비 검사를 통해 아래 영역을 채우는 것이 다음 목표다.

```text
데이터 모델
판정 기준
테스트 기준
실제 구현 요구사항
```

---

# 6. PHASE 0 Audit Requirements

PHASE 0에서는 `Test Device #001 — TP-Link Archer C50 v6`를 대상으로 실제 수동 검사를 수행한다.

모든 검사 항목은 최소한 다음 정보를 기록해야 한다.

```text
1. 무엇을 확인했는가?
2. 어떤 정보가 필요했는가?
3. 그 정보를 어떻게 얻었는가?
4. 관리자 인증이 필요한가?
5. 일반 사용자가 어려워할 부분은 무엇인가?
6. 자동화 가능한가?
7. 자동화 정확도는 어느 정도인가?
8. 실패 시 대체 방법은 무엇인가?
9. 프로그램 기능으로 구현하려면 무엇이 필요한가?
```

---

# 7. Mandatory PHASE 0 Matrix

다음 표를 실제 검사 결과로 채운다.

| 검사 항목 | 획득 가능 | 인증 필요 | 자동화 | 신뢰도 | 실패 시 대안 |
|---|---|---|---|---|---|
| Gateway IP | TBD | TBD | TBD | TBD | TBD |
| Gateway MAC | TBD | TBD | TBD | TBD | TBD |
| Vendor | TBD | TBD | TBD | TBD | TBD |
| Model | TBD | TBD | TBD | TBD | TBD |
| Hardware Version | TBD | TBD | TBD | TBD | TBD |
| Firmware Version | TBD | TBD | TBD | TBD | TBD |
| Firmware Latest Check | TBD | TBD | TBD | TBD | TBD |
| CVE Applicability | TBD | TBD | TBD | TBD | TBD |
| Router DNS | TBD | TBD | TBD | TBD | TBD |
| UPnP | TBD | TBD | TBD | TBD | TBD |
| WPS | TBD | TBD | TBD | TBD | TBD |
| DMZ | TBD | TBD | TBD | TBD | TBD |
| Port Forwarding | TBD | TBD | TBD | TBD | TBD |
| Remote Administration | TBD | TBD | TBD | TBD | TBD |
| Admin HTTP / HTTPS | TBD | TBD | TBD | TBD | TBD |
| LAN Devices | TBD | TBD | TBD | TBD | TBD |

---

# 8. Recommended PHASE 0 Result Format

각 검사 항목은 다음 형식으로 기록한다.

```text
Test
Firmware Detection

Target
TP-Link Archer C50 v6

Required Information
Current Firmware Version

Method
Router Admin Page

Authentication
AUTH_USER_REQUIRED

Result
1.0.0 Build XXXXX

Automation
PARTIAL

Confidence
HIGH

Pain Point
사용자가 관리자 페이지에 로그인해야 함

Automation Idea
TP-Link Router Adapter에서 Firmware API 또는 Web UI Parser 활용

Fallback
사용자에게 Firmware Version 직접 입력 요청

Requirement
TP-Link Firmware Detection Module 필요
```

---

# 9. Development Gate Policy

앞으로 각 Phase는 아래 방식으로 감사한다.

```text
PASS
CONDITIONAL PASS
FAIL
```

## PASS

다음 단계로 이동 가능.

## CONDITIONAL PASS

핵심 기능은 충족했지만 수정 또는 추가 검증 항목이 존재한다.

## FAIL

현재 단계의 성공 조건을 충족하지 못했다.

다음 단계로 이동하지 않는다.

---

# 10. Phase Gate

## Gate 0 → Gate 1

PHASE 1 개발을 시작하려면 다음 조건이 충족되어야 한다.

- [ ] Archer C50 v6 실제 수동 검사 완료
- [ ] 주요 검사 항목 획득 가능 여부 기록
- [ ] 인증 필요 여부 기록
- [ ] 자동화 가능 여부 `YES / PARTIAL / NO` 분류
- [ ] 자동 탐지 실패 시 Fallback 정의
- [ ] `requirements.md` 작성
- [ ] PHASE 1 Acceptance Criteria 작성
- [ ] v0.1 범위 밖 기능이 개발 항목에 포함되지 않았는지 확인

모든 핵심 항목이 완료된 후에만 PHASE 1 개발을 승인한다.

---

# 11. Scope Guardrail

프로젝트 진행 중 새로운 아이디어가 등장하면 다음 순서로 판단한다.

```text
새 기능 제안
     ↓
v0.1 MVP에 포함되어 있는가?
     │
  YES│
     ↓
현재 개발 후보

  NO
     ↓
Final 12 Questions 중 하나를 더 정확하게 해결하는가?
     │
 YES │        NO
     ↓         ↓
Future       제외
Backlog
```

이 규칙을 통해 프로젝트가 다음 방향으로 변질되는 것을 방지한다.

- 일반 네트워크 관리 도구
- 백신 프로그램
- 공격 도구
- 패킷 분석 전문 프로그램
- 모든 제조사를 동시에 지원하는 범용 플랫폼

---

# 12. Manager Audit Position

현재 단계에서 가장 중요한 것은 코드를 빨리 만드는 것이 아니다.

잘못된 가정을 코드로 굳히지 않는 것이 더 중요하다.

따라서 현재 프로젝트 진행 순서는 다음을 유지한다.

```text
PRD
 ↓
실제 공유기 수동 검사
 ↓
관찰
 ↓
자동화 가능성 판단
 ↓
Requirement 도출
 ↓
Acceptance Criteria 정의
 ↓
PHASE 1 개발
```

다음과 같은 순서는 허용하지 않는다.

```text
PRD
 ↓
바로 대규모 코드 작성
 ↓
공유기에서 동작하지 않음
 ↓
요구사항 재설계
```

---

# 13. Final Audit Conclusion

현재 `Router Security Checker PRD v0.1`은 **프로젝트의 기준 문서로 사용하기에 충분한 방향성을 확보했다.**

특히 다음 요소는 유지해야 한다.

```text
General User First
Evidence-Based Decision
Multi-Evidence Risk Judgment
Unknown Transparency
False Positive Reduction
Defensive-Only Scope
Manual Fix + Re-scan
```

다만 아직 구현 단계에서 필요한 세부사항은 실제 공유기 검사를 통해 확보해야 한다.

따라서 최종 감사 판정은 다음과 같다.

```text
Document Quality
PASS

Project Direction
PASS

Scope Control
PASS

PHASE 0 Entry
PASS

PHASE 1 Development
HOLD

Overall Audit
CONDITIONAL PASS
```

---

# 14. Immediate Next Action

현재 승인된 다음 작업은 하나다.

> **Test Device #001 — TP-Link Archer C50 v6 수동 보안 검사**

검사 결과를 바탕으로 다음 문서를 작성한다.

```text
manual_test_001.md
requirements.md
```

그 후 Manager Audit을 통해 `PHASE 1 — Discovery MVP` 진입 여부를 다시 판정한다.

---

## Audit Status

```text
Audit Report Version : v0.1
Source PRD Version    : v0.1
Current Stage         : Product Definition / Manual Research
Audit Result          : CONDITIONAL PASS
Next Approved Stage   : PHASE 0 — Manual Research
Development Gate      : PHASE 1 HOLD
Primary Platform      : Windows
Initial Vendor        : TP-Link
Test Device           : Archer C50 v6
```
