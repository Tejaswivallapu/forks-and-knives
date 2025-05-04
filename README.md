# Forks and Knives - Hack The Box Challenge

**Contributors**:  
- Kavya Sree Chandhi  
- Venkata Sri Sai Gaman Yerramneni  
- Tejaswi Reddyvallapu  

---

## Table of Contents

- [Overview](#overview)
- [Tools Used](#tools-used)
- [Setup Instructions](#setup-instructions)
  - [Docker Environment Setup](#docker-environment-setup)
  - [VPN Connection](#vpn-connection)
- [Vulnerability Discovery](#vulnerability-discovery)
- [Attack Strategy](#attack-strategy)
  - [Stack Canary Brute-Forcing](#1-stack-canary-brute-forcing)
  - [Format String Exploit](#2-format-string-exploit)
  - [ROP Chain Construction](#3-rop-chain-construction)
- [Final Exploitation](#final-exploitation)
- [Summary of Exploitation Steps](#summary-of-exploitation-steps)
- [Key Cybersecurity Concepts](#key-cybersecurity-concepts)
- [Important Files](#important-files)
- [Screenshots](#screenshots)
- [Thank You](#thank-you)

---

## Overview

This project documents the complete exploitation process of the **Forks and Knives** Hack The Box (HTB) pwn challenge.  
We successfully performed a full end-to-end attack involving **stack canary brute-forcing**, **format string vulnerability exploitation**, and **ROP chain construction** to gain remote shell access and capture the flag.

---

## Tools Used

- **Pwntools** — for scripting the exploit and automating the attack steps
- **Netcat (nc)** — for manual interaction and testing
- **Docker** — for building a local environment based on Ubuntu 22.04
- **Python 3** — for scripting and exploitation
- **Local libc.so.6** — to resolve function offsets and construct accurate ROP chains

---

## Setup Instructions

### Docker Environment Setup

- Built a Docker image named `pwn_forks_and_knives` using Ubuntu 22.04.
- Created a user `ctf` and copied challenge files to `/home/ctf/`.
- Set permissions for the server binary.
- Verified the build with `docker images`.

### VPN Connection

- Connected to the Hack The Box environment using OpenVPN.
- Ensured access to the remote challenge server.

---

## Vulnerability Discovery

- **Buffer Overflow**:
  - Manual testing revealed that sending 255 'A' characters caused a crash, indicating a stack-based buffer overflow vulnerability.
  
- **Stack Canary**:
  - Observed that overflowing beyond 255 bytes caused immediate crashes, confirming the presence of a stack canary protection.

---

## Attack Strategy

### 1. Stack Canary Brute-Forcing

- Sent payloads byte-by-byte after 255 'A's to determine the correct canary value without crashing the server.
- Reconstructed the full 8-byte stack canary through trial and error.

### 2. Format String Exploit

- Leveraged `%2$p` format specifier to leak a memory address from the stack.
- Subtracted known offset (`lseek64+11`) to calculate the base address of `libc`.

### 3. ROP Chain Construction

- Built a ROP chain to:
  - Redirect socket file descriptors to stdin, stdout, and stderr using `dup2()`.
  - Execute a shell by calling `system("/bin/sh")`.

---

## Final Exploitation

- Successfully bypassed stack canary.
- Leaked the `libc` base address.
- Sent final payload crafted with padding, canary, fake RBP, and ROP chain.
- Gained a remote interactive shell with `io.interactive()`.
- Captured the flag.

---

## Summary of Exploitation Steps

1. Set up the Docker environment for local testing.
2. Connected to the live Hack The Box server over VPN.
3. Performed manual buffer overflow and format string testing.
4. Wrote an automated exploit script using Pwntools.
5. Brute-forced the stack canary byte-by-byte.
6. Leaked a libc address to calculate base address.
7. Constructed a working ROP chain.
8. Achieved remote code execution and captured the flag.

---

## Key Cybersecurity Concepts

- **Buffer Overflow Attack** — Exploited to overwrite stack memory.
- **Stack Canary Bypass** — Required brute-forcing correct canary value.
- **Return Oriented Programming (ROP)** — Chained existing code gadgets for execution.
- **Format String Vulnerability** — Leaked memory addresses for ASLR bypass.
- **Address Space Layout Randomization (ASLR) Bypass** — Overcame randomized memory layouts.

---

## Important Files

- `exploit.py` — Full exploitation script automating the attack.
- `Grp9.pptx` — Presentation.
- `Forks and Knives.zip` - Challenge file.
- `flag.txt` — Flag captured.

---

## Screenshots
![image](https://github.com/user-attachments/assets/de47cac0-9b6e-4042-b4ee-246bbed8774a)
![image](https://github.com/user-attachments/assets/36d062bf-33d5-4217-af75-21b624af4245)
![image](https://github.com/user-attachments/assets/e5f01987-9094-453c-b372-570b6bca593b)
![image](https://github.com/user-attachments/assets/d712074d-4206-49aa-b9a3-edfe9b7e8c94)
![image](https://github.com/user-attachments/assets/c4ce4fb5-b21e-4779-aac1-3e94891faff5)
![image](https://github.com/user-attachments/assets/bf90d9c2-1062-4d44-8554-97f4b5a4df73)
![image](https://github.com/user-attachments/assets/9c8a84e3-4b87-4196-8ea0-3a80a787056c)
![image](https://github.com/user-attachments/assets/00f76a87-90bb-49cd-afaf-0ab83f44f001)
![image](https://github.com/user-attachments/assets/ce13754e-9c44-4cb7-9c61-e3339621e457)
![image](https://github.com/user-attachments/assets/607bc546-f4ed-466b-8f92-4b35ec65ed20)
![image](https://github.com/user-attachments/assets/eafd8a51-06ea-4e4b-9054-d8e4ec590af4)
![image](https://github.com/user-attachments/assets/b001278d-d3d5-4b44-a04e-def30df67292)



























---

# Thank You!

---
