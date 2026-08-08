# Router Security Checker
## Internet Research PRD v0.1

> **목적:** Router Security Checker를 구현하기 전에 인터넷에 공개된 공식 문서, 보안 권고문, 취약점 DB, 기술 문서, 오픈소스 프로젝트 및 보안 연구자료를 조사하여 **무엇을 검사해야 하는지, 어떻게 검사할 수 있는지, 어떤 데이터가 신뢰 가능한지**를 확정한다.

---

# 1. Research Mission

본 조사의 최종 목적은 단순히 공유기 관련 정보를 많이 수집하는 것이 아니다.

다음 흐름을 만드는 것이 목적이다.

```text
인터넷 정보
    ↓
검증된 사실
    ↓
검사 가능한 항목
    ↓
자동화 가능성 판단
    ↓
Router Security Checker 요구사항
```

조사 결과는 반드시 다음 질문으로 연결되어야 한다.

> 이 정보를 이용해서 프로그램이 사용자의 공유기를 더 정확하게 검사할 수 있는가?

YES  
→ 개발 요구사항 후보

NO  
→ 참고자료 또는 Backlog

---

# 2. Initial Research Target

초기 조사 범위를 제한한다.

```text
OS
Windows

Vendor
TP-Link

Test Device
Archer C50

Hardware Revision
V6
```

초기 단계에서는 모든 공유기 제조사를 조사하지 않는다.

먼저 Archer C50 v6 한 대를 기준으로 전체 검사 과정을 완성한다.

---

# 3. Primary Research Questions

인터넷 조사를 통해 다음 질문에 답해야 한다.

## RQ1. Archer C50 v6를 어떻게 정확하게 식별할 수 있는가?

찾아야 할 정보:

- MAC OUI
- 기본 Gateway 정보
- UPnP Device Description
- SSDP 응답
- HTTP Header
- HTTPS Certificate
- 관리자 Web UI 특징
- TP-Link 내부 API
- 모델명 반환 API
- Hardware Version 반환 API

목표:

```text
Unknown Router
    ↓
TP-Link
Archer C50
Hardware V6
```

---

## RQ2. Firmware Version을 자동으로 얻을 방법이 존재하는가?

찾아야 할 정보:

- TP-Link 관리자 페이지 구조
- Firmware 표시 페이지
- API endpoint
- JavaScript 내부 API 호출
- CGI endpoint
- JSON endpoint
- Authentication 방식
- Session / Token 구조
- UPnP에서 Firmware 정보를 제공하는지 여부

조사 결과는 다음과 같이 분류한다.

```text
AUTO
PARTIAL
MANUAL
IMPOSSIBLE
```

---

## RQ3. 최신 Firmware를 어디서 확인할 수 있는가?

찾아야 할 정보:

- TP-Link 공식 다운로드 페이지
- 국가별 TP-Link 사이트
- Hardware Revision별 Firmware
- Firmware Release Date
- Firmware Version
- Build Number
- Security Fix Notes
- Region 차이

특히 다음을 확인한다.

```text
Archer C50 V6 KR
Archer C50 V6 US
Archer C50 V6 EU
```

Firmware가 지역별로 다른지 조사한다.

---

# 4. Vulnerability Research

## RQ4. Archer C50 계열에 알려진 CVE가 존재하는가?

검색 대상:

```text
"TP-Link Archer C50 CVE"
"Archer C50 V6 vulnerability"
"Archer C50 firmware vulnerability"
site:nvd.nist.gov Archer C50
site:cve.org Archer C50
```

찾아야 할 정보:

- CVE ID
- 취약한 모델
- Hardware Revision
- Firmware Version
- 영향 범위
- 공격 조건
- 인증 필요 여부
- LAN 공격인지 WAN 공격인지
- RCE 여부
- Authentication Bypass
- Command Injection
- Buffer Overflow
- Information Disclosure
- CSRF
- XSS

---

# 5. Vulnerability Source Priority

취약점 정보는 출처별 신뢰도를 구분한다.

## Tier 1 — 최우선

```text
TP-Link Security Advisory
CISA
CISA KEV
NVD
CVE.org
CERT
```

## Tier 2 — 보안 연구기관

예:

```text
Cisco Talos
Trend Micro / ZDI
Fortinet
Palo Alto Unit 42
Check Point Research
Kaspersky
Rapid7
Tenable
Qualys
```

## Tier 3 — 연구자료

```text
Academic Paper
Security Conference
Research Blog
Vendor-independent analysis
```

## Tier 4 — 참고 자료

```text
GitHub
Reddit
Forum
Blog
YouTube
Community Post
```

Tier 4 자료만으로 취약하다고 판정해서는 안 된다.

---

# 6. CVE Applicability Research

가장 중요한 조사 중 하나다.

다음과 같은 자료를 발견했다고 가정한다.

```text
CVE-XXXX-XXXX
TP-Link Archer C50 vulnerable
```

이것만으로 프로그램이

```text
DANGER
```

라고 판단해서는 안 된다.

반드시 확인한다.

```text
Vendor
Model
Hardware Revision
Firmware
Build
Region
Attack Surface
Authentication Requirement
```

조사 결과는 다음 구조로 저장한다.

```text
CVE
CVE-XXXX-XXXX

Vendor
TP-Link

Model
Archer C50

Hardware
V6

Firmware
x.x.x

Region
EU / US / KR / Unknown

Attack Vector
LAN / WAN

Authentication
Required / Not Required

Known Exploited
YES / NO / UNKNOWN
```

---

# 7. CISA KEV Research

CISA Known Exploited Vulnerabilities에 해당 취약점이 존재하는지 확인한다.

검색:

```text
site:cisa.gov TP-Link
site:cisa.gov Archer
CISA KEV TP-Link router
```

찾아야 할 정보:

```text
Known Exploited
YES / NO

Date Added

Required Action

Affected Product

Vulnerability Type
```

KEV 등재 여부는 Risk Engine의 강력한 증거로 사용한다.

---

# 8. Router Configuration Research

다음 설정 각각에 대해 조사한다.

```text
Remote Administration
UPnP
WPS
DMZ
Port Forwarding
DNS
HTTP Admin
HTTPS Admin
Administrator Account
```

각 설정별로 두 가지를 조사해야 한다.

### A. 보안적으로 왜 문제가 되는가?

### B. Archer C50에서 어떻게 확인할 수 있는가?

예:

```text
UPnP

Security Meaning
UPnP enabled 자체는 취약점이 아님

Router Location
Advanced → NAT Forwarding → UPnP

Automatic Detection
API 가능 여부 조사
```

---

# 9. TP-Link Web Interface Research

매우 중요한 조사 대상이다.

Archer C50 관리자 페이지의 동작 방식을 조사한다.

찾아야 할 것:

```text
Login URL
Authentication Method
Session Token
Cookies
API Endpoint
XHR Request
JSON Response
Firmware API
Device List API
DHCP API
DNS API
UPnP API
Port Forwarding API
DMZ API
WPS API
Remote Management API
```

특히 JavaScript 파일을 조사한다.

웹 UI가 정보를 표시한다는 것은 대부분 어디선가 데이터를 가져오고 있다는 의미이기 때문이다.

```text
Web UI
   ↓
JavaScript
   ↓
HTTP Request
   ↓
Router API
   ↓
JSON / CGI Response
```

이 구조를 찾으면 Router Adapter 개발이 크게 쉬워질 수 있다.

---

# 10. Existing Open Source Research

이미 비슷한 작업을 수행하는 프로그램이 존재하는지 조사한다.

검색 예:

```text
GitHub router scanner
GitHub router security scanner
GitHub TP-Link API
GitHub Archer C50 API
GitHub router fingerprint
GitHub UPnP device detection
GitHub TP-Link router client
GitHub router vulnerability scanner
```

찾아야 할 것은 코드를 그대로 가져오는 것이 아니다.

다음 아이디어를 조사한다.

```text
Router Identification 방법
Firmware Detection 방법
Authentication 방식
API Endpoint
Device Discovery 방식
CVE Matching 구조
Report 구조
```

---

# 11. Router Fingerprinting Research

다음 기술을 각각 조사한다.

```text
ARP
MAC OUI
UPnP
SSDP
HTTP Header
HTTPS Certificate
HTML Title
Favicon Hash
Web UI Fingerprint
mDNS
```

각 방식마다 기록한다.

```text
Method
Information Available
Accuracy
Authentication Required
Network Permission Required
False Positive Risk
Archer C50 Support
Automation Difficulty
```

---

# 12. MAC OUI Research

조사 대상:

```text
IEEE OUI database
TP-Link MAC prefixes
```

목적:

```text
Gateway MAC
    ↓
OUI
    ↓
TP-Link 추정
```

하지만 원칙:

```text
OUI = Vendor Evidence
OUI ≠ Exact Router Model
```

따라서 OUI 하나만으로 모델을 확정하지 않는다.

---

# 13. UPnP / SSDP Research

찾아야 할 정보:

```text
SSDP Discovery
M-SEARCH
239.255.255.250:1900
Device Description XML
```

Device Description에서 다음 정보가 제공되는지 조사한다.

```text
manufacturer
manufacturerURL
modelDescription
modelName
modelNumber
serialNumber
UDN
friendlyName
```

Archer C50 v6가 실제로 무엇을 반환하는지가 핵심이다.

---

# 14. LAN Device Discovery Research

조사 방식:

```text
ARP Table
ICMP Scan
DHCP Client List
mDNS
SSDP
```

비교해야 한다.

| 방법 | 장점 | 단점 |
|---|---|---|
| ARP | 빠름 | 최근 통신 장치 중심 |
| ICMP | 간단 | Ping 차단 가능 |
| DHCP | 정확한 경우 많음 | Router 접근 필요 |
| mDNS | 장치명 확보 가능 | 일부 장치만 지원 |
| SSDP | IoT 탐지 유용 | 지원 장치 제한 |

최종적으로 여러 방식을 병합하는 구조를 검토한다.

---

# 15. WAN Exposure Research

조사해야 할 개념:

```text
Remote Administration
WAN Port Exposure
NAT
Port Forwarding
DMZ
UPnP Mapping
NAT Loopback
CGNAT
```

특히 다음 질문에 답해야 한다.

> Windows PC가 공유기 내부에 있을 때 자기 공유기의 WAN 노출을 정확하게 검사할 수 있는가?

불가능하거나 정확도가 낮다면 외부 Scanner Service가 필요한지 조사한다.

---

# 16. Default Dangerous Ports Research

다음 포트가 Router에서 어떤 의미를 가지는지 조사한다.

```text
21 FTP
22 SSH
23 Telnet
53 DNS
80 HTTP
443 HTTPS
1900 SSDP
7547 TR-069
8080 HTTP Alternate
8443 HTTPS Alternate
```

중요 원칙:

```text
OPEN PORT
≠
VULNERABILITY
```

따라서 각 포트에 대해

```text
Expected Service
Authentication
WAN/LAN
Known Vulnerabilities
Router Usage
```

를 함께 조사한다.

---

# 17. DNS Security Research

조사 항목:

```text
Router DNS
DHCP DNS
Client DNS
DNS Hijacking
Malicious DNS
DNS Provider Identification
```

DNS 서버를 다음과 같이 분류할 방법을 조사한다.

```text
Known Public DNS
ISP DNS
Router Proxy
Unknown DNS
Known Malicious DNS
```

Unknown만으로 악성 판정하지 않는다.

---

# 18. Traffic Analysis Research

MVP 이후 기능이지만 지금부터 가능성을 조사한다.

핵심 질문:

> 일반 Windows PC만으로 Router 자체의 WAN Traffic을 관찰할 수 있는가?

다음 방식 조사:

```text
Router System Log
Router Traffic Statistics
Router API
SNMP
Port Mirroring
Network TAP
Bridge Mode
Transparent Gateway
Packet Capture
```

각각 기록:

```text
Accuracy
Hardware Requirement
User Difficulty
Router Modification Required
Automation
Privacy Impact
```

---

# 19. Threat Intelligence Research

향후 Traffic Analyzer에서 Destination을 판단하기 위한 데이터 소스를 조사한다.

필요 정보:

```text
IP Reputation
Domain Reputation
ASN
Organization
GeoIP
Known Malware C2
Known Botnet
Known Router Infrastructure
```

후보 데이터 소스마다 기록한다.

```text
Provider
Free / Paid
API Available
Rate Limit
Commercial Use
Privacy
Accuracy
Update Frequency
```

---

# 20. Normal Router Traffic Research

악성 트래픽을 찾기 전에 정상 트래픽을 알아야 한다.

조사 대상:

```text
TP-Link Cloud
Firmware Update Server
NTP
DNS
Telemetry
DDNS
Remote Management
Cloud Management
```

예:

```text
Router → NTP Server
Router → Firmware Server
Router → TP-Link Cloud
```

이러한 정상 통신을 모르면 단순 반복 접속을 악성으로 오판할 수 있다.

---

# 21. Backdoor Research

검색어:

```text
TP-Link router backdoor
TP-Link Archer backdoor analysis
router persistent C2
router botnet TP-Link
Archer malware
Archer C50 botnet
TP-Link compromised router
```

하지만 조사 결과는 다음 세 수준으로 분리한다.

```text
CONFIRMED
REPORTED
SPECULATION
```

언론 기사 또는 커뮤니티 게시물 하나만으로 `Backdoor`라고 확정하지 않는다.

---

# 22. Malware / Botnet Research

공유기 공격 사례를 조사한다.

예:

```text
Mirai
Mozi
Mēris
VPNFilter
```

목적은 Malware Scanner를 만드는 것이 아니다.

다음 패턴을 이해하기 위함이다.

```text
Router Infection Method
Persistence
C2 Communication
Ports
Protocols
Known IOC
Exploited CVE
```

이 정보를 향후 Traffic Risk Rule에 활용한다.

---

# 23. Search Method

조사는 일반 Google 검색 하나로 끝내지 않는다.

다음 순서로 수행한다.

```text
1. 제조사 공식 문서
    ↓
2. CVE / NVD
    ↓
3. CISA / CERT
    ↓
4. 보안 연구기관
    ↓
5. 논문
    ↓
6. GitHub
    ↓
7. 커뮤니티
    ↓
8. 교차 검증
```

---

# 24. Search Query Strategy

하나의 주제에 여러 형태의 검색어를 사용한다.

### Firmware

```text
TP-Link Archer C50 V6 firmware
Archer C50 V6 download
site:tp-link.com Archer C50 V6 firmware
```

### API

```text
Archer C50 API
TP-Link web API Archer C50
Archer C50 endpoint
Archer C50 CGI
Archer C50 JavaScript API
```

### Vulnerability

```text
Archer C50 CVE
Archer C50 V6 CVE
TP-Link Archer C50 vulnerability
site:nvd.nist.gov Archer C50
```

### GitHub

```text
site:github.com "Archer C50"
site:github.com "TP-Link API"
site:github.com TP-Link router scanner
```

---

# 25. Evidence Recording Format

모든 조사 결과는 동일한 구조로 저장한다.

```text
Research ID
RES-001

Topic
Firmware Detection

Question
Archer C50 v6 firmware를 자동으로 읽을 수 있는가?

Finding
관리자 Web UI에서 Firmware Version 확인 가능

Source
URL

Source Type
Manufacturer / CVE / Research / GitHub / Community

Confidence
HIGH / MEDIUM / LOW

Verified
YES / NO

Applicable Device
Archer C50 v6

Automation
YES / PARTIAL / NO

Development Impact
TPLinkAdapter.get_firmware() 구현 후보

Notes
추가 인증 분석 필요
```

---

# 26. Evidence Confidence

## HIGH

```text
Manufacturer Documentation
CISA
NVD
CVE
Multiple Independent Sources
Actual Device Verification
```

## MEDIUM

```text
Reliable Security Research
GitHub Implementation
Single Technical Analysis
```

## LOW

```text
Forum
Reddit
Blog
Unverified Claim
Old Post
```

---

# 27. Important Research Rule

인터넷에서 발견했다고 사실로 기록하지 않는다.

반드시 구분한다.

```text
SOURCE CLAIM
VERIFIED FACT
TEST DEVICE RESULT
```

예:

```text
SOURCE CLAIM
Archer C50에 특정 API가 존재한다.

↓

DEVICE TEST
실제 Archer C50 v6에서 요청

↓

VERIFIED
API 존재 확인
```

이 과정을 통해 잘못된 인터넷 정보를 프로그램 요구사항으로 가져오는 것을 막는다.

---

# 28. Automation Evaluation

모든 조사 항목에 다음 평가를 붙인다.

```text
AUTO
PARTIAL
MANUAL
NOT POSSIBLE
```

### AUTO

사용자 개입 없이 프로그램이 검사 가능.

### PARTIAL

로그인 또는 일부 사용자 입력 필요.

### MANUAL

사용자가 관리자 페이지 등에서 직접 확인해야 함.

### NOT POSSIBLE

현재 환경에서는 신뢰성 있게 검사 불가능.

---

# 29. Research Deliverables

PHASE 0 종료 시 최소 다음 문서를 만든다.

```text
research_index.md
router_identity_research.md
firmware_research.md
vulnerability_research.md
configuration_research.md
lan_discovery_research.md
wan_exposure_research.md
traffic_research.md
source_database.md
manual_test_001.md
requirements.md
```

---

# 30. Research → Requirement Conversion

조사 결과가 바로 기능이 되는 것은 아니다.

다음 흐름을 거친다.

```text
Finding
    ↓
Evidence
    ↓
Device Verification
    ↓
Automation Evaluation
    ↓
Requirement
```

예:

```text
Finding
Archer C50 API에서 firmware version 반환

↓

Verification
실제 V6에서 확인

↓

Automation
YES

↓

Requirement
TPLinkAdapter.get_firmware()
```

---

# 31. First Research Sprint

첫 번째 조사에서는 범위를 매우 좁게 잡는다.

## Sprint 001 — Archer C50 v6 Identity

찾기:

```text
1. 공식 제품 페이지
2. Hardware Revision 체계
3. Firmware Download 페이지
4. 최신 Firmware
5. 관리자 페이지 구조
6. Router Login 방식
7. UPnP 정보
8. SSDP 응답
9. Router API 존재 여부
10. Model/Firmware 자동 탐지 방법
```

---

# 32. Second Research Sprint

## Sprint 002 — Archer C50 v6 Vulnerability

찾기:

```text
1. CVE
2. NVD
3. CISA KEV
4. TP-Link Security Advisory
5. CERT
6. Security Research
7. Exploited Vulnerabilities
8. Hardware/Firmware Applicability
```

Exploit 코드를 실행하는 것이 아니라 존재와 영향을 조사한다.

---

# 33. Third Research Sprint

## Sprint 003 — Configuration Audit

각 항목에 대해

```text
Remote Admin
UPnP
WPS
DMZ
Port Forwarding
DNS
HTTP/HTTPS Admin
```

다음을 조사한다.

```text
Security Meaning
Archer C50 Menu Location
API
Automatic Detection
Risk Rule
```

---

# 34. Fourth Research Sprint

## Sprint 004 — LAN / WAN

조사:

```text
Gateway Detection
ARP
OUI
SSDP
mDNS
DHCP Device List
Port Forwarding
WAN Exposure
NAT Loopback
CGNAT
```

---

# 35. Fifth Research Sprint

## Sprint 005 — Traffic Feasibility

아직 구현하지 않는다.

다음 질문만 확정한다.

```text
Router Traffic을 PC에서 볼 수 있는가?
Router Log가 충분한가?
TP-Link API로 외부 Connection을 볼 수 있는가?
추가 Hardware가 필요한가?
일반 사용자에게 현실적인가?
```

---

# 36. Research Stop Condition

무한정 조사하지 않는다.

각 기능에 다음 세 질문에 답하면 조사를 종료할 수 있다.

```text
1. 무엇을 검사해야 하는가?
2. 어떤 데이터로 검사할 수 있는가?
3. 자동화 가능한가?
```

답이 확보되면 개발 단계로 넘긴다.

---

# 37. PHASE 0 Completion Criteria

다음 조건이 충족되면 Research Phase를 통과한다.

### Router Identification

```text
Vendor 확인 방법 존재
Model 확인 방법 존재
Hardware Version 확인 방법 존재
Firmware 확인 방법 존재
```

### Vulnerability

```text
CVE Source 확정
Firmware Matching 방법 확정
KEV 확인 방법 확정
```

### Configuration

```text
검사 대상 확정
각 설정 확인 방법 확정
자동화 수준 평가 완료
```

### LAN

```text
Device Discovery 방식 확정
```

### WAN

```text
내부 검사 한계 파악
```

### Traffic

```text
MVP 포함 여부 결정
```

---

# 38. Researcher Decision Rule

새로운 자료를 발견할 때마다 묻는다.

> 이 자료가 Router Security Checker의 검사 정확도를 높이는가?

```text
YES
→ Evidence DB

MAYBE
→ Research Backlog

NO
→ 제외
```

---

# 39. Absolute Restrictions

본 조사 단계에서는 다음 작업을 하지 않는다.

```text
실제 취약점 Exploit
무단 외부 장비 Scan
공격 Payload 실행
Password Brute Force
Firmware 변조
Router 침투
```

조사의 목적은 공격 성공 여부가 아니라

```text
Detection
Evidence
Risk Assessment
Remediation
```

이다.

---

# 40. Immediate Next Action

현재 가장 먼저 조사해야 하는 것은 CVE가 아니다.

순서는 다음과 같다.

```text
STEP 1
Archer C50 v6라는 장비를 프로그램이 어떻게 식별할지 조사
    ↓
STEP 2
Firmware Version을 어떻게 얻을지 조사
    ↓
STEP 3
공식 최신 Firmware와 비교
    ↓
STEP 4
해당 Firmware에 적용되는 CVE 조사
    ↓
STEP 5
Router Configuration 조사
    ↓
STEP 6
LAN / WAN 검사
    ↓
STEP 7
Traffic Analysis 가능성 조사
```

즉,

```text
DEVICE IDENTITY
    ↓
FIRMWARE
    ↓
VULNERABILITY
    ↓
CONFIGURATION
    ↓
NETWORK
    ↓
TRAFFIC
```

순서로 진행한다.

---

## Status

```text
Document
Internet Research PRD

Version
v0.1

Parent Project
Router Security Checker

Research Stage
PHASE 0 — Manual Research

Primary Target
TP-Link Archer C50 v6

Primary Goal
인터넷 조사 결과를 실제 개발 요구사항으로 변환
```
