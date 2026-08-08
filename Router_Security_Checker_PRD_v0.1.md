# Router Security Checker
## Product Definition & Development Roadmap v0.1

> **목표:** 일반 사용자가 복잡한 네트워크 지식 없이 자신의 공유기 보안 상태를 검사하고, 프로그램이 펌웨어·취약점·설정·연결기기·외부 노출·실제 네트워크 통신을 종합 분석하여 근거 기반으로 결과를 제공하는 도구를 만든다.

---

# 1. Product Vision

Router Security Checker는 일반 사용자가 한 번의 검사로 자신의 공유기 보안 상태를 이해하고 대응할 수 있도록 하는 진단 도구다.

프로그램은 다음 정보를 종합한다.

- 펌웨어
- 공개 취약점
- 공유기 설정
- 연결된 기기
- 외부 노출
- 실제 네트워크 통신

하나의 이상 징후만으로 위험을 단정하지 않는다.

여러 증거를 조합하여 다음 상태 중 하나로 판단한다.

- `SAFE`
- `WARNING`
- `CHECK REQUIRED`
- `DANGER`

기본 화면에서는 결과를 쉽게 보여주고, 상세 화면에서는 판단 근거를 투명하게 제공한다.

문제가 발견되면 해결 방법까지 안내한다.

---

# 2. Core Principles

프로젝트의 핵심 원칙은 다음과 같다.

1. **쉬운 사용**
2. **실제 검사**
3. **다중 근거 판단**
4. **오탐 최소화**
5. **판단 근거 공개**
6. **해결 방법 제공**
7. **모르는 것은 모른다고 표시**
8. **개인정보 최소 수집**

새로운 기능을 추가할 때는 다음 질문으로 판단한다.

> 이 기능이 핵심 보안 질문 중 하나를 더 정확하게 해결하는가?

YES → 검토  
NO → 우선 제외

---

# 3. Target User

메인 사용자는 다음 수준으로 정의한다.

> PC 기본 사용은 가능하지만 네트워크·보안 전문 지식은 없는 일반 사용자

사용자가 다음 용어를 몰라도 프로그램 사용이 가능해야 한다.

- NAT
- CVE
- RCE
- DNS Hijacking
- UPnP
- ASN
- C2

---

# 4. Core User Question

프로그램이 최종적으로 답해야 하는 핵심 질문:

> **내 공유기를 지금 계속 사용해도 괜찮은가?**

이를 위해 다음 하위 질문에 답한다.

- 펌웨어가 최신인가?
- 현재 펌웨어에 알려진 취약점이 있는가?
- 위험한 공유기 설정이 존재하는가?
- 외부 인터넷에 불필요한 서비스가 노출되어 있는가?
- 모르는 장치가 네트워크에 연결되어 있는가?
- 공유기가 수상한 외부 서버와 통신하는가?
- 알려진 악성 인프라와 연결되는가?
- 외부에서 원격 제어될 가능성이 있는가?

---

# 5. Scope

## 포함

- Router Device
- Router Firmware
- Router Configuration
- LAN Devices
- WAN Exposure
- Network Traffic
- External Servers
- Known Vulnerabilities

## 제외

초기 버전에서는 다음 기능을 포함하지 않는다.

- PC 악성코드 검사
- 일반 백신 기능
- 자동 공유기 설정 변경
- 공격 코드 실행
- 취약점 실제 Exploit 수행

본 프로그램은 **방어적 진단 도구**로 개발한다.

---

# 6. Router Discovery

## Q1. 공유기 IP를 자동으로 찾을 수 있는가?

기본 방식:

```text
Default Gateway
    ↓
Router IP
```

예:

```text
192.168.0.1
```

### Requirement

- Windows Default Gateway 자동 탐색
- 활성 Network Adapter 확인
- IPv4 우선 지원
- 탐색 실패 시 사용자 입력 허용

---

## Q2. 공유기 제조사를 어떻게 식별할 것인가?

가능한 데이터:

- Gateway MAC Address
- MAC OUI Vendor
- HTTP Header
- HTTPS Certificate
- UPnP
- SSDP
- Web UI Fingerprint

### 원칙

하나의 데이터만으로 확정하지 않는다.

여러 증거를 조합한다.

예:

```text
Vendor: TP-Link
Confidence: 96%
```

---

## Q3. 정확한 모델명을 어떻게 찾을 것인가?

목표:

```text
TP-Link
Archer C50
Hardware V6
```

가능한 방법:

- UPnP Device Description
- Web UI 분석
- Router API
- HTTP Fingerprint
- 제조사별 API
- 사용자 입력

### Fallback

```text
자동 탐지
   ↓ 실패
후보 모델 표시
   ↓ 실패
사용자 직접 입력
```

---

# 7. Firmware Inspection

## Q4. 현재 펌웨어 버전을 어떻게 수집할 것인가?

가능한 방법:

- Router API
- Web UI
- UPnP
- 관리자 페이지 Parsing
- 사용자 입력

예:

```text
0.9.1 0.5 v0001.0
Build 230810
```

---

## Q5. 최신 펌웨어 정보는 어디서 가져올 것인가?

우선순위:

1. 제조사 공식 사이트
2. 제조사 공식 API
3. 프로그램 내부 Cache DB
4. 제3자 보조 데이터

### 원칙

공식 제조사 데이터를 최우선으로 한다.

---

## Q6. 오래된 펌웨어는 무조건 위험한가?

아니다.

판단 시 다음 항목을 함께 본다.

```text
Firmware Age
+
Security Fix Included
+
Known Vulnerability
+
Exploitability
```

단순히 오래됐다는 이유만으로 `DANGER` 처리하지 않는다.

---

# 8. Vulnerability Intelligence

## Q7. 어떤 취약점 데이터를 사용할 것인가?

후보:

- Manufacturer Security Advisory
- CISA KEV
- NVD
- CVE
- 기타 신뢰 가능한 보안 데이터

우선순위:

```text
Manufacturer Advisory
        ↓
CISA KEV
        ↓
NVD / CVE
```

---

## Q8. CVE가 실제 장비에 적용되는지 어떻게 판단할 것인가?

반드시 다음 정보를 함께 비교한다.

```text
Manufacturer
Model
Hardware Revision
Firmware Version
Region
```

모델명만 같다고 취약하다고 판단하지 않는다.

---

## Q9. CVSS 점수만으로 위험도를 결정할 것인가?

아니다.

다음 요소를 함께 평가한다.

- Severity
- Exploitability
- Authentication Required
- WAN Exposure
- Known Exploitation
- Firmware Applicability
- Required Attack Conditions

---

# 9. Router Configuration Audit

검사 대상 설정:

- Remote Administration
- UPnP
- DMZ
- Port Forwarding
- DNS
- WPS
- HTTP / HTTPS Admin Interface
- 관리자 계정 보안

---

## Q10. Remote Administration이 켜져 있는가?

검사:

```text
Internet
   ↓
Router Admin Interface
```

WAN에서 관리자 페이지 접근이 가능하면 위험도 증가.

---

## Q11. UPnP가 활성화되어 있는가?

UPnP ON 자체만으로 위험 판정하지 않는다.

함께 확인:

```text
UPnP ON
+
Actual Port Mapping
+
Requesting Device
```

---

## Q12. DMZ가 설정되어 있는가?

예:

```text
DMZ
→ 192.168.0.10
```

DMZ 대상 장치가 무엇인지 확인한다.

---

## Q13. Port Forwarding이 존재하는가?

예:

```text
WAN:8080
    ↓
192.168.0.5:80
```

사용자가 만든 설정인지 자동 판단이 어렵다면:

- Known
- Unknown

으로 구분한다.

---

## Q14. DNS 설정은 정상인가?

검사:

- Router DNS
- DHCP DNS
- Client DNS

분류 예:

- ISP DNS
- Google
- Cloudflare
- Known Provider
- Unknown

알려지지 않은 DNS라고 바로 악성으로 판단하지 않는다.

---

## Q15. WPS가 활성화되어 있는가?

활성 상태를 보안 Warning 요소로 포함한다.

---

## Q16. 관리자 페이지는 HTTPS를 지원하는가?

가능한 상태:

- HTTPS
- HTTP Only
- HTTP → HTTPS Redirect

HTTP Only일 경우 Warning 요소로 처리할 수 있다.

---

# 10. LAN Device Discovery

## Q17. 같은 네트워크의 장치를 어떻게 탐색할 것인가?

후보:

- ARP
- ICMP
- DHCP
- mDNS
- SSDP

결과 예:

```text
192.168.0.2    PC
192.168.0.3    Samsung TV
192.168.0.8    Unknown
```

---

## Q18. Unknown Device는 위험한가?

즉시 위험 처리하지 않는다.

기본 판정:

```text
Unknown Device
→ CHECK REQUIRED
```

사용자에게 질문 가능:

> 이 장치를 알고 있습니까?

---

## Q19. 장치 제조사를 추정할 것인가?

MAC OUI 활용.

예:

- Apple
- Samsung
- LG
- Espressif
- TP-Link

---

# 11. WAN Exposure

## Q20. 외부에서 어떤 서비스가 노출되어 있는가?

검사 후보:

- FTP 21
- SSH 22
- Telnet 23
- HTTP 80
- HTTPS 443
- TR-069 7547
- 8080
- 8443
- 기타 탐지된 포트

### 원칙

```text
Open Port ≠ Vulnerable
```

포트 존재와 실제 취약성을 구분한다.

---

## Q21. LAN 내부 검사만으로 WAN 노출을 알 수 있는가?

항상 그렇지는 않다.

NAT Loopback 등의 영향이 존재한다.

향후 구조:

```text
Router Security Checker
        ↓
External Scan Service
        ↓
User Public IP
```

외부 검사 사용 시 사용자 동의를 받는다.

---

# 12. Traffic Analysis

이 기능은 프로젝트의 핵심 차별점이다.

## Q22. 공유기 자체의 WAN 통신을 PC에서 볼 수 있는가?

일반 구조:

```text
PC
 ↓
Router
 ↓
Internet
```

PC에서는 공유기 자체가 만드는 모든 WAN 통신을 직접 볼 수 없을 수 있다.

---

## Q23. Router-originated Traffic을 어떻게 캡처할 것인가?

후보:

### A. Router Log / API

장점:
- 일반 사용자에게 가장 간단

단점:
- 제조사마다 기능 차이

### B. Managed Switch Port Mirroring

장점:
- 정확한 패킷 관찰

단점:
- 일반 사용자에게 어려움

### C. PC Gateway / Bridge Mode

```text
Internet
   ↓
Scanner PC
   ↓
Router
```

장점:
- 강력한 분석 가능

단점:
- 설정 복잡

### D. 별도 Hardware

예:

- Raspberry Pi
- Network Tap

초기 MVP에서는 우선 연구 대상으로 둔다.

---

# 13. Traffic Metadata

## Q24. 어떤 데이터를 수집할 것인가?

최소 항목:

```text
Timestamp
Source IP
Destination IP
Destination Port
Protocol
Domain
Packet Count
Bytes
Connection Frequency
```

---

## Q25. Payload까지 저장할 것인가?

기본 정책:

> Payload는 저장하지 않는다.

Metadata 중심 분석.

---

## Q26. DNS Query를 분석할 것인가?

YES.

예:

```text
ntp.tp-link.com
time.google.com
unknown-domain.example
```

도메인은 IP보다 사용자 설명에 유리하다.

---

## Q27. HTTPS 통신을 복호화할 것인가?

기본적으로 하지 않는다.

대신 다음 Metadata 활용:

- Destination IP
- SNI
- Certificate
- Connection Pattern
- Frequency
- Data Volume

---

# 14. External Server Analysis

## Q28. 특정 국가 IP이면 위험한가?

아니다.

핵심 원칙:

```text
Country ≠ Malicious
```

GeoIP는 참고 정보일 뿐이다.

---

## Q29. 외부 서버 판단에 어떤 정보를 사용할 것인가?

- IP Reputation
- Domain Reputation
- ASN
- Organization
- GeoIP
- Connection Frequency
- Data Volume
- Known Router Infrastructure
- Known Malware C2
- Threat Intelligence

---

## Q30. 정상 서버 Allowlist를 만들 것인가?

가능한 예:

- TP-Link
- Google
- Microsoft
- AWS
- Cloudflare
- NTP Pool
- ISP

하지만 Allowlist만으로 안전을 확정하지 않는다.

---

## Q31. 반복 통신은 언제 이상한가?

예:

```text
35초마다 접속
```

이 사실만으로는 악성이라고 판단할 수 없다.

다음 요소를 함께 본다.

```text
Frequency
+
Destination Reputation
+
Protocol
+
Data Volume
+
Expected Router Behavior
```

---

# 15. Backdoor Suspicion

## Q32. 어떤 조건에서 Backdoor 의심으로 분류할 것인가?

단일 증거가 아니라 복수 증거 필요.

예:

```text
Unknown External Server
+
Persistent Communication
+
Known Malicious Reputation
+
Vulnerable Firmware
+
Unexpected Inbound Command Channel
```

---

## Q33. 언제 "백도어 발견"이라고 표현할 수 있는가?

매우 높은 증거 수준이 필요하다.

일반적인 결과는 다음 표현을 우선한다.

```text
Suspicious Communication
```

또는

```text
Further Investigation Required
```

확증이 없는 상태에서 `Backdoor Detected`라고 단정하지 않는다.

---

# 16. Risk Engine

## Q34. 보안 점수제를 사용할 것인가?

예:

```text
Start: 100

Critical CVE       -30
Remote Admin       -15
Outdated Firmware  -10
UPnP                -3
Unknown Device      -2
Known Malicious C2 -50
```

단순 합산만 사용하지 않는다.

---

## Q35. Critical Rule을 둘 것인가?

YES.

예:

```text
Known Exploited RCE
+
WAN Exposure
```

같은 조합은 점수와 관계없이 `DANGER`로 승격할 수 있다.

---

## Q36. 최종 상태는 몇 단계로 할 것인가?

기본:

```text
SAFE

WARNING

CHECK REQUIRED

DANGER
```

`CHECK REQUIRED`를 유지한다.

---

# 17. Confidence System

## Q37. 판단의 신뢰도를 표시할 것인가?

YES.

예:

```text
Risk: WARNING
Confidence: HIGH
```

또는

```text
Firmware Status: UNKNOWN

Reason:
Router API does not expose firmware version.
```

프로그램이 모르는 정보를 숨기지 않는다.

---

# 18. User Interface

## Q38. 첫 화면에는 무엇을 보여줄 것인가?

최소 정보:

```text
Router Security

82 / 100

WARNING

2 problems found
```

---

## Q39. 세부 상태는 어떻게 표현할 것인가?

예:

```text
Firmware       ⚠
Vulnerability  ✓
Configuration  ⚠
Devices        ✓
Exposure       ✓
Traffic        ✓
```

---

## Q40. 전문가용 상세 화면을 제공할 것인가?

YES.

예:

```text
Destination
52.xxx.xxx.xxx

ASN
Microsoft

Port
443

Frequency
12 connections / hour

Verdict
Expected
```

---

# 19. Remediation

## Q41. 문제만 알려줄 것인가?

아니다.

각 문제마다 해결 방법을 제공한다.

예:

```text
UPnP is enabled

How to fix:
관리자 페이지에서 UPnP를 비활성화하십시오.
```

---

## Q42. 프로그램이 공유기 설정을 자동으로 수정할 것인가?

초기 버전:

```text
NO
```

흐름:

```text
문제 발견
   ↓
해결 방법 안내
   ↓
사용자 직접 수정
   ↓
Re-scan
```

향후 안전성이 충분히 검증되면 자동 수정 기능을 검토한다.

---

# 20. Privacy

## Q43. 패킷 데이터는 어디에 저장할 것인가?

기본:

```text
Local Only
```

---

## Q44. 외부 서버로 전송 가능한 정보는 무엇인가?

가능한 최소 정보:

```text
Router Model
Firmware Version
CVE Query
IP Reputation Query
```

개인 패킷 Payload는 기본적으로 전송하지 않는다.

---

## Q45. Public IP 사용이 필요한 경우는?

외부 WAN Exposure 검사 등에서 필요할 수 있다.

이 경우:

- 기능 설명
- 사용자 명시적 동의
- 최소 보관
- 가능하면 즉시 폐기

원칙을 적용한다.

---

# 21. Proposed Architecture

```text
Router Security Checker
│
├─ Discovery Engine
│   ├─ Gateway Detection
│   ├─ Router Detection
│   └─ Vendor Detection
│
├─ Router Adapter
│   ├─ TP-Link
│   ├─ ASUS
│   ├─ ipTIME
│   └─ Netgear
│
├─ Firmware Engine
│
├─ Vulnerability Engine
│
├─ Configuration Auditor
│
├─ Device Scanner
│
├─ Exposure Scanner
│
├─ Traffic Analyzer
│
├─ Threat Intelligence
│
├─ Risk Engine
│
└─ Report Engine
```

---

# 22. Router Adapter Design

제조사 확장을 위해 Adapter 구조를 사용한다.

```text
BaseRouterAdapter
        │
        ├── TPLinkAdapter
        ├── AsusAdapter
        ├── IptimeAdapter
        └── NetgearAdapter
```

공통 Interface 후보:

```text
detect()
get_model()
get_hardware_version()
get_firmware()
get_dns()
get_upnp()
get_remote_admin()
get_port_forwarding()
get_dmz()
get_wps()
```

---

# 23. Manual Test Strategy

첫 번째 실제 장비:

```text
Test User #001
일반 사용자

Test Device #001
TP-Link Archer C50 v6
```

직접 검사하면서 모든 단계에서 기록한다.

핵심 질문:

```text
Can it be automated?
YES / PARTIAL / NO
```

기록 항목:

```text
Input
Method
Result
Confidence
Pain Point
Automation Idea
Requirement
```

예:

```text
Test
Firmware Detection

Input
Archer C50 v6

Current Method
관리자 페이지 직접 확인

Automation
PARTIAL

Pain Point
사용자가 메뉴를 찾아야 함

Requirement
TP-Link Firmware API Parser 필요
```

---

# 24. Development Roadmap

## PHASE 0 — Manual Research

### 목표

코드부터 작성하지 않는다.

실제 Archer C50 v6를 직접 검사한다.

### 산출물

```text
manual_test_001.md
requirements.md
```

---

## PHASE 1 — Discovery MVP

구현:

- Gateway Detection
- Router IP Detection
- MAC Address
- Vendor Detection
- LAN Device Discovery

### 성공 기준

프로그램이 사용자의 기본 네트워크 구조를 자동으로 파악한다.

---

## PHASE 2 — Router Identification

구현:

- Manufacturer
- Model
- Hardware Version
- Firmware Version

초기 제조사:

```text
TP-Link
```

### 성공 기준

Archer C50 v6를 자동 또는 반자동으로 정확히 식별한다.

---

## PHASE 3 — Security Intelligence

구현:

- Firmware Check
- CVE Check
- Manufacturer Advisory
- CISA KEV

### 성공 기준

현재 펌웨어와 관련 취약점을 정확히 연결한다.

---

## PHASE 4 — Local Security Audit

구현:

- DNS
- UPnP
- WPS
- DMZ
- Port Forwarding
- Remote Administration
- LAN Devices

### 성공 기준

위험한 공유기 설정을 자동 또는 반자동으로 판별한다.

---

## PHASE 5 — Risk Engine

구현:

- Rule Engine
- Severity
- Confidence
- Evidence
- Security Score
- Critical Rules

### 성공 기준

여러 검사 결과를 하나의 보안 상태로 종합한다.

여기까지를 `v0.1` 목표로 한다.

---

## PHASE 6 — Traffic Analyzer

구현:

- DNS Monitoring
- Connections
- Destination IP
- Domain
- Port
- Frequency
- Data Volume

---

## PHASE 7 — Threat Intelligence

구현:

- ASN
- Organization
- GeoIP
- Reputation
- Known C2
- Known Router Infrastructure

---

## PHASE 8 — Advanced WAN Analysis

구현:

- External Port Scan
- Router-Originated Traffic Detection
- Suspicious Command Channel Detection
- Backdoor Behavior Heuristics

---

## PHASE 9 — GUI

목표:

```text
[ Scan Router ]

       ↓

Security Score

82

WARNING
```

일반 사용자가 별도 네트워크 지식 없이 사용할 수 있는 GUI를 만든다.

---

# 25. MVP Definition

초기 `v0.1` 범위:

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

실시간 Router WAN Traffic 분석은 이후 버전으로 분리한다.

---

# 26. Final 12 Questions

프로젝트 전체에서 반드시 답해야 하는 핵심 질문이다.

1. **무엇을 안전하다고 정의하는가?**
2. **어떤 정보를 자동으로 수집할 수 있는가?**
3. **정확한 공유기 모델과 펌웨어를 어떻게 식별하는가?**
4. **어떤 취약점 데이터를 신뢰할 것인가?**
5. **어떤 공유기 설정을 위험하다고 판단하는가?**
6. **어떤 외부 노출을 위험하다고 판단하는가?**
7. **공유기 자체의 외부 통신을 어떻게 관찰하는가?**
8. **정상 통신과 악성 통신을 어떻게 구분하는가?**
9. **여러 증거를 어떻게 하나의 위험도로 합치는가?**
10. **모르는 것은 어떻게 '모른다'고 표현하는가?**
11. **일반 사용자에게 결과를 어떻게 설명하는가?**
12. **발견한 문제를 사용자가 어떻게 해결하게 할 것인가?**

---

# 27. Decision Rule

새로운 기능 또는 아이디어가 등장하면 반드시 확인한다.

> **이 기능이 위 12개 질문 중 하나를 더 정확하게 해결하는가?**

```text
YES
→ 프로젝트 기능 후보

NO
→ Backlog 또는 제외
```

이 규칙을 통해 프로젝트가 단순 네트워크 도구, 백신, 해킹 도구 등 다른 방향으로 확장되는 것을 방지한다.

---

# 28. Next Step

다음 작업은 기능 추가가 아니다.

먼저 `Test Device #001 — TP-Link Archer C50 v6`를 실제로 수동 검사한다.

각 검사 단계에서 다음을 기록한다.

```text
1. 무엇을 확인했는가?
2. 어떤 정보가 필요했는가?
3. 그 정보를 어떻게 얻었는가?
4. 일반 사용자가 어려워한 부분은 무엇인가?
5. 자동화 가능한가?
6. 어떤 프로그램 기능이 필요해졌는가?
```

이 결과를 기반으로 실제 개발 요구사항을 확정한다.

---

## Status

```text
Document Version : v0.1
Project Stage    : Product Definition / Manual Research
Primary Platform : Windows
Initial Vendor   : TP-Link
Test Device      : Archer C50 v6
```
