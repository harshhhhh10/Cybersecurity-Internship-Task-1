🕵️ Network Port Scan – Task 1


This project documents a TCP SYN scan performed on a local home network. The goal was to discover active devices and open TCP ports using Nmap, interpret the results, and identify potential security risks.





Key Objectives:

Determine the live hosts on the local subnet.
Identify open ports and running services.
Analyze the security implications of the findings.
Verify the scan traffic using Wireshark.
Note: Sensitive identifiers like specific hostnames and MAC addresses have been redacted from screenshots to protect privacy.





🛠 Tools Used
Nmap (Network Mapper): Used for network discovery and security auditing.
Wireshark: Used for network packet analysis to verify scan traffic.
Kali Linux: The operating system environment used for execution.
Terminal: Command-line interface for running tools.




🌐 Networking Setup
Before scanning, it was necessary to identify the current network range to ensure the target was correct.


Command:

bash

ip a
Finding:
The output showed the machine's IP address:

inet 192.168.1.39/24

This indicated the machine is on a /24 subnet (Class C), meaning the scan target range was 192.168.1.0 to 192.168.1.255.



🧪 Commands Executed
The scan utilized the TCP SYN Scan (-sS). This technique is often referred to as "stealth" scanning because it does not complete the full TCP three-way handshake, making it faster and less likely to be logged by basic application logs.

Note: The -sS scan requires root privileges to craft raw packets.

1. Verify Nmap Installation:

bash

nmap -v
2. Execute TCP SYN Scan:

bash

sudo nmap -sS 192.168.1.0/24
3. Save Results to File:

bash

sudo nmap -sS 192.168.1.0/24 -oN nmap_result.txt
(I used the -oN flag for normal output formatting, though redirection > also works).

4. View Saved Results:

bash

cat nmap_result.txt




📸 Screenshots
Visual documentation of the process is located in the /screenshots folder:

01_nmap_install.png – Verification of Nmap installation.
02_ip_address.png – Identification of local IP and subnet.
03_scan_command.png – Execution of the SYN scan command.
04_scan_results.png – Raw output showing discovered hosts and ports.
05_saved_output.png – Verification of the saved text file.
06_wireshark.png – Packet capture of the scan traffic.




🔍 Scan Results Summary
The scan targeted 256 possible IP addresses.

Total Hosts Found: 10 hosts were "up".
Findings: Multiple devices exposed management services.
Discovered Ports Table
IP Address
Port
State
Service
Notes
192.168.1.1	21	open	ftp	File Transfer Protocol
192.168.1.1	22	open	ssh	Secure Shell
192.168.1.1	80	open	http	Web Server (Router Admin)
192.168.1.1	443	open	https	Secure Web Server
192.168.1.33	80	open	http	Web Service
192.168.1.41	62078	open	iphone-sync	Apple iPhone Sync Service




🔐 Security Risk Analysis
The analysis focused on the services exposed by the router (192.168.1.1) and mobile devices.

1. Insecure FTP Service (Port 21):
The most significant finding was an open FTP port on the router. FTP (File Transfer Protocol) transmits data and credentials in plaintext by default.

Risk: An attacker on the same network could sniff traffic to capture usernames and passwords.
Recommendation: Disable FTP on the router interface if not in use. Switch to SFTP (Secure FTP) if file transfers are required.
2. HTTP Management Interface (Port 80):
The router's admin panel is accessible via HTTP.

Risk: If credentials are entered over HTTP, they are sent unencrypted.
Recommendation: Force HTTPS (Port 443) for all administrative access.
3. iPhone Sync Service (Port 62078):
This port is standard for Apple devices for syncing with iTunes/Find My, but it does reveal the device type. While generally safe, closing unused ports is always best practice to reduce the attack surface.




📷 Wireshark Packet Analysis
To verify the behavior of the -sS scan, live traffic was captured in Wireshark.

Filter Applied:

wireshark

tcp.flags.syn == 1
Observations:
The capture clearly demonstrated the SYN scan mechanics:

SYN Packets: My machine sent SYN packets to target IPs.
SYN/ACK Responses: Open ports replied with a SYN/ACK (inviting a connection).
RST Responses: Closed ports replied with an RST (Reset), refusing the connection.
Nmap Behavior: Nmap immediately sent an RST to tear down the connection after receiving the SYN/ACK, confirming it did not complete the handshake.



🧠 Key Learnings
Subnetting: How to calculate the scan range using CIDR notation (/24).
Scan Types: The difference between a full TCP connect scan and a SYN scan.
Packet Mechanics: Visualizing the TCP handshake (SYN -> SYN/ACK -> RST).
Service Identification: Mapping open ports to potential vulnerabilities (e.g., FTP vs. SFTP).
Operational Security: The importance of redacting sensitive data in public reports.
