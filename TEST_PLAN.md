# Verification and Test Plan

Run tests only after all interfaces are up, OSPF has converged, and clients have renewed their DHCP leases.

## Expected connectivity

| ID | Source | Destination / action | Expected | Feature verified |
|---|---|---|---|---|
| T01 | Engineering PC | Its default gateway | Pass | VLAN 10 and trunking |
| T02 | Product PC | `10.10.60.10` | Pass | DHCP relay, OSPF, services reachability |
| T03 | Finance PC | People Operations PC | Fail | `FINANCE-IN` ACL |
| T04 | People Operations PC | Finance PC | Fail | `PEOPLE-IN` ACL |
| T05 | Finance PC | `10.10.60.10` | Pass | ACL allows shared services |
| T06 | Guest PC | `10.10.60.10` DNS query | Pass | Limited DNS exception |
| T07 | Guest PC | Any internal gateway except its own | Fail | Guest RFC 1918 isolation |
| T08 | Guest PC | `172.16.10.10` | Fail | Guest-to-DMZ isolation |
| T09 | Guest PC | `198.51.100.10` | Pass | OSPF default route and PAT |
| T10 | Engineering PC | `http://staging.novacore.local` | Pass | DNS and DMZ HTTP policy |
| T11 | Product PC | SSH to `172.16.10.10` | Fail | DMZ SSH restriction |
| T12 | Engineering PC | SSH to `172.16.10.10` | Pass if SSH is enabled on server | DMZ admin exception |
| T13 | PUBLIC-PC | `http://203.0.113.10` | Pass | Static NAT and internet ACL |
| T14 | PUBLIC-PC | Private campus address | Fail | No direct private routing / edge filtering |
| T15 | Admin PC | SSH to each router | Pass | SSH-only management |

## Router verification commands

Run on all internal routers:

```text
show ip interface brief
show ip route
show ip ospf neighbor
show access-lists
```

Run on EDGE-R1:

```text
show ip nat translations
show ip nat statistics
show access-lists INET-IN
show access-lists DMZ-OUT
```

Run on switches:

```text
show vlan brief
show interfaces trunk
show mac address-table dynamic
```

## Resilience test

1. Start a continuous ping from an Engineering PC to a Finance PC.
2. Shut down `CORE-R1 Serial0/0/0`.
3. Allow OSPF to reconverge.
4. Confirm traffic resumes over CORE-R1 → CORE-R2 → EDGE-R1.
5. Re-enable the interface and confirm the preferred route returns.

## Suggested screenshots for the final report

- Full logical topology with green links
- `show ip ospf neighbor` on EDGE-R1
- DHCP lease on one client from each floor
- Successful Engineering-to-DMZ HTTP test
- Failed Finance-to-People Operations PDU
- Failed Guest-to-private-network PDU
- `show ip nat translations` after an internet test
- Browser access to `http://203.0.113.10` from the ISP side
