VirtualBox Network Troubleshooting Lab
A small virtual networking lab built using Oracle VirtualBox, Windows virtual machines, and Ubuntu Linux.
This lab was created as part of my studies in Certificate IV in Cyber Security and my ongoing CCNA studies. The goal was to practise basic network configuration, connectivity testing, and troubleshooting in an isolated virtual environment.
________________________________________
Lab Overview
In this lab, I created a small virtual network containing:
●	Computer01 — Windows client
●	Computer02 — Windows client
●	Ubuntu — Ubuntu Linux VM acting as the default gateway
●	CiscoLab — VirtualBox Internal Network connecting the three VMs
The lab uses static IPv4 addressing and tests connectivity between the virtual machines using ping.
The final troubleshooting scenario involves identifying a Windows Defender Firewall rule that was preventing ICMP Echo Requests from receiving a response.
________________________________________
Network Topology
                   Virtual Network
                       CiscoLab
                           │
             ┌─────────────┼─────────────┐
             │             │             │
        Computer01     Computer02     Ubuntu
        Windows         Windows        Linux
        Client          Client         Gateway
             │             │             │
             └─────────────┴─────────────┘

        192.168.10.10   192.168.10.20   192.168.10.1

All three virtual machines are connected to the same 192.168.10.0/24 network.
________________________________________
IP Addressing
Device	Operating System	Role	IP Address	Subnet Mask	Default Gateway
Computer01	Windows	Client	192.168.10.10	255.255.255.0	192.168.10.1
Computer02	Windows	Client	192.168.10.20	255.255.255.0	192.168.10.1
Ubuntu	Ubuntu Linux	Gateway	192.168.10.1	255.255.255.0	—
The lab configuration assigns Ubuntu 192.168.10.1, Computer01 192.168.10.10, and Computer02 192.168.10.20, all using a /24 subnet.
________________________________________
VirtualBox Configuration
Each virtual machine was configured using:
●	Network Adapter: Adapter 1
●	Attached To: Internal Network
●	Internal Network Name: CiscoLab
●	Promiscuous Mode: Deny
●	Virtual Cable: Connected
Using an Internal Network creates an isolated virtual network allowing the VMs to communicate with each other without requiring an external physical network connection.
________________________________________
Configuration
Ubuntu
Ubuntu was configured with:
IP Address: 192.168.10.1
Subnet Mask: 255.255.255.0
Gateway: Not configured

The configuration was verified using:
ip addr

________________________________________
Computer01
Computer01 was configured with:
IP Address: 192.168.10.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.1

The configuration was verified using:
ipconfig

________________________________________
Computer02
Computer02 was configured with:
IP Address: 192.168.10.20
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.1

The configuration was verified using:
ipconfig

________________________________________
Connectivity Testing
After configuring the machines, I used ping to test connectivity.
Computer01 → Ubuntu
ping 192.168.10.1

Result: PASS
Computer02 → Ubuntu
ping 192.168.10.1

Result: PASS
Computer01 → Computer02
ping 192.168.10.20

Result: Initial failure
Computer02 → Computer01
ping 192.168.10.10

Result: Initial failure
The lab specifically uses these connectivity tests to verify communication between the clients and the Ubuntu gateway, as well as communication between the two Windows clients.
________________________________________
Troubleshooting
Problem
Although the IP addresses, subnet masks, and default gateways were configured correctly, the Windows machines were initially unable to successfully ping each other.
The result included:
Request timed out.

Instead of immediately changing the network configuration, I worked through the problem systematically.
Initial checks
Check	Result
IP address configuration	✅ Correct
Subnet mask	✅ Correct
Default gateway	✅ Correct
VirtualBox Internal Network	✅ Correct
Computer01 → Ubuntu	✅ Working
Computer02 → Ubuntu	✅ Working
Computer01 → Computer02	❌ Failed
Computer02 → Computer01	❌ Failed
Because the clients could communicate with the Ubuntu gateway, the basic virtual network configuration appeared to be functioning.
________________________________________
Windows Defender Firewall
I investigated Windows Defender Firewall with Advanced Security.
Under:
Inbound Rules

I located:
File and Printer Sharing
(Echo Request - ICMPv4-In)

The rule was disabled.
The lab identifies Windows Firewall blocking incoming ICMP Echo Requests as the cause of the failed ping tests.
________________________________________
Solution
Rather than completely disabling Windows Defender Firewall, I enabled the specific inbound rule:
File and Printer Sharing
(Echo Request - ICMPv4-In)

After enabling the rule, I repeated the ping test:
ping 192.168.10.20

The test then returned successful replies.
Reply from 192.168.10.20

The same process was used to verify connectivity in the opposite direction.
________________________________________
Verification
After troubleshooting, the final connectivity tests were successful:
Source	Destination	Result
Computer01	Ubuntu 192.168.10.1	✅ PASS
Computer02	Ubuntu 192.168.10.1	✅ PASS
Computer01	Computer02 192.168.10.20	✅ PASS
Computer02	Computer01 192.168.10.10	✅ PASS
________________________________________
What I Learned
This lab helped me practise:
●	Configuring VirtualBox Internal Networks
●	Understanding basic virtual networking
●	Configuring static IPv4 addresses
●	Understanding /24 subnetting
●	Configuring default gateways
●	Using ipconfig to troubleshoot Windows networking
●	Using ip addr to verify Linux network configuration
●	Using ping to test network connectivity
●	Understanding ICMP Echo Requests and Echo Replies
●	Troubleshooting Windows Defender Firewall
●	Understanding inbound firewall rules
●	Applying the principle of least privilege
●	Documenting a troubleshooting process
The lab's stated learning outcomes include configuring Internal Networks, static IPv4 addressing, using ipconfig and ping, identifying Windows Firewall issues, and documenting troubleshooting processes.
________________________________________
Security Consideration
A key lesson from this lab was that the easiest solution isn't always the best solution.
It would have been possible to completely disable Windows Defender Firewall to allow the ping traffic.
Instead, I enabled only the specific ICMPv4 Echo Request rule required for the test.
This follows the principle of least privilege by allowing the required traffic without unnecessarily disabling the host firewall.
________________________________________
Tools Used
●	Oracle VirtualBox
●	Windows
●	Ubuntu Linux
●	Windows Command Prompt
●	Linux Terminal
●	ipconfig
●	ip addr
●	ping
●	Windows Defender Firewall with Advanced Security
________________________________________
Learning Context
This project was completed as part of my ongoing development in:
Certificate IV in Cyber Security
and
CCNA studies
I'm using home labs like this to reinforce the networking concepts I'm learning and to gain practical experience with configuring and troubleshooting networks.
________________________________________
Future Improvements
This lab is a basic starting point. Future labs will expand the environment and introduce more advanced networking concepts, including:
●	Multiple subnets
●	Routing between networks
●	VLANs
●	DHCP
●	DNS
●	Windows Server
●	Linux networking
●	Packet analysis with Wireshark
●	Network security controls
●	Firewall configuration
●	Network troubleshooting scenarios
The goal is to progressively build more complex networking and cybersecurity environments rather than relying solely on theoretical study.
________________________________________
Disclaimer
This is a personal educational lab created for learning and experimentation in an isolated virtual environment.
The configurations and techniques demonstrated here should be tested in controlled environments and should not be applied to production systems without understanding their security implications.



Software & ISO Downloads
Oracle VirtualBox
https://www.virtualbox.org/wiki/Downloads
Ubuntu Desktop
https://ubuntu.com/download/desktop
Windows 11 ISO
https://www.microsoft.com/en-ca/software-download/windows11
Windows 10
https://www.microsoft.com/en-ca/software-download/windows10
Note: Windows 10 reached end of free support on October 14, 2025. For new labs, Windows 11 may be preferable where your hardware and VirtualBox setup support it.
