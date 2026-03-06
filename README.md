# Enterprise Network Lab — Cisco 1841 Routers & 2960 Switches

# Table of Contents
- Overview
- Network Topology
- Ip Addressing Scheme
- Hardware Used
- Technologies & Protocols
- Device Configurations
- Security hardening
- Testing & Verification
- Troubleshooting Notes
- Key Takealways
# Overview 
The goal of this project was to design and implement a small enterprise-style network using physical Cisco hardware. The lab includes two routers connected through a WAN link running OSPF for dynamic routing, along with two access switches configured with VLAN segmentation. Inter-VLAN communication is achieved using Router-on-a-Stick, while DHCP provides automatic IP address assignment to end devices. Additional security hardening measures were implemented across all devices to improve network security and device  management.

# Network Topology
<img width="1052" height="965" alt="image" src="https://github.com/user-attachments/assets/fbd52310-1b43-4a89-9a90-e3c9c0a413b8" />

# VLAN Design
VLAN    Name        Purpose
10    VLAN10_Users  End Useres (PC1,PC2)
20    VLAN20_Users  Reserved for Expansion
30    VLAN30_Remote Remote Segment (PC3)
99    Management    Network device management only

# IP Addressing Scheme
Device    Interface   IP Address    Subnet     Purpose
R1        Fa0/0.10    192.168.10.1    /24    VLAN 10 Gateway
R1        Fa0/0.20    192.168.20.1    /24    VLAN 20 Gateway
R1        Fa0/0.99    192.168.99.1    /24    Management Gateway
R1        Fa0/1       10.0.0.1        /30    WAN Link
R2        Fa0/0       192.168.30.1    /24    VLAN 30 Gateway
R2        Fa0/1       10.0.0.2        /30    WAN Link
SW1       VLAN 99     192.168.99.2    /24    Management
Sw2       VLAN 99     192.168.99.3    /24    Management
PC1       NIC         192.168.10.12   /24    DHCP Assigned
PC2       NIC         192.168.10.10   /24    DHCP Assigned

# Hardware Used
Device      Model    Role
Router1  Cisco 1841  Core Router (Router-on-a-Stick + OSPF + DHCP)
Router2  Cisco 1841  Remote Router (OSPF)
Switch1  Cisco 2960  Access/Distribution Switch (VLANs 10, 20, 99)
Switch2  Cisco 2960  Access Switch (VLAN 30, 99)
PC1      MacBook     End user device - VLAN 10
PC2      Windows PC  End user device - VLAN 10

# Technology & Protocols
Routing:
- OSPF (Open Shortest Path First): dynamic routing between R1 and R2 in Area 0
- Router-on-a-Stick: inter-VLAN routing using subinterfaces on R1 Fa0/0
- DHCP: automatic IP assignment for all VLANs with excluded address ranges
Switching:
- VLANs: network segmentation across VLAN 10, 20, 30, 99
- 802.1Q Trunking: carrying multiple VLANs over single uplink
- Spanning Tree PortFast: faster port activation on access ports
- BPDU Guard: STP attack prevention on access ports
- Port Security: sticky MAC address learning, max 1 MAC per port
Security:
- SSH v2: secure remote management (Telnet disabled)
- Native VLAN 99:  protection against VLAN hopping attacks
- Service password-encryption: encrypting plain text passwords
- Enable secret: MD5 encrypted privileged access
- Management VLAN isolation: management traffic separated from user traffic
- Unused port shutdown: all unused ports administratively down in VLAN 99
- MOTD Banner: unauthorized access warning on all devices
- exec-timeout: automatic session timeout after 5 minutes
- RSA 1024-bit key: SSH encryption
