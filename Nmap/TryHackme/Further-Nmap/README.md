# Further Nmap - TryHackMe

**Room:** [Further Nmap](https://tryhackme.com/room/furthernmap)  
**Difficulty:** Easy  
**Date completed:** 07 November 2026  
**Platform:** TryHackMe AttackBox  
**Series:** Nmap

---

## Description

This room focuses on more advanced usage of Nmap. Main topics covered:

- Different scan types (SYN, Connect, Xmas, Null, FIN)
- Firewall behaviour and port states (`open`, `closed`, `filtered`, `open|filtered`)
- ICMP handling
- NSE (Nmap Scripting Engine)

---

## Screenshots

### Practice - results
![Practice Results](practical.png)

---

## Room Structure

| Task | Title                              | Summary |
|------|------------------------------------|---------|
| 2    | Introduction                       | What is Nmap and ports |
| 3    | Nmap Switches                      | Main flags |
| 4    | Scan Types – Overview              | Overview of scan types |
| 5    | TCP Connect Scans                  | `-sT` |
| 6    | SYN Scans                          | `-sS` (half-open / stealth) |
| 7    | UDP Scans                          | `-sU` |
| 8    | NULL, FIN and Xmas                 | `-sN`, `-sF`, `-sX` |
| 9    | ICMP Network Scanning              | Ping scans |
| 10   | NSE Scripts – Overview             | Introduction to scripts |
| 11   | NSE Scripts – Working with NSE     | Running scripts |
| 12   | NSE Scripts – Searching for Scripts| Finding scripts |
| 13   | Firewall Evasion                   | Bypassing filters |
| 14   | Practical                          | Hands-on practice |

---

## Key Answers

### Task 2 - Introduction

- Networking constructs used to direct traffic → **Ports**
- How many available → **65535**
- How many considered “well-known” → **1024**

### Task 3 - Nmap Switches

| Question | Answer |
|----------|--------|
| First switch for Syn Scan | `-sS` |
| Switch for UDP scan | `-sU` |
| Detect OS | `-O` |
| Detect service version | `-sV` |
| Increase verbosity | `-v` |
| Verbosity level 2 | `-vv` |
| Save in three major formats | `-oA` |
| Save in normal format | `-oN` |
| Save in grepable format | `-oG` |
| Aggressive mode | `-A` |
| Timing template level 5 | `-T5` |
| Scan only port 80 | `-p 80` |
| Scan ports 1000-1500 | `-p 1000-1500` |
| Scan all ports | `-p-` |
| Activate a script | `--script` |
| Activate all vuln scripts | `--script=vuln` |

### Task 5 - TCP Connect Scans

- RFC that defines TCP behaviour → **RFC 793**
- Flag sent when port is closed → **RST**

### Task 6 - SYN Scans

- Other names for SYN scan → **Half-Open, Stealth**

### Task 14 - Practical

**Target IP:** `10.113.183.10`

#### 1. ICMP Echo
```bash
ping -c 4 10.113.183.10
```
**Answer:** `N` (100% packet loss)

#### 2. Xmas Scan (ports 1–999)
```bash
sudo nmap -sX -Pn -p 1-999 10.113.183.10
```
**Answer:** `999` (open|filtered)

#### 3. Reason
**Answer:** `No Response`

#### 4. TCP SYN Scan (ports 1–5000)
```bash
sudo nmap -sS -Pn -p 1-5000 10.113.183.10
```
**Answer:** `5` open ports

| Port | Service         |
|------|-----------------|
| 21   | ftp             |
| 53   | domain          |
| 80   | http            |
| 135  | msrpc           |
| 3389 | ms-wbt-server   |

#### 5. NSE - ftp-anon
```bash
sudo nmap -sV -p 21 --script ftp-anon 10.113.183.10
```
**Answer:** `Y`  
`Anonymous FTP login allowed (FTP code 230)`

---

## Useful Commands

```bash
# Basic SYN scan
sudo nmap -sS -Pn -T4 <IP>

# With service and OS detection
sudo nmap -sS -sV -O -Pn <IP>

# Xmas / Null / FIN
sudo nmap -sX -Pn <IP>
sudo nmap -sN -Pn <IP>
sudo nmap -sF -Pn <IP>

# UDP scan
sudo nmap -sU -Pn <IP>

# All ports
sudo nmap -sS -Pn -p- <IP>

# NSE
sudo nmap --script=vuln <IP>
sudo nmap -p 21 --script ftp-anon <IP>

# Save results
sudo nmap -sS -Pn -oA scan_results <IP>
```

---

## Key Takeaways

- `-Pn` is almost always needed on TryHackMe (machines often do not reply to ICMP)
- SYN scan (`-sS`) is the main working method (faster and stealthier than Connect)
- Xmas / Null / FIN are useful when regular SYN is blocked by a firewall
- NSE scripts greatly speed up enumeration (example with `ftp-anon`)
- Always save scan results (`-oA`, `-oN`, `-oG`)

---
