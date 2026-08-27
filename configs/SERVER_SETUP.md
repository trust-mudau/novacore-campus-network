# Packet Tracer Server Setup

## SERVICES-SRV

### Desktop > IP Configuration

- IP address: `10.10.60.10`
- Subnet mask: `255.255.255.240`
- Default gateway: `10.10.60.1`
- DNS server: `10.10.60.10`

### Services > DHCP

Turn DHCP on and create these pools. Use the listed start address, mask, gateway, DNS address, and maximum user count.

| Pool | Start IP | Mask | Gateway | DNS | Maximum users |
|---|---|---|---|---|---:|
| ENGINEERING | 10.10.10.11 | 255.255.255.224 | 10.10.10.1 | 10.10.60.10 | 20 |
| PRODUCT | 10.10.20.5 | 255.255.255.240 | 10.10.20.1 | 10.10.60.10 | 10 |
| FINANCE | 10.10.30.5 | 255.255.255.240 | 10.10.30.1 | 10.10.60.10 | 10 |
| PEOPLE_OPS | 10.10.40.5 | 255.255.255.240 | 10.10.40.1 | 10.10.60.10 | 10 |
| GUEST | 10.10.50.5 | 255.255.255.224 | 10.10.50.1 | 10.10.60.10 | 26 |

Packet Tracer will stop each pool at the subnet broadcast boundary even when the maximum count is larger than the remaining usable addresses. The suggested counts intentionally fit each subnet.

### Services > DNS

Turn DNS on and add:

- `intranet.novacore.local` → `10.10.60.10`
- `staging.novacore.local` → `172.16.10.10`
- `www.novacore.example` → `203.0.113.10`

### Services > HTTP

Turn HTTP on. Replace the default page with a simple heading such as “NovaCore Internal Services” so the intranet test is visually obvious.

## DMZ-WEB

### Desktop > IP Configuration

- IP address: `172.16.10.10`
- Subnet mask: `255.255.255.240`
- Default gateway: `172.16.10.1`
- DNS server: `10.10.60.10`

### Services

- Turn HTTP on.
- Turn HTTPS on if the Packet Tracer server version supports it.
- Replace the default page with “NovaCore Public Staging Service”.
- Optional: turn SSH on and create a lab-only account to execute the SSH policy tests.

## Client setup

Open each departmental PC and select Desktop > IP Configuration > DHCP. Confirm the assigned gateway and DNS server match the pool table before moving on to connectivity tests.
