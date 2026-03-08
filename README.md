Network Security Lab
Overview
This project documents the setup and configuration of a network security environment using Windows Server 2019, Windows 10, and Wireshark. The lab demonstrates core networking and security skills including firewall rule configuration, ICMP traffic blocking, and live packet capture and analysis — simulating real-world network security tasks performed by IT professionals.

Tools & Technologies Used

Oracle VirtualBox
Windows Server 2019 (Domain Controller)
Windows 10 (Client Machine)
Windows Defender Firewall with Advanced Security
Wireshark 4.6.4
Command Prompt (ipconfig, ping)
ICMP, DNS, TCP Protocols


Network Configuration
ComponentDetailsDomain ControllerWindows Server 2019Client MachineWindows 10 (CLIENT1)DC Internal IP172.16.0.1Client IP172.16.0.101Subnet Mask255.255.255.0Default Gateway172.16.0.1Domainmydomain.com

Lab Walkthrough
Part 1 — Network Configuration & Connectivity
Step 1: Verify Domain Controller Network Configuration

Booted the Windows Server 2019 Domain Controller in VirtualBox
Opened Command Prompt and ran ipconfig to verify network adapters
Confirmed two adapters:

Internet adapter — IP: 10.0.2.15 (external internet access)
X_internal_x adapter — IP: 172.16.0.1 (internal network)



Step 2: Verify Client Machine Network Configuration

Booted the Windows 10 client machine (CLIENT1) in VirtualBox
Opened Command Prompt and ran ipconfig to verify network configuration
Confirmed CLIENT1 received IP 172.16.0.101 via DHCP
Confirmed Default Gateway pointed to DC at 172.16.0.1
Confirmed DNS Suffix showed mydomain.com

Step 3: Test Network Connectivity

From CLIENT1 ran ping 172.16.0.1 to test connectivity to the Domain Controller
Confirmed successful ping:

Packets Sent: 4, Received: 4, Lost: 0 (0% loss)
Response time under 1ms


Verified both machines were communicating on the internal network


Part 2 — Firewall Rule Configuration
Step 4: Open Windows Defender Firewall with Advanced Security

On the Domain Controller opened Windows Defender Firewall with Advanced Security
Reviewed active firewall profiles:

Domain Profile — Firewall On, Inbound blocked, Outbound allowed
Private Profile — Firewall On, Inbound blocked, Outbound allowed
Public Profile — Firewall On, Inbound blocked, Outbound allowed



Step 5: Create Inbound Firewall Rule to Block ICMP

Navigated to Inbound Rules and clicked New Rule
Selected Custom rule type to target ICMP specifically
Set Protocol type to ICMPv4
Applied rule to All Programs and Any IP addresses
Set Action to Block the connection
Applied rule to all profiles: Domain, Private, and Public
Named the rule: Block ICMP Ping
Confirmed the rule appeared at the top of the Inbound Rules list

Step 6: Verify Firewall Rule is Blocking Traffic

Switched to CLIENT1 and ran ping 172.16.0.1 again
Confirmed the firewall rule was working:

All 4 packets timed out
Packets Sent: 4, Received: 0, Lost: 4 (100% loss)


Screenshot shows before and after on the same screen demonstrating the firewall rule working in real time


Part 3 — Wireshark Packet Capture & Analysis
Step 7: Install and Launch Wireshark

Downloaded Wireshark 4.6.4 on CLIENT1
Installed with default settings including Npcap for packet capture
Launched Wireshark and identified available network interfaces
Selected the Ethernet interface showing live network activity

Step 8: Capture Blocked ICMP Traffic

Applied display filter: icmp
Ran ping 172.16.0.1 from Command Prompt while Wireshark was capturing
Wireshark captured ICMP Echo (ping) request packets from 172.16.0.101 to 172.16.0.1
All packets showed "no response" confirming the firewall was blocking the traffic
Bottom panel confirmed protocol as Internet Control Message Protocol

Step 9: Capture Allowed ICMP Traffic (Before and After)

Deleted the Block ICMP Ping firewall rule on the Domain Controller
Ran ping 172.16.0.1 again from CLIENT1 while Wireshark was still capturing
Wireshark now showed both blocked and allowed ICMP traffic in the same capture:

Packets 563-608: Echo (ping) request with no response (firewall blocked)
Packets 707-714: Echo (ping) request and reply (firewall removed, traffic allowed)


Command Prompt confirmed 0% packet loss after rule removal

Step 10: Capture DNS Traffic

Changed Wireshark display filter from icmp to dns
Browsed to external websites using Microsoft Edge on CLIENT1
Wireshark captured DNS queries and responses:

SOURCE: 172.16.0.101 (CLIENT1) sending DNS queries
DESTINATION: 172.16.0.1 (DC acting as DNS server)
Captured standard DNS queries and responses for multiple domains
Bottom panel confirmed protocol as Domain Name System




Key Skills Demonstrated

Configuring and verifying network settings using ipconfig
Testing network connectivity using ping
Creating custom inbound firewall rules in Windows Defender Firewall
Blocking and allowing ICMP traffic using firewall rules
Installing and configuring Wireshark for packet capture
Applying display filters in Wireshark (icmp, dns)
Analyzing and interpreting captured network packets
Identifying network protocols including ICMP, DNS, and TCP
Understanding the full packet lifecycle from request to response


Screenshots
1. DC Network Configuration (ipconfig)
Show Image
Domain Controller ipconfig output showing two network adapters — internet adapter (10.0.2.15) and internal adapter (172.16.0.1).
2. CLIENT1 Network Configuration (ipconfig)
Show Image
CLIENT1 ipconfig output confirming IP address 172.16.0.101, subnet mask 255.255.255.0, default gateway 172.16.0.1, and DNS suffix mydomain.com.
3. Successful Ping Before Firewall Rule
Show Image
Ping from CLIENT1 to DC (172.16.0.1) showing 4 packets sent, 4 received, 0% loss — confirming full network connectivity before the firewall rule was applied.
4. Windows Defender Firewall with Advanced Security
Show Image
Windows Defender Firewall with Advanced Security open on the Domain Controller showing all three active profiles — Domain, Private, and Public.
5. Block ICMP Ping Firewall Rule Created
Show Image
Custom inbound firewall rule "Block ICMP Ping" created and active at the top of the Inbound Rules list — configured to block all ICMPv4 traffic on all profiles.
6. Ping Blocked — 100% Packet Loss
Show Image
Before and after ping results showing the firewall rule working in real time. First ping shows 0% loss (allowed), second ping shows 100% loss (blocked by firewall rule).
7. Wireshark Installed and Ready
Show Image
Wireshark 4.6.4 installed and launched on CLIENT1 showing available network interfaces including the Ethernet adapter with live traffic activity.
8. Wireshark — ICMP Traffic Blocked
Show Image
Wireshark with icmp filter applied showing ICMP Echo (ping) requests from CLIENT1 (172.16.0.101) to DC (172.16.0.1) with "no response" — confirming the firewall rule is blocking the traffic at the packet level.
9. Wireshark — ICMP Blocked vs Allowed
Show Image
Wireshark capture showing the complete before and after — blocked ICMP packets (no response) at the top and allowed ICMP packets (reply) at the bottom after the firewall rule was removed. Command Prompt confirms 0% packet loss.
10. Wireshark — DNS Traffic Captured
Show Image
Wireshark with dns filter applied showing live DNS queries and responses between CLIENT1 (172.16.0.101) and the DC acting as DNS server (172.16.0.1). Captured standard DNS queries for multiple domains confirming the DC is functioning as the domain DNS resolver.
