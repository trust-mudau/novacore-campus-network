# IP Addressing Plan

## User and service networks

| VLAN | Name | Subnet | Mask | Gateway | Usable hosts | DHCP range / static use |
|---:|---|---|---|---|---:|---|
| 10 | Engineering | 10.10.10.0/27 | 255.255.255.224 | 10.10.10.1 | 30 | DHCP 10.10.10.11–10.10.10.30 |
| 20 | Product | 10.10.20.0/28 | 255.255.255.240 | 10.10.20.1 | 14 | DHCP 10.10.20.5–10.10.20.14 |
| 30 | Finance | 10.10.30.0/28 | 255.255.255.240 | 10.10.30.1 | 14 | DHCP 10.10.30.5–10.10.30.14 |
| 40 | People Operations | 10.10.40.0/28 | 255.255.255.240 | 10.10.40.1 | 14 | DHCP 10.10.40.5–10.10.40.14 |
| 50 | Guest | 10.10.50.0/27 | 255.255.255.224 | 10.10.50.1 | 30 | DHCP 10.10.50.5–10.10.50.30 |
| 60 | Services | 10.10.60.0/28 | 255.255.255.240 | 10.10.60.1 | 14 | SERVICES-SRV 10.10.60.10 |
| 99 | Management | 10.10.99.0/28 | 255.255.255.240 | 10.10.99.1 | 14 | SRV-SW1 10.10.99.2; admin PC 10.10.99.10 |
| 70 | DMZ | 172.16.10.0/28 | 255.255.255.240 | 172.16.10.1 | 14 | DMZ-WEB 172.16.10.10 |

## Routed links

| Link | Network | Side A | Side B |
|---|---|---|---|
| EDGE-R1 ↔ CORE-R1 | 10.255.0.0/30 | EDGE-R1 10.255.0.1 | CORE-R1 10.255.0.2 |
| CORE-R1 ↔ CORE-R2 | 10.255.0.4/30 | CORE-R1 10.255.0.5 | CORE-R2 10.255.0.6 |
| EDGE-R1 ↔ CORE-R2 | 10.255.0.8/30 | EDGE-R1 10.255.0.9 | CORE-R2 10.255.0.10 |
| ISP-RTR ↔ EDGE-R1 | 203.0.113.0/28 | ISP-RTR 203.0.113.1 | EDGE-R1 203.0.113.2 |

`203.0.113.0/24` and `198.51.100.0/24` are documentation-only ranges, making them safe for a closed lab.

## Public and server addresses

| Purpose | Address | Notes |
|---|---|---|
| Public DMZ web address | 203.0.113.10 | Static NAT to 172.16.10.10 |
| PUBLIC-PC | 198.51.100.10 | External test PC; gateway 198.51.100.1 |
| SERVICES-SRV | 10.10.60.10 | DHCP, DNS, and optional intranet HTTP |
| DMZ-WEB | 172.16.10.10 | Public HTTP/HTTPS target |

## DNS records

| Name | Address |
|---|---|
| intranet.novacore.local | 10.10.60.10 |
| staging.novacore.local | 172.16.10.10 |
| www.novacore.example | 203.0.113.10 |
