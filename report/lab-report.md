# Penetration Testing Report
## Network Security Lab — Metasploitable 2

---

## 1. Executive Summary
A penetration test was performed on Metasploitable 2
(192.168.30.128) in an isolated virtual lab environment.
The assessment revealed multiple critical vulnerabilities
that allowed complete system compromise.

---

## 2. Scope
| Item        | Details                    |
|-------------|----------------------------|
| Target IP   | 192.168.30.128             |
| Target OS   | Linux Ubuntu (Metasploitable 2) |
| Attacker    | Kali Linux                 |
| Network     | Host-Only (Isolated)       |
| Test Type   | Black Box Penetration Test |
| Date        | May 2026                   |

---

## 3. Tools Used
| Tool        | Purpose                    |
|-------------|----------------------------|
| Nmap        | Port scanning & enumeration|
| Metasploit  | Exploitation framework     |
| Wireshark   | Packet capture & analysis  |
| Netcat (nc) | Manual connection testing  |
| MySQL client| Database access testing    |

---

## 4. Vulnerabilities Found

### CRITICAL
| Port | Service      | CVE            | Result        |
|------|-------------|----------------|---------------|
| 1524 | Bindshell   | No CVE needed  | Instant Root  |
| 21   | vsftpd 2.3.4| CVE-2011-2523  | Root Shell    |
| 445  | Samba 3.0.20| CVE-2007-2447  | Root Shell    |
| 6667 | UnrealIRCd  | CVE-2010-2075  | Root Shell    |

### HIGH
| Port | Service   | Risk                          |
|------|-----------|-------------------------------|
| 23   | Telnet    | Credentials sent in plaintext |
| 3306 | MySQL     | No root password set          |
| 5900 | VNC       | Weak authentication           |
| 512  | rexecd    | No strong authentication      |

---

## 5. Attack Timeline
1. Ran Nmap scan → discovered 23 open ports
2. Identified vsftpd 2.3.4 → exploited CVE-2011-2523
3. Connected to Port 1524 → got instant root
4. Exploited Samba → got root via SMB
5. Captured Telnet traffic → saw plaintext password
6. Accessed MySQL → no password required
7. Collected /etc/shadow → password hashes obtained

---

## 6. Evidence
- scans/nmap-scan.txt — full port scan results
- wireshark/capture.pcapng — network traffic capture
- exploitation/*.md — step by step exploit details
- screenshots/ — visual proof of each exploit

---

## 7. Recommendations (How to Fix)
| Vulnerability     | Fix                              |
|-------------------|----------------------------------|
| vsftpd backdoor   | Update to latest vsftpd version  |
| Open bindshell    | Close port 1524 immediately      |
| Samba old version | Update Samba to latest version   |
| Telnet open       | Disable Telnet, use SSH only     |
| MySQL no password | Set strong root password         |
| VNC weak auth     | Use VPN + strong password        |

---

## 8. Conclusion
The target machine was fully compromised through
multiple attack vectors. All critical services were
outdated and misconfigured. This demonstrates the
importance of regular patching and security audits.

## Disclaimer
This test was performed in a 100% isolated lab
on an intentionally vulnerable machine.
No real systems were targeted.
