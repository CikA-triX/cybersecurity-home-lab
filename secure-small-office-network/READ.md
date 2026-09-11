Secure Small Office Network
Capstone Project 2 — TechCrush Cybersecurity Bootcamp, Cohort 6 Role: Network Design Lead | Group 8


Overview
Small offices are easy targets. Everything on one flat network — web servers, staff PCs, printers, guest WiFi — no separation, no control. One compromised device and an attacker moves freely across everything.

This project designs, builds, and tests a secure small office network that addresses exactly that problem. It uses a single-firewall DMZ architecture with zone-based access control, threat simulation, and verified security outcomes.

This was a group capstone project. My individual contribution covers the network design, zone architecture, firewall ACL configuration, threat simulation, packet capture analysis, and FTP hardening. Password configuration for some devices was handled by a teammate.


My Individual Contribution
Designed the full network topology and zone architecture
Configured the Cisco ASA 5506-X firewall — interfaces, security levels, NAT
Wrote all five ACL rules from scratch and applied them to the correct interfaces
Conducted port scanning (before and after hardening)
Ran the threat simulation — attacker vs. staff network
Captured and analysed packet traffic in Simulation Mode
Hardened the DMZ web server — disabled FTP, removed default credentials
Verified all ACL rules using live hit counters from the ASA CLI
Produced the full project report and presentation slides


Tools Used
Tool
Purpose
Cisco Packet Tracer
Full network simulation
Cisco ASA 5506-X (simulated)
Firewall configuration via CLI
Built-in Telnet
Port scanning (Nmap equivalent)
Packet Tracer Simulation Mode
Packet capture (Wireshark equivalent)



Network Design
Architecture: Single Firewall DMZ — Star Topology Devices: 11 total Zones: 4
Firewall Interface Mapping
Interface
Zone
Security Level
Network
Gig1/1
OUTSIDE
0
192.168.1.0/24
Gig1/2
GUESTWIFI
0
192.168.40.0/24
Gig1/3
INSIDE
100
192.168.20.0/24
Gig1/4
DMZ
50
192.168.10.0/24

Zone Summary
Internet/Edge Zone (Level 0) — Untrusted. Edge router and simulated attacker.
DMZ Zone (Level 50) — Semi-trusted. Hosts www.smalloffice.com and DNS. If compromised, the attacker is contained here and cannot reach internal systems.
Internal Zone (Level 100) — Most trusted. Staff PCs and printer on VLAN 10.
Guest WiFi Zone (Level 0) — Untrusted. Visitors get internet access only. Zero access to staff or DMZ.
Device List
Device
IP Address
Notes
DMZ_WebServer
192.168.10.1
Hosts HTTP and DNS
Staff_PC1
192.168.20.2
Internal staff
Staff_PC2
192.168.20.3
Internal staff
Office_Printer
192.168.20.4
Locked to port 9100 inbound only
GuestWiFi_Router
192.168.30.1 (LAN)
WPA2-PSK AES
Attacker_PC
203.0.113.3
Simulated external threat
Edge_Router
203.0.113.1 / 192.168.1.2
ISP-facing router



Security Controls
Firewall ACLs — 5 Rules, Deny All by Default
ACL Name
Direction
What It Does
OUTSIDE_TO_DMZ
Inbound on OUTSIDE
Permits TCP port 80, 443, UDP port 53 to web server only
DMZ_IN
Inbound on DMZ
Blocks DMZ from initiating connections to internal network
DMZ_RETURN_TO_INSIDE
Inbound on DMZ
Permits reply traffic from DMZ back to internal — added during live testing
INSIDE_OUT
Inbound on INSIDE
Permits staff PC traffic. Printer locked to port 9100 only. Printer cannot initiate outbound connections.
GUEST_IN
Inbound on GUESTWIFI
Blocks guest network from reaching internal or DMZ. Internet only.


Key discovery: Packet Tracer's ASA does not track active sessions automatically. During live testing, return traffic from the DMZ kept dropping. I identified the issue and added DMZ_RETURN_TO_INSIDE as a dedicated rule to permit reply traffic. This made stateful firewall behaviour click in a way studying alone never could.
Unidirectional Printer ACL
The Office Printer (192.168.20.4) can receive print jobs on port 9100 but cannot initiate any outbound connection. Even if the printer is compromised, it cannot move laterally or phone home. This eliminates a common attack surface that most small offices ignore.
Port Hardening
Port
Before
After
80 (HTTP)
Open
Open
443 (HTTPS)
Open
Open
53 (DNS)
Open
Open
21 (FTP)
Open — default credentials (cisco/cisco)
Closed — Connection Timed Out


FTP was documented as the before state, disabled, and verified closed using Telnet.
Additional Controls
Static IP addressing — predictable addresses enable precise ACL rules
VLAN 10 — all internal devices segmented from guest traffic
WPA2-PSK with AES — guest WiFi encryption
Encrypted passwords — all devices configured with strong credentials
NAT — static NAT for DMZ web server


Threat Simulation
Scenario: Attacker_PC (203.0.113.3) attempts to reach Staff_PC1 (192.168.20.2)

Result: Request Timeout — the attacker never reached the internal network.

Why it was blocked:

Firewall checked the packet against the OUTSIDE_TO_DMZ ACL
Destination 192.168.20.x matched no permit rule
Implicit deny — packet dropped silently


ACL Verification — Live Hit Counters
These numbers were captured live from the ASA CLI using show access-list during testing. They are not fabricated.

Counter
Value
What It Proves
INSIDE_OUT line 1
hitcnt=35
Staff traffic flowing to DMZ and internet
OUTSIDE_TO_DMZ port 80
hitcnt=12
HTTP requests reaching the web server
DMZ_RETURN_TO_INSIDE line 1
hitcnt=22
Return traffic path working correctly



Key Design Decisions
Every decision in this project was intentional.

Why static IP over DHCP? Fixed addresses allow precise firewall rules. Dynamic IPs would break ACL targeting.

Why star topology? The firewall sits at the centre — no traffic can bypass security controls.

Why single firewall DMZ? Cost-effective and appropriately secure for a small office scope. Dual-firewall DMZ is the identified next step for a more robust architecture.

Why Defense in Depth? ACLs alone are not enough. This project layers ACLs, VLAN segmentation, WPA2 encryption, and password hardening at every level.

Why unidirectional printer ACL? Printers are commonly overlooked attack vectors. By denying all outbound initiation from the printer, lateral movement risk is eliminated even if the device is compromised.


Lessons Learned
The most important thing I learned was not in any lecture.

During live testing, HTTP responses from the web server kept dropping before reaching the staff PC. After tracing the packets, I realised the ASA in Packet Tracer does not track active sessions the way a stateful firewall does in production. Return traffic was being implicitly denied.

I added DMZ_RETURN_TO_INSIDE to explicitly permit reply traffic. The moment it worked, session tracking and stateful firewall behaviour made complete sense in a way no textbook had managed to explain.

What I would do differently:

Configure and test each ACL rule immediately after writing it, rather than all at once
Explore dual-firewall DMZ architecture for a more robust design next time


Screenshots
All screenshots are named and available in the /screenshots folder:

1_asa_cli_baseline_configuration
2_asa_cli_baseline_show_acl
3_outside_scan_ports_open
4_asa_cli_hardening_command
5_outside_scan_ports_hardened
6_asa_cli_hardened_show_acl
7a_dns_resolved_staff_pc
7b_http_request_at_firewall
7c_http_request_at_dmz_server
7d_http_response_at_firewall
7e_http_response_staff_pc
8_pdu_layer3_layer4_headers
9_threat_blocked_at_firewall
10_firewall_acl_drop_log
12_asa_cli_final_hit_counters
show_vlan_brief
network_topology_diagram
firewall_interface_mapping
connectivity_test_traffic_works
connectivity_test_attacker_blocked


About This Project
Programme: TechCrush Cybersecurity Bootcamp — Cohort 6 Track: Cybersecurity Project: Capstone Project 2 — Building a Secure Small Office Network Group: Group 8 My Role: Network Design Lead Completion Date: June 2026



Built with Cisco Packet Tracer. Documented by Tolu Akinyele (CikA-triX). GitHub: github.com/CikA-triX LinkedIn: linkedin.com/in/toluakinyele


