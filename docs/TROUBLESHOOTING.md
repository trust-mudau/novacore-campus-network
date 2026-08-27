# Packet Tracer Troubleshooting Guide

Work from the nearest layer outward. Fix the first failed check before changing routing or ACLs.

## A client does not receive DHCP

1. Confirm the client access port is assigned to the intended VLAN with `show vlan brief`.
2. Confirm the router-facing port is trunking the VLAN with `show interfaces trunk`.
3. Check that the matching router subinterface is up/up and uses the correct 802.1Q tag.
4. Confirm `ip helper-address 10.10.60.10` is present on the client VLAN gateway.
5. Check the SERVICES-SRV address, gateway, DHCP service state, pool mask, and available addresses.

## A client reaches its gateway but not another VLAN

1. Check `show ip route` on the local core router.
2. Check `show ip ospf neighbor`; both routed neighbors should reach FULL state.
3. Confirm the destination subnet appears as an OSPF route.
4. Review the inbound ACL and its counters with `show access-lists`.

## Internet access fails

1. Ping `203.0.113.1` from EDGE-R1.
2. Confirm EDGE-R1 has the default route through `203.0.113.1`.
3. Confirm OSPF distributes a default route to both core routers.
4. Generate traffic from a client and inspect `show ip nat translations`.
5. Check `INET-IN` counters to verify return traffic is permitted.

## Public DMZ access fails

1. Confirm DMZ-WEB is `172.16.10.10/28` with gateway `172.16.10.1`.
2. Confirm the static translation maps `172.16.10.10` to `203.0.113.10`.
3. Check TCP 80/443 entries in both `INET-IN` and `DMZ-OUT`.
4. Test the private address internally before testing the public address from PUBLIC-PC.

## OSPF adjacency is missing

1. Confirm both serial interfaces are up/up and use matching `/30` addresses.
2. Apply `clock rate 64000` only on the DCE end.
3. Confirm both interfaces are included in OSPF area 0 and are not passive.
4. Compare hello/dead timers and check for duplicate router IDs.

## A test passes when it should fail

Clear prior assumptions before changing the ACL. Confirm the test uses the expected source VLAN, renew the DHCP lease, review ACL direction, and check counters before and after one new test packet.
