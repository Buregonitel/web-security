# Further Nmap — TryHackMe

**Room:** [Further Nmap](https://tryhackme.com/room/furthernmap)  
**Difficulty:** Easy  
**Date completed:** 07 November 2026  
**Platform:** TryHackMe AttackBox

---

## Описание

Комната посвящена одному из самых важных инструментов в пентесте — **Nmap** (Network Mapper).

Темы, которые разбираются:

- Основные переключатели Nmap
- Типы сканирования (TCP Connect, SYN, UDP, Null, FIN, Xmas)
- Работа с ICMP
- NSE (Nmap Scripting Engine)
- Обход файрволов
- Практическое применение

---

## Скриншоты

### Practice — результаты прохождения

![Practice Results](practical.png)

---

## Структура комнаты

| Task | Название                          | Кратко |
|------|-----------------------------------|--------|
| 2    | Introduction                      | Что такое Nmap и порты |
| 3    | Nmap Switches                     | Основные флаги |
| 4    | Scan Types – Overview             | Обзор типов сканов |
| 5    | TCP Connect Scans                 | `-sT` |
| 6    | SYN Scans                         | `-sS` (half-open / stealth) |
| 7    | UDP Scans                         | `-sU` |
| 8    | NULL, FIN and Xmas                | `-sN`, `-sF`, `-sX` |
| 9    | ICMP Network Scanning             | Ping-сканы |
| 10   | NSE Scripts – Overview            | Введение в скрипты |
| 11   | NSE Scripts – Working with NSE    | Запуск скриптов |
| 12   | NSE Scripts – Searching for Scripts | Поиск скриптов |
| 13   | Firewall Evasion                  | Обход фильтров |
| 14   | Practical                         | Практика |

---

## Ключевые ответы по заданиям

### Task 2 – Introduction

- Networking constructs used to direct traffic → **Ports**
- How many available → **65535**
- How many considered “well-known” → **1024**

### Task 3 – Nmap Switches

| Вопрос | Ответ |
|--------|-------|
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

### Task 5 – TCP Connect Scans

- RFC that defines TCP behaviour → **RFC 793**
- Flag sent when port is closed → **RST**

### Task 6 – SYN Scans

- Other names for SYN scan → **Half-Open, Stealth**

### Task 14 – Practical (выполнено)

**Целевой IP:** `10.113.183.10`

#### 1. ICMP Echo
```bash
ping -c 4 10.113.183.10
```
**Ответ:** `N` (100% packet loss)

#### 2. Xmas Scan (1-999)
```bash
sudo nmap -sX -Pn -p 1-999 10.113.183.10
```
**Ответ:** `999` (open|filtered)

#### 3. Причина
**Ответ:** `No Response`

#### 4. TCP SYN Scan (1-5000)
```bash
sudo nmap -sS -Pn -p 1-5000 10.113.183.10
```
**Ответ:** `5` открытых портов

| Port | Service         |
|------|-----------------|
| 21   | ftp             |
| 53   | domain          |
| 80   | http            |
| 135  | msrpc           |
| 3389 | ms-wbt-server   |

#### 5. NSE – ftp-anon
```bash
sudo nmap -sV -p 21 --script ftp-anon 10.113.183.10
```
**Ответ:** `Y`  
`Anonymous FTP login allowed (FTP code 230)`

---

## Полезные команды

```bash
# Базовый SYN-скан
sudo nmap -sS -Pn -T4 <IP>

# С версиями сервисов и ОС
sudo nmap -sS -sV -O -Pn <IP>

# Xmas / Null / FIN
sudo nmap -sX -Pn <IP>
sudo nmap -sN -Pn <IP>
sudo nmap -sF -Pn <IP>

# UDP-скан
sudo nmap -sU -Pn <IP>

# Все порты
sudo nmap -sS -Pn -p- <IP>

# NSE
sudo nmap --script=vuln <IP>
sudo nmap -p 21 --script ftp-anon <IP>

# Сохранение результатов
sudo nmap -sS -Pn -oA scan_results <IP>
```

---

## Важные выводы

- **`-Pn`** почти всегда нужен на TryHackMe (машины часто не отвечают на ICMP).
- SYN-скан (`-sS`) — основной рабочий метод (быстрее и стелснее Connect-скана).
- Xmas/Null/FIN полезны, когда обычный SYN блокируется файрволом.
- NSE сильно ускоряет разведку (пример с `ftp-anon`).
- Всегда сохраняй результаты сканов (`-oA`, `-oN`, `-oG`).

---
