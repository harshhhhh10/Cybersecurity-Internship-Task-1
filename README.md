# 🔍 Task 1 – Network Port Scanning with Nmap

![Internship](https://img.shields.io/badge/Elevate%20Labs-Cybersecurity%20Internship-blue?style=for-the-badge)
![Tool](https://img.shields.io/badge/Tool-Nmap-4EAA25?style=for-the-badge&logo=linux&logoColor=white)
![OS](https://img.shields.io/badge/OS-Kali%20Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

---

## 📌 Objective

Perform a TCP SYN scan on a local home network to discover active devices and open ports using Nmap, analyze the results for potential security risks, and verify scan traffic using Wireshark.

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| **Nmap** | Network discovery and port scanning |
| **Wireshark** | Packet capture to verify SYN scan traffic |
| **Kali Linux** | OS environment for execution |

---

## 🌐 Network Setup

Before scanning, the local subnet was identified using:

```bash
ip a
```

**Finding:** Machine IP was `192.168.1.39/24` — meaning the scan range was `192.168.1.0/24` (256 addresses).

---

## 🧪 Commands Used

```bash
# 1. Verify Nmap installation
nmap -v

# 2. Run TCP SYN scan on local subnet
sudo nmap -sS 192.168.1.0/24

# 3. Save results to file
sudo nmap -sS 192.168.1.0/24 -oN nmap_result.txt

# 4. View saved results
cat nmap_result.txt
```

> **Note:** `-sS` (SYN scan) requires root privileges to craft raw packets. It does not complete the full TCP three-way handshake, making it faster and less likely to be logged.

---

## 📊 Scan Results Summary

- **Total hosts scanned:** 256
- **Active hosts found:** 10

### Discovered Open Ports

| IP Address | Port | State | Service | Notes |
|------------|------|-------|---------|-------|
| 192.168.1.1 | 21 | open | FTP | ⚠️ Plaintext — credentials exposed |
| 192.168.1.1 | 22 | open | SSH | ✅ Secure Shell |
| 192.168.1.1 | 80 | open | HTTP | ⚠️ Unencrypted admin panel |
| 192.168.1.1 | 443 | open | HTTPS | ✅ Secure web |
| 192.168.1.33 | 80 | open | HTTP | Web service |
| 192.168.1.41 | 62078 | open | iphone-sync | Apple device sync |

---

## 🔐 Security Risk Analysis

### ⚠️ FTP Open (Port 21)
FTP transmits credentials in **plaintext** — anyone on the network can sniff them.
- **Fix:** Disable FTP on the router; use SFTP if file transfers are needed.

### ⚠️ HTTP Admin Panel (Port 80)
Router admin interface accessible over unencrypted HTTP.
- **Fix:** Force HTTPS (port 443) for all admin access.

### ℹ️ iPhone Sync (Port 62078)
Standard Apple sync port — low risk, but reveals device type.
- **Fix:** Close if unused to reduce attack surface.

---

## 📦 Wireshark Packet Verification

Scan traffic was captured in Wireshark using the filter:

```
tcp.flags.syn == 1
```

**Observations:**
- **Open ports** replied with `SYN/ACK`
- **Closed ports** replied with `RST`
- **Nmap behavior:** Immediately sent `RST` after receiving `SYN/ACK` — confirming the half-open scan never completed the handshake

---

## 📸 Screenshots

All screenshots are in the [`/screenshots`](./screenshots) folder:

| File | Description |
|------|-------------|
| `01_nmap_install.png` | Nmap installation verification |
| `02_ip_address.png` | Local IP and subnet identification |
| `03_scan_command.png` | SYN scan command execution |
| `04_scan_results.png` | Raw scan output |
| `05_saved_output.png` | Saved results file |
| `06_wireshark.png` | Wireshark packet capture |

---

## 🧠 Key Learnings

- How to calculate scan range using CIDR notation (`/24`)
- Difference between a full TCP connect scan (`-sT`) and a SYN scan (`-sS`)
- Visualizing the TCP handshake: `SYN → SYN/ACK → RST`
- Mapping open ports to real-world vulnerabilities (e.g., FTP vs SFTP)
- Importance of redacting sensitive info (MACs, hostnames) in public reports

---

## 📁 Repository Structure

```
Cybersecurity-Internship-Task-1/
├── README.md
├── nmap_result.txt
└── screenshots/
    ├── 01_nmap_install.png
    ├── 02_ip_address.png
    ├── 03_scan_command.png
    ├── 04_scan_results.png
    ├── 05_saved_output.png
    └── 06_wireshark.png
```

---

## 🏷️ Suggested GitHub Topics

```
nmap  port-scanning  network-security  kali-linux  wireshark
cybersecurity  ethical-hacking  tcp-syn-scan  network-recon  internship
```

---

> 🔒 *This scan was performed on a personal home network for educational purposes only as part of the Elevate Labs Cybersecurity Internship.*
