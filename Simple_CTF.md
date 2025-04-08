# TryHackMe: Simple CTF - Professional Walkthrough Report

**Report Title:** Simple CTF Room Analysis  
**Platform:** TryHackMe  
**Author:** Your Name  
**Date:** April 8, 2025  

---

## 1. Introduction

This report provides a comprehensive walkthrough of the **Simple CTF** room on TryHackMe — a beginner-friendly Capture The Flag (CTF) exercise. The room is ideal for individuals new to cybersecurity, as it demonstrates the fundamental skills needed to enumerate services, identify vulnerabilities, exploit them, and escalate privileges.

The walkthrough includes all major steps, tools used, and techniques involved. It also lists the prerequisite knowledge a learner should possess to successfully complete the room without external assistance.

---

## 2. Learning Objectives

By completing this room, users will gain practical experience in:

- Basic reconnaissance and enumeration techniques  
- Identifying and exploiting web application vulnerabilities  
- Cracking hashed credentials  
- Gaining remote access using SSH  
- Performing local privilege escalation  

---

## 3. Prerequisites

Before attempting this room independently, it is recommended to be familiar with the following concepts and tools:

### 3.1 Technical Knowledge

- Basic Linux terminal commands and file structure  
- Understanding of networking concepts (IP, ports, TCP/UDP)  
- Web application components (HTTP, CMS, login pages)

### 3.2 Tools & Techniques

- **Nmap** – Network scanning and service enumeration  
- **Gobuster/Dirb** – Web directory brute forcing  
- **CMS Identification** – Recognizing platforms like CMS Made Simple  
- **Exploit-DB** – Finding and using public exploits  
- **Hashcat/John** – Cracking password hashes  
- **SSH** – Secure shell for remote access  
- **Sudo and Privilege Escalation** – GTFOBins and safe privilege abuse  

---

## 4. Step-by-Step Walkthrough

### 4.1 Initial Reconnaissance

Use `nmap` to scan for open ports and detect services:

```bash
nmap -sC -sV -p- <target-ip>
```

**Discovered Ports:**
- **21**: FTP  
- **80**: HTTP  
- **2222**: SSH

---

### 4.2 Web Enumeration

Use `gobuster` to discover hidden directories:

```bash
gobuster dir -u http://<target-ip> -w /usr/share/wordlists/dirb/common.txt
```

- `/simple` is discovered.
- The path reveals a **CMS Made Simple** login page.

---

### 4.3 Vulnerability Research

- CMS Made Simple v2.2.8 is identified.
- Search for known vulnerabilities using [Exploit-DB](https://www.exploit-db.com/).
- **CVE-2019-9053** (SQL Injection) is found.
- Use a public Python exploit to retrieve hashed credentials.

---

### 4.4 Cracking Credentials

- Crack the hash using `John the Ripper` or `Hashcat`.

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

- Use recovered credentials to SSH into the system:

```bash
ssh <username>@<target-ip> -p 2222
```

- Retrieve the `user.txt` flag from the home directory.

---

### 4.5 Privilege Escalation

- Check for sudo privileges:

```bash
sudo -l
```

- The user can run `vim` as root.
- Use this command to escalate privileges:

```bash
sudo vim -c '!sh'
```

- Access root shell and retrieve `root.txt`.

---

## 5. Conclusion

The **Simple CTF** room offers practical exposure to core penetration testing techniques, including:

- Service enumeration  
- Web application vulnerability exploitation  
- Credential cracking  
- Privilege escalation using known binaries

This room is ideal for beginners and provides a strong foundation for tackling more advanced CTFs and real-world penetration testing tasks.

---

## 6. References

- [TryHackMe - Simple CTF](https://tryhackme.com/room/simplectf)  
- [Exploit-DB - CMS Made Simple SQLi (CVE-2019-9053)](https://www.exploit-db.com/exploits/46635)  
- [GTFOBins - Vim Privilege Escalation](https://gtfobins.github.io/gtfobins/vim/)  
- [Nmap](https://nmap.org)  
- [Gobuster](https://github.com/OJ/gobuster)  
