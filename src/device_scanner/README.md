# Device Scanner

LAN 장치 탐색과 장치 식별을 담당한다.

후보 데이터 소스:

- ARP
- ICMP
- DHCP
- mDNS
- SSDP
- MAC OUI

Unknown Device는 즉시 악성으로 판정하지 않고 `CHECK REQUIRED` 후보로 취급한다.
