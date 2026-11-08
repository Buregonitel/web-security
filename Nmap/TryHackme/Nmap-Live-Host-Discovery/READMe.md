# Nmap Live Host Discovery — TryHackMe

**Room:** [Nmap Live Host Discovery](https://tryhackme.com/room/nmap01)  
**Difficulty:** Medium  
**Date completed:** November 2026  
**Series:** 1/4 (Nmap series)

---

## Описание

Первая комната из серии Nmap. Фокус на **обнаружении живых хостов** (host discovery) до начала сканирования портов.

Рассматриваются методы:

- **ARP scan**
- **ICMP scan** (Echo, Timestamp, Address Mask)
- **TCP/UDP Ping scan**

Также упоминаются инструменты `arp-scan` и `masscan`.

---

## Структура комнаты

| Task | Название                        | Содержание |
|------|----------------------------------|------------|
| 1    | Introduction                     | Зачем нужно host discovery |
| 2    | Subnetworks                      | Подсети, ARP и границы |
| 3    | Enumerating Targets              | Как указывать цели Nmap |
| 4    | Discovering Live Hosts           | ARP / ICMP / TCP / UDP |
| 5    | Nmap Host Discovery Using ARP    | `-PR` |
| 6    | Nmap Host Discovery Using ICMP   | `-PE`, `-PP`, `-PM` |
| 7    | Nmap Host Discovery Using TCP/UDP| `-PS`, `-PA`, `-PU` |
| 8    | Using Reverse-DNS Lookup         | `-n`, `-R` |
| 9    | Summary                          | Сводка флагов |

---

## Ключевые ответы

### Task 2 – Subnetworks (симулятор)

**Отправка ARP Request от computer1 к computer6 (broadcast):**
- How many devices can see the ARP Request? → **4**
- Did computer6 receive the ARP Request? → **N**

**Отправка ARP Request от computer4 к computer6:**
- How many devices can see the ARP Request? → **4**
- Did computer6 receive the ARP Request? → **Y**

### Task 3 – Enumerating Targets

- First IP Nmap would scan for `10.10.12.13/29` → **10.10.12.8**
- How many IP addresses for range `10.10.0-255.101-125` → **6400**

### Task 4 – Discovering Live Hosts (симулятор)

**Ping от computer1 к computer3:**
- Тип пакета до ping → **ARP Request**
- Тип пакета, который получил computer1 → **ARP Response**
- Сколько компьютеров ответило на ping → **1**

**Ping от computer2 к computer5:**
- Первый устройство, ответившее на первый ARP → **router**
- Первый устройство на второй ARP → **computer5**
- Нужны ли новые ARP при повторном ping? → **N**

### Task 5 – ARP Scan

```bash
sudo nmap -PR -sn <target>/24
```

### Task 6 – ICMP Scans

| Метод              | Флаг  |
|--------------------|-------|
| ICMP Echo          | `-PE` |
| ICMP Timestamp     | `-PP` |
| ICMP Address Mask  | `-PM` |

### Task 7 – TCP / UDP Ping

| Метод            | Флаг              |
|------------------|-------------------|
| TCP SYN Ping     | `-PS<ports>`      |
| TCP ACK Ping     | `-PA<ports>`      |
| UDP Ping         | `-PU<ports>`      |

Примеры:
```bash
sudo nmap -PS22,80,443 -sn <target>
sudo nmap -PA22,80,443 -sn <target>
sudo nmap -PU53,161 -sn <target>
```

### Task 8 – Reverse-DNS

- Не делать DNS-резолв → `-n`
- Всегда делать reverse-DNS → `-R`

---

## Полезные команды (сводка)

```bash
# Только host discovery (без скана портов)
nmap -sn <targets>

# ARP scan (локальная сеть)
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

# Без DNS
nmap -n -sn <targets>

# Список целей без сканирования
nmap -sL -n <targets>
```

---

## Важные выводы

- **ARP** работает только в пределах одной подсети (link-layer).
- Если ICMP заблокирован, помогают TCP SYN/ACK и UDP ping.
- Флаг **`-sn`** отключает сканирование портов — только discovery.
- **`-Pn`** пропускает host discovery полностью (считает все хосты живыми).
- На локальной сети Nmap по умолчанию предпочитает ARP.

---

## Связанные комнаты серии

1. **Nmap Live Host Discovery** ← текущая
2. [Nmap Basic Port Scans](https://tryhackme.com/room/nmap02)
3. [Nmap Advanced Port Scans](https://tryhackme.com/room/nmap03)
4. [Nmap Post Port Scans](https://tryhackme.com/room/nmap04)
