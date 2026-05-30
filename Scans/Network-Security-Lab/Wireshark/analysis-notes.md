# Wireshark Network Traffic Analysis

## Capture Details
- Date: May 2026
- Interface: eth0 (Host-Only Network)
- Target IP: 192.168.30.128
- Attacker IP: 192.168.30.100 (Kali)
- File: capture.pcapng

---

## Analysis 1 — FTP Traffic (Port 21)

### Filter Used
ftp

### What I Found
- Protocol: FTP (File Transfer Protocol)
- Username transmitted in PLAINTEXT: msfadmin
- Password transmitted in PLAINTEXT: msfadmin
- Anyone on the network can see credentials

### Packets Observed
| Packet | Direction | Content |
|--------|-----------|---------|
| FTP Request | Client→Server | USER msfadmin |
| FTP Response | Server→Client | 331 Password required |
| FTP Request | Client→Server | PASS msfadmin |
| FTP Response | Server→Client | 230 Login successful |

### Security Risk
CRITICAL — Credentials sent in plain text.
Attacker on same network can steal username
and password just by watching traffic.

---

## Analysis 2 — Telnet Traffic (Port 23)

### Filter Used
telnet

### What I Found
- Protocol: Telnet
- Everything typed is visible in plaintext
- Username, password, and all commands exposed
- Used TCP Stream to reconstruct full session

### How I Saw the Password
Right click packet → Follow → TCP Stream
Full conversation visible including credentials

### Security Risk
CRITICAL — Telnet has zero encryption.
SSH should always be used instead of Telnet.

---

## Analysis 3 — HTTP Traffic (Port 80)

### Filter Used
http

### What I Found
- Apache 2.2.8 web server running
- DVWA (Damn Vulnerable Web App) accessible
- No HTTPS — all web traffic unencrypted
- GET and POST requests visible in plaintext

### Security Risk
HIGH — Login forms submit passwords
over plain HTTP with no encryption.

---

## Analysis 4 — SMB Traffic (Port 445)

### Filter Used
smb

### What I Found
- Samba 3.0.20 running
- SMB authentication attempts visible
- Workgroup: WORKGROUP
- NetBIOS name: METASPLOITABLE

### Security Risk
HIGH — Old SMB version vulnerable to
multiple exploits including CVE-2007-2447

---

## Key Takeaways
1. FTP and Telnet send passwords in PLAIN TEXT
2. Always use SFTP instead of FTP
3. Always use SSH instead of Telnet
4. Always use HTTPS instead of HTTP
5. Network traffic analysis can reveal
   credentials and sensitive data easily

## Tools Used
- Wireshark 4.x
- Filters: ftp, telnet, http, smb
- Feature: Follow TCP Stream
