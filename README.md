🕵️ Network Port Scan – Task 1

This project documents a TCP SYN scan performed on my local home network. The goal was to discover active devices and open TCP ports using Nmap, interpret the results, and identify possible security concerns. Sensitive identifiers like hostnames and MAC addresses have been redacted from screenshots to protect privacy.

🛠 Tools Used

Nmap – command-line port scanning tool

Wireshark – network packet analysis

Kali Linux Terminal – for running commands and capturing outputs

Screenshots – to visually document each step

🌐 Networking Setup

To scan the correct part of my home network, I first checked my own IP address:

ip a


This showed:

inet 192.168.1.39/24


I used the subnet /24, meaning the scan covered:

192.168.1.0–192.168.1.255

🧪 Commands Executed

Verify Nmap installation:

nmap -v


Run a TCP SYN scan on the home subnet:

nmap -sS 192.168.1.0/24


Save the results to a file:

nmap -sS 192.168.1.0/24 > nmap_result.txt


View the saved results:

cat nmap_result.txt

📸 Screenshots

All screenshots are in the screenshots/ folder:

01_nmap_install.png – Nmap installation check

02_ip_address.png – IP address and subnet

03_scan_command.png – Scan command being run

04_scan_results.png – Scan results with open ports

05_saved_output.png – Saved output file

06_wireshark.png – Wireshark capture showing SYN packets

Sensitive data like device names, hostnames, and MAC addresses have been redacted appropriately.

🔍 Scan Results Summary

The scan covered 256 possible addresses in the subnet.

10 hosts responded as “up”

Multiple devices showed open TCP ports

Some notable findings included:

IP Address	Port	Service
192.168.1.1	21	ftp
	22	ssh
	80	http
	443	https
192.168.1.33	80	http
192.168.1.41	62078	iphone-sync

The router had several management services exposed, and a mobile device was identified by the iPhone sync port.

🔐 Security Risk Analysis

One of the more interesting findings was that port 21 (FTP) on the router responded as open. FTP is an older protocol that typically sends credentials in plaintext unless secured with TLS. On a modern home network, this service is rarely necessary and could expose sensitive information if abused. For better security, FTP should be disabled or restricted to trusted internal hosts.

Other detected services, such as SSH and HTTP/HTTPS, are normal for network devices, but they should be secured with strong authentication if they are in use.

📷 Wireshark Packet Capture (Optional)

To illustrate how a TCP SYN scan works at the packet level, I captured live traffic in Wireshark during the Nmap scan.

Filtering on:

tcp.flags.syn == 1


…shows SYN packets sent from my machine and SYN/ACK packets sent back by responsive hosts. This demonstrates Nmap’s technique of probing ports without completing full TCP handshakes.

(See 06_wireshark.png for details)

🧠 What I Learned

How to determine the correct subnet before scanning

How to perform a TCP SYN scan with Nmap

How to interpret open ports and services

How TCP SYN scans work at the packet level

Why keeping personal network details private matters
