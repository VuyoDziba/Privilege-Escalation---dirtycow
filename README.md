# Privilege-Escalation---dirtycow# Linux Privilege Escalation Lab – Dirty COW Exploit

---

# Project Overview

This project demonstrates a **Linux privilege escalation attack** using the **Dirty COW vulnerability (CVE-2016-5195)**.

The lab shows how a normal user account can exploit a vulnerability in the Linux kernel to gain **root-level privileges**.

The attack was performed in a controlled lab environment using a vulnerable Debian system and an attacker machine running Kali Linux.

---

# What is Privilege Escalation?

Privilege escalation is a cybersecurity attack where a user gains **higher permissions than originally allowed** on a system.

Attackers often use privilege escalation to:

* Gain administrator/root access
* Install malicious software
* Access restricted files
* Control the entire system

There are two main types:

### Vertical Privilege Escalation

The attacker increases their privilege level.

Example:

```
User → Administrator → Root
```

### Horizontal Privilege Escalation

The attacker accesses another user account **with the same privilege level**.

Example:

```
User A → User B
```

---

# Vulnerability Used: Dirty COW

The **Dirty COW vulnerability** is a race condition in the Linux kernel's **copy-on-write (COW)** memory management system.

It allows an unprivileged user to **modify read-only memory mappings**, which can lead to privilege escalation.

Details:

* Vulnerability ID: **CVE-2016-5195**
* Affected systems: Linux Kernel **2.6.22 – 4.8**
* Impact: Local privilege escalation to root

---

# Lab Environment

## Attacker Machine

* Kali Linux
* Tools used:

  * SSH
  * GCC
  * Linux commands

## Victim Machine

* Debian Linux
* Kernel version: 2.6.32
* Vulnerable to Dirty COW

---

# Exploitation Steps

## 1. Setup the Vulnerable Machine

Download the vulnerable lab environment from:

https://github.com/sagishahar/lpeworkshop

Import the machine into **VirtualBox or VMware**.

---

## 2. Login to the Victim Machine

Credentials:

```
Username: user
Password: password321
```

Check the system IP address:

```
ifconfig
```

Example:

```
192.168.56.103
```

---

## 3. Connect from Kali Linux

From the attacker machine:

```
ssh user@192.168.56.103
```

Enter the password when prompted.

---

## 4. Perform System Enumeration

Check the Linux kernel version:

```
uname -a
```

Check detailed kernel information:

```
cat /proc/version
```

The system is vulnerable to the **Dirty COW exploit**.

---

## 5. Create the Exploit File

Create a C file for the exploit:

```
nano dirty.c
```

Paste the Dirty COW exploit code inside the file.

---

## 6. Compile the Exploit

Compile the exploit using GCC:

```
gcc -pthread dirty.c -o dirty -lcrypt
```

---

## 7. Execute the Exploit

Run the compiled exploit:

```
./dirty
```

You will be asked to enter a password.

Example:

```
Enter password: root123
```

---

## 8. Switch to the New Root User

The exploit creates a new root user.

Login using:

```
su firefart
```

Enter the password you created.

---

## 9. Verify Root Access

Check privileges:

```
id
```

Expected output:

```
uid=0(root) gid=0(root)
```

This confirms **successful privilege escalation**.

---

# Proof of Exploitation

Example commands to verify root access:

```
id
cat /etc/shadow
```

If these commands execute successfully, the system has been compromised.

---

# Screenshots

## SSH Connection

![SSH Login](screenshots/ssh-login.png)

## Kernel Enumeration

![Kernel Version](screenshots/kernel-version.png)

## Exploit Compilation

![Compile Exploit](screenshots/compile-exploit.png)

## Root Access

![Root Access](screenshots/root-access.png)

---

# Technical Explanation

The Dirty COW exploit abuses a race condition in the Linux kernel's memory subsystem.

Normally, **copy-on-write (COW)** ensures that multiple processes can safely share memory.

However, the vulnerability allows a malicious process to:

1. Continuously trigger memory writes
2. Exploit the race condition
3. Modify protected files such as `/etc/passwd`
4. Create a new user with **UID 0 (root privileges)**

Once the user is created, the attacker logs in and gains full system control.

---

# Security Impact

If exploited on a real system, an attacker could:

* Gain root access
* Modify system files
* Install malware
* Steal sensitive data
* Control the entire server

---

# Mitigation

To protect against Dirty COW attacks:

* Update the Linux kernel
* Apply security patches
* Restrict local access
* Use modern Linux distributions

---

# References

* CVE-2016-5195 – Dirty COW Vulnerability
* Linux Kernel Security Documentation
* Privilege Escalation Research
* Exploit database references
