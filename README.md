# Basic-LAN-Configuration

# Overview
This lab walks through building and verifying a basic LAN (Local Area Network) in Cisco Packet Tracer. A LAN connects multiple devices — a router, a switch, two PCs, and a server — so they can all communicate on the same network. The exercise covers physical cabling, IP addressing, and basic router/switch configuration via the command line, finishing with a connectivity test using ping.
# Objectives

- Build a small network topology with one router, one switch, two PCs, and one server
- Cable all devices correctly using the right cable types
- Configure IP addressing on the router, switch, PCs, and server so they're all on the same subnet (192.168.1.0/24)
- Verify that every device can communicate with every other device using ping

# Step-by-Step

- I placed five unconnected devices on the canvas: a 2911 Router, a 2960-24TT Switch, two PCs (PC0, PC1), and a Server, giving me a blank starting layout before any wiring or configuration.
- I cabled all four devices to the switch using Copper Straight-Through cables: Router Gig0/0 → Switch Fa0/1, PC0 → Fa0/2, PC1 → Fa0/3, and Server → Fa0/4.
- I waited about 30 seconds for the link lights to turn green, confirming all physical connections were active in a star topology.
- I opened the Router's CLI tab and ran enable, configure terminal, then set the hostname to R1.
- I entered interface config mode for g0/0, assigned IP address 192.168.1.1 255.255.255.0, and issued no shutdown to bring the interface up.
- I confirmed the log showed the interface and line protocol both changed state to "up."
- I switched to the router's Config tab (GUI view) and verified Port Status was On, IPv4 Address was 192.168.1.1, and Subnet Mask was 255.255.255.0 — confirming the GUI matched the CLI configuration.
- I reviewed the full topology view and confirmed every link (Router–Switch, Switch–PC0, Switch–PC1, Switch–Server) was lit green and passing traffic at Layer 1/2.
- I opened the Switch's CLI tab and watched each port (Fa0/1–Fa0/4) cycle up as devices connected.
- I set the switch hostname to S1, entered interface vlan 1, assigned IP 192.168.1.2 255.255.255.0, and issued no shutdown to give the switch a manageable IP on the network.
- I opened PC0's Desktop > IP Configuration tab and set a static IP of 192.168.1.10, subnet mask 255.255.255.0, and default gateway 192.168.1.1.
- I did the same for PC1, assigning it 192.168.1.11 with the same subnet mask and gateway.
- I opened Server1's IP Configuration tab and assigned it 192.168.1.5, with the same subnet mask and gateway, completing IP addressing across the whole network.
- I opened PC1's Command Prompt and ran ping 192.168.1.11 (pinging itself) — result: 0% loss.
- I ran ping 192.168.1.1 (PC1 → Router) — result: 0% loss.
- I ran ping 192.168.1.5 (PC1 → Server) on the first attempt — result: 100% loss, all four requests timed out.
- I re-ran ping 192.168.1.5 a second time — result: 0% loss, confirming the server was reachable once ARP/switch tables had time to populate.

# Conclusion

- A full star-topology LAN was built and cabled correctly using the appropriate cable types
- Router and switch were configured via CLI with matching GUI verification
- All devices were assigned static IPs on the 192.168.1.0/24 subnet
- Connectivity was verified end-to-end using ping, including troubleshooting an initial failed ping to the server that resolved on retry
