| Details | Information |
|---|---|
| Name | Haziq Danial Bin Nor Azan |
| Student ID | 52215225213 |
| Programme | Bachelor Of Information Technology (Hons) In Computer System Security |
| Course | IKB21403 - Vulnerability Analysis |
| Lecturer | Nor Adani Kamal Mohamad Nasir |

---

# Introduction

Enumeration is the process of gathering detailed information from a target system. In cybersecurity, enumeration helps identify open ports, operating systems, users, services, protocols, and possible vulnerabilities. The purpose of this lab is to perform different enumeration techniques against a vulnerable machine using Kali Linux tools.

The victim machine used in this lab was Metasploitable2 running on VirtualBox. Kali Linux was used as the attacker machine.

---

# Lab Environment

| Component | Description |
|---|---|
| Attacker Machine | Kali Linux |
| Victim Machine | Metasploitable2 |
| Network Type | VirtualBox Host-Only Network |
| Victim IP Address | 192.168.56.105 |

---

# Challenge 2 — Fast Nmap Scan

## Objective

To quickly identify common open ports and services running on the target machine.

## Command Used

```bash
nmap -F 192.168.56.105
```

## Explanation of Command

- `nmap` = Network scanning tool
- `-F` = Fast scan mode that scans the top 100 common ports
- `192.168.56.105` = Target victim machine IP address

## Findings

The scan identified several open ports including:

- FTP (21)
- SSH (22)
- Telnet (23)
- HTTP (80)
- SMB (445)

## Analysis

The target machine exposes multiple network services. These services may be vulnerable to attacks if outdated or misconfigured.

## Screenshot

![Fast Nmap Scan](images/challenge2.png)

---

# Challenge 5 — TTL OS Fingerprinting

## Objective

To identify the target operating system using TTL values.

## Command Used

```bash
ping 192.168.56.105
```

## Findings

```bash
TTL=64
```

## Analysis

TTL value 64 commonly indicates a Linux or Unix operating system. This suggests the target machine is running Linux.

## Screenshot

![TTL Fingerprinting](images/challenge5.png)

---

# Challenge 9 — FTP Banner Enumeration

## Objective

To identify the FTP service version running on the target machine.

## Command Used

```bash
nc 192.168.56.105 21
```

## Findings

```bash
220 (vsFTPd 2.3.4)
```

## Analysis

The target machine is running vsFTPd version 2.3.4. Older versions of FTP services may contain known vulnerabilities.

## Screenshot

![FTP Banner](images/challenge9.png)

---

# Challenge 10 — Anonymous FTP Login

## Objective

To determine whether the FTP server allows login access and directory enumeration.

## Command Used

```bash
ftp 192.168.56.105
```

## Findings

The FTP login was successful using credentials:

```bash
Username: msfadmin
Password: msfadmin
```

Directory listing showed:

```bash
vulnerable
```

## Analysis

The FTP server allows authenticated access and exposes accessible directories. Attackers may use FTP enumeration to identify sensitive files or upload malicious content.

## Screenshot

![FTP Login](images/challenge10.png)

---

# Challenge 11 — SMB NSE Enumeration

## Objective

To enumerate SMB information using Nmap NSE scripts.

## Commands Used

```bash
nmap --script smb-os-discovery -p445 192.168.56.105
```

```bash
nmap --script smb-enum-users -p445 192.168.56.105
```

## Findings

The scan revealed:

- Samba server
- Workgroup information
- SMB-related services

## Analysis

SMB enumeration can expose usernames, shares, and operating system information that may assist attackers during later attacks.

## Screenshot

![SMB Enumeration](images/challenge11.png)

---

# Challenge 12 — Enum4linux

## Objective

To enumerate SMB shares, users, groups, and NetBIOS information.

## Command Used

```bash
enum4linux -a 192.168.56.105
```

## Findings

The scan discovered:

- Users
- Groups
- NetBIOS names
- SMB shares
- Workgroup information

## Analysis

Enum4linux provides detailed information about SMB configurations and may expose sensitive system information useful for attackers.

## Screenshot

![Enum4linux](images/challenge12.png)

---

# Challenge 13 — NFS Exports

## Objective

To identify NFS shared directories available on the target machine.

## Command Used

```bash
showmount -e 192.168.56.105
```

## Findings

```bash
/ *
```

## Analysis

The target machine exposes NFS shared directories to all hosts. Improperly secured NFS shares may allow unauthorized access to files.

## Screenshot

![NFS Export](images/challenge13.png)

---

# Challenge 16 — Version Detection

## Objective

To identify service versions running on the target machine.

## Command Used

```bash
nmap -sV 192.168.56.105
```

## Findings

Detected services included:

- vsFTPd 2.3.4
- OpenSSH
- Apache HTTP Server
- Samba

## Analysis

Version detection helps identify outdated software that may contain vulnerabilities.

## Screenshot

![Version Detection](images/challenge16.png)

---

# Challenge 17 — OS Detection

## Objective

To identify the target operating system.

## Command Used

```bash
sudo nmap -O 192.168.56.105
```

## Findings

Nmap identified the target as Linux-based operating system.

## Analysis

Operating system detection helps attackers determine suitable exploitation methods.

## Screenshot

![OS Detection](images/challenge17.png)

---

# Challenge 18 — Finger Enumeration

## Objective

To determine whether the Finger service is enabled on the target machine.

## Command Used

```bash
finger @192.168.56.105
```

## Findings

```bash
finger: connect: Connection refused
```

## Analysis

The Finger service is disabled or unavailable on the target machine. This reduces information leakage because attackers cannot enumerate users through the Finger protocol.

## Screenshot

![Finger Enumeration](images/challenge18.png)

---

# Challenge 19 — RPC Information Enumeration

## Objective

To enumerate RPC services running on the target machine.

## Command Used

```bash
rpcinfo -p 192.168.56.105
```

## Findings

The scan identified:

- portmapper
- nfs
- mountd
- status services

## Analysis

RPC services provide useful information about NFS and network file-sharing configurations.

## Screenshot

![RPC Enumeration](images/challenge19.png)

---

# Challenge 29 — SMTP Enumeration via Nmap

## Objective

To enumerate SMTP users using Nmap scripts.

## Command Used

```bash
nmap -p25 --script=smtp-enum-users 192.168.56.105
```

## Findings

The SMTP service responded successfully and enumeration attempts were performed.

## Analysis

SMTP enumeration may reveal valid usernames that attackers can use during brute-force or phishing attacks.

## Screenshot

![SMTP Enumeration](images/challenge29.png)

---

# Conclusion

In this lab, multiple enumeration techniques were successfully performed against the target machine. Information gathered included open ports, operating system details, service versions, SMB information, FTP access, NFS exports, and RPC services. Enumeration is an important phase in vulnerability analysis because it helps security professionals identify exposed services and possible security weaknesses.

---

# References

- Nmap Documentation
- Kali Linux Tools
- Metasploitable2
- SMB Enumeration Techniques
- FTP Enumeration Techniques
