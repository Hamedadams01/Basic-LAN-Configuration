# Basic-LAN-Configuration

## Overview

This lab walks through building and verifying a basic LAN (Local Area Network) in Cisco Packet Tracer. A LAN connects multiple devices — a router, a switch, two PCs, and a server — so they can all communicate on the same network. The exercise covers physical cabling, IP addressing, and basic router/switch configuration via the command line, finishing with a connectivity test using ping.

## Objectives


Build a small network topology with one router, one switch, two PCs, and one server.
Cable all devices correctly using the right cable types.
Configure IP addressing on the router, switch, PCs, and server so they're all on the same subnet (192.168.1.0/24).
Verify that every device can communicate with every other device using ping.


## Process

1 — Initial Topology

The workspace starts with five unconnected devices placed on the canvas: a 2911 Router, a 2960-24TT Switch, two PCs (PC0, PC1), and a Server. No cables are drawn yet — this is the blank starting layout before any wiring or configuration. 

![image alt]()

2 — Planning the Cabling

Before connecting anything, I mapped out which ports connect to which, and confirmed all links use Copper Straight-Through cables:

FromToCable TypeRouter Gig0/0Switch Fa0/1Copper Straight-ThroughPC0 FastEthernet0Switch Fa0/2Copper Straight-ThroughPC1 FastEthernet0Switch Fa0/3Copper Straight-ThroughServer FastEthernet0Switch Fa0/4Copper Straight-Through

After connecting, the link lights take about 30 seconds to turn green, confirming the physical connections are active. This picture shows the resulting star topology: Router at the top, Switch in the middle, and PC0/PC1/Server fanned out below it.

![image alt]()

3 — Router Initial Boot & Basic Config (CLI)

This is the Router's CLI tab, showing the boot-up sequence (IOS version, hardware info, licensing text) followed by the first configuration commands:

Router>enable
Router#configure terminal
Router(config)#hostname R1
R1(config)#interface g0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)#no shutdown

This renames the router to R1, assigns IP 192.168.1.1/24 to its Gig0/0 interface, and brings the interface up (no shutdown). The log confirms the interface and line protocol both changed state to "up."

![image alt]()

4 — Router Interface Config (GUI view)

This is the same Gig0/0 interface, now viewed through the Config tab's GUI instead of CLI. It confirms:


Port Status: On
IPv4 Address: 192.168.1.1
Subnet Mask: 255.255.255.0


The "Equivalent IOS Commands" panel at the bottom mirrors the CLI steps from Picture 3, showing the GUI and CLI are in sync.

![image alt]()

5 — Topology with Active Links

The full topology view now shows all links lit up green, confirming every cable (Router–Switch, Switch–PC0, Switch–PC1, Switch–Server) is physically up and passing traffic at Layer 1/2.

![image alt]()

6 — Switch Configuration (CLI)

This is the Switch's CLI tab. The log shows each switch port (Fa0/2, Fa0/3, Fa0/4, Fa0/1) cycling up as devices were connected, followed by the switch's own management configuration:

Switch(config)#hostname S1
S1(config)#interface vlan 1
S1(config-if)#ip address 192.168.1.2 255.255.255.0
S1(config-if)#no shutdown

This renames the switch to S1 and gives its VLAN 1 management interface IP 192.168.1.2/24, letting me manage the switch over the network if needed.

![image alt]()

7 — PC0 IP Configuration

PC0's Desktop > IP Configuration tab shows:


IPv4 Address: 192.168.1.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1


This static address puts PC0 on the same subnet as the router and points its gateway to R1, so it can reach beyond the local network if needed.

![image alt]()

8 — PC1 IP Configuration

Same idea as Picture 7, but for PC1:


IPv4 Address: 192.168.1.11
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1

![image alt]()

9 — Server IP Configuration

Server1's IP Configuration tab shows:

IPv4 Address: 192.168.1.5
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1


At this point, every device on the network — router, switch, both PCs, and the server — has a valid static IP on the 192.168.1.0/24 subnet.

![image alt]()

10 — Connectivity Verification (Ping Tests)

From PC1's Command Prompt, I ran four ping tests to confirm the LAN works end-to-end:


ping 192.168.1.11 (PC1 pinging itself) → 0% loss
ping 192.168.1.1 (PC1 → Router) → 0% loss
ping 192.168.1.5 (PC1 → Server, first attempt) → 100% loss (all 4 requests timed out)
ping 192.168.1.5 (PC1 → Server, second attempt) → 0% loss

![image alt]()
The first attempt to the server timed out completely, likely due to ARP resolution needing to complete first. The second attempt succeeded with 0% loss, confirming full end-to-end connectivity across the LAN.

Result

All devices on the 192.168.1.0/24 subnet — router, switch, both PCs, and the server — can successfully communicate with one another, confirming the LAN was built and configured correctly.
