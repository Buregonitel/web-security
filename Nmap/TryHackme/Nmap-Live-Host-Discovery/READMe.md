# Nmap Live Host Discovery - TryHackMe

**Room:** [Nmap Live Host Discovery](https://tryhackme.com/room/nmap01)  
**Difficulty:** Medium  
**Date completed:** November 2026  
**Platform:** TryHackMe AttackBox  
**Series:** 1/4 (Nmap series)

---

## Description

First room in the Nmap series. Focus is on **live host discovery** before starting port scanning.

Methods covered:

- **ARP scan**
- **ICMP scan** (Echo, Timestamp, Address Mask)
- **TCP/UDP Ping scan**

Also introduces `arp-scan` and `masscan`.

---

## Room Structure

| Task | Title                              | Summary |
|------|------------------------------------|---------|
| 1    | Introduction                       | Why host discovery is needed |
| 2    | Subnetworks                        | Subnets, ARP and boundaries |
| 3    | Enumerating Targets                | How to specify targets in Nmap |
| 4    | Discovering Live Hosts             | ARP / ICMP / TCP / UDP |
| 5    | Nmap Host Discovery Using ARP      | `-PR` |
| 6    | Nmap Host Discovery Using ICMP     | `-PE`, `-PP`, `-PM` |
| 7    | Nmap Host Discovery Using TCP/UDP  | `-PS`, `-PA`, `-PU` |
| 8    | Using Reverse-DNS Lookup           | `-n`, `-R` |
| 9    | Summary                            | Flags overview |

---

## Key Answers

### Task 2 - Subnetworks (simulator)

**Sending ARP Request from computer1 to computer6 (broadcast):**
- How many devices can see the ARP Request? → **4**
- Did computer6 receive the ARP Request? → **N**

**Sending ARP Request from computer4 to computer6:**
- How many devices can see the ARP Request? → **4**
- Did computer6 receive the ARP Request? → **Y**

### Task 3 - Enumerating Targets

- First IP Nmap would scan for `10.10.12.13/29` → **10.10.12.8**
- How many IP addresses for range `10.10.0-255.101-125` → **6400**

### Task 4 - Discovering Live Hosts (simulator)

**Ping from computer1 to computer3:**
- Packet type before the ping → **ARP Request**
- Packet type received by computer1 → **ARP Response**
- How many computers responded to the ping → **1**

**Ping from computer2 to computer5:**
- First device that responded to the first ARP → **router**
- First device that responded to the second ARP → **computer5**
- Did another Ping Request require new ARP Requests? → **N**

### Task 5 - ARP Scan

```bash
sudo nmap -PR -sn <target>/24
```

### Task 6 – ICMP Scans

| Method              | Flag  |
|---------------------|-------|
| ICMP Echo           | `-PE` |
| ICMP Timestamp      | `-PP` |
| ICMP Address Mask   | `-PM` |

### Task 7 - TCP / UDP Ping

| Method            | Flag              |
|-------------------|-------------------|
| TCP SYN Ping      | `-PS<ports>`      |
| TCP ACK Ping      | `-PA<ports>`      |
| UDP Ping          | `-PU<ports>`      |

Examples:
```bash
sudo nmap -PS22,80,443 -sn <target>
sudo nmap -PA22,80,443 -sn <target>
sudo nmap -PU53,161 -sn <target>
```

### Task 8 - Reverse-DNS

- Never do DNS resolution → `-n`
- Always do reverse-DNS → `-R`

---

## Useful Commands

```bash
# Host discovery only (no port scan)
nmap -sn <targets>

# ARP scan (local network)
sudo nmap -PR -sn 192.168.1.0/24

# ICMP Echo
sudo nmap -PE -sn 10.10.0.0/24

# ICMP Timestamp
sudo nmap -PP -sn 10.10.0.0/24

# ICMP Address Mask
sudo nmap -PM -sn 10.10.0.0/24

# TCP SYN Ping
sudo nmap -PS22,80,443 -sn 10.10.0.0/24

# TCP ACK Ping
sudo nmap -PA22,80,443 -sn 10.10.0.0/24

# UDP Ping
sudo nmap -PU53,161,162 -sn 10.10.0.0/24

# No DNS
nmap -n -sn <targets>

# List targets without scanning
nmap -sL -n <targets>
```

---

## Key Takeaways

- **ARP** only works within the same subnet (link-layer)
- If ICMP is blocked, TCP SYN/ACK and UDP ping help
- Flag **`-sn`** disables port scanning — discovery only
- **`-Pn`** skips host discovery completely (treats all hosts as online)
- On a local network Nmap prefers ARP by default

---

## Related Rooms in the Series

1. **Nmap Live Host Discovery** ← current
2. [Nmap Basic Port Scans](https://tryhackme.com/room/nmap02)
3. [Nmap Advanced Port Scans](https://tryhackme.com/room/nmap03)
4. [Nmap Post Port Scans](https://tryhackme.com/room/nmap04)

---
