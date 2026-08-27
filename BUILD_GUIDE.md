# Packet Tracer Build Guide

## 1. Place and name the devices

Add four 2911 routers and name them `ISP-RTR`, `EDGE-R1`, `CORE-R1`, and `CORE-R2`. Add four 2960 switches named `F1-SW1`, `F2-SW1`, `SRV-SW1`, and `DMZ-SW1`. Add two servers named `SERVICES-SRV` and `DMZ-WEB`, client PCs for each department, and one external test PC named `PUBLIC-PC`.

For each of the three internal routers, open the Physical tab, power the router off, insert HWIC-2T serial modules, and power it on again. Interface numbers can differ by Packet Tracer version; adjust the supplied configuration only if your module slots create different serial names.

## 2. Cable the topology

Use automatic cable selection for Ethernet links. Use serial DCE/DTE cables for the routed triangle.

| From | Port | To | Port | Purpose |
|---|---|---|---|---|
| ISP-RTR | G0/0 | EDGE-R1 | G0/0 | Public network |
| ISP-RTR | G0/1 | PUBLIC-PC | F0 | Simulated internet LAN |
| EDGE-R1 | S0/0/0 | CORE-R1 | S0/0/0 | OSPF link 1 |
| CORE-R1 | S0/0/1 | CORE-R2 | S0/0/0 | OSPF link 2 |
| EDGE-R1 | S0/0/1 | CORE-R2 | S0/0/1 | OSPF link 3 |
| CORE-R1 | G0/0 | F1-SW1 | F0/1 | VLAN 10/20 trunk |
| CORE-R1 | G0/1 | SRV-SW1 | F0/1 | VLAN 60/99 trunk |
| CORE-R2 | G0/0 | F2-SW1 | F0/1 | VLAN 30/40/50 trunk |
| EDGE-R1 | G0/1 | DMZ-SW1 | F0/1 | DMZ access link |
| SERVICES-SRV | F0 | SRV-SW1 | F0/2 | Services VLAN |
| DMZ-WEB | F0 | DMZ-SW1 | F0/2 | DMZ VLAN |

Connect client PCs to the access-port ranges defined in the switch configuration files.

The `clock rate 64000` commands assume the DCE ends are EDGE-R1 S0/0/0, EDGE-R1 S0/0/1, and CORE-R1 S0/0/1. If the DCE icon appears on the opposite end, move the relevant `clock rate` command there.

## 3. Load IOS configurations

Open each device's CLI, enter privileged EXEC and global configuration mode, then paste the matching file from `configs/`.

Recommended sequence:

1. `F1-SW1.cfg`, `F2-SW1.cfg`, `SRV-SW1.cfg`, `DMZ-SW1.cfg`
2. `CORE-R1.cfg`, `CORE-R2.cfg`
3. `EDGE-R1.cfg`
4. `ISP-RTR.cfg`

If Packet Tracer pauses during RSA key generation, accept a 1024-bit key. Save each configuration with `copy running-config startup-config`.

## 4. Configure the servers

Follow `configs/SERVER_SETUP.md`. Server services in Packet Tracer are configured in the Config/Desktop/Services tabs rather than by IOS commands.

## 5. Configure clients

Set all departmental PCs to DHCP. Use static settings only for the management PC:

- IP: `10.10.99.10`
- Mask: `255.255.255.240`
- Gateway: `10.10.99.1`
- DNS: `10.10.60.10`

Configure `PUBLIC-PC` statically:

- IP: `198.51.100.10`
- Mask: `255.255.255.0`
- Gateway: `198.51.100.1`

## 6. Wait for convergence

Allow OSPF and spanning tree to settle. Confirm all required interfaces show green before testing. Use `show ip ospf neighbor` on all three internal routers; each should form two FULL adjacencies.

## 7. Verify and save

Run every test in `TEST_PLAN.md`. Fix any result that differs from the expected outcome, switch to Simulation mode for representative PDU tests, and save the final lab as `NovaCore-Campus-Network.pkt`.

## Suggested workspace labels

Add these notes to the Packet Tracer logical workspace:

- “OSPF Area 0 — resilient routed triangle” inside the three-router core
- “PAT + static NAT + internet ACL” beside EDGE-R1
- VLAN ID and subnet above each department
- “HTTP/HTTPS public; SSH from Engineering only” beside DMZ-WEB
- “DHCP / DNS / intranet” beside SERVICES-SRV
