# TryHackMe CTF Writeups

Writeups for [TryHackMe](https://tryhackme.com) rooms focused on enumeration,
exploitation, and Linux privilege escalation. Flags are redacted; IP addresses
are the original lab IPs and no longer route to live machines.

| Room | Difficulty | Initial access | Privilege escalation |
|------|-----------|----------------|----------------------|
| [Anonymous](writeups/anonymous.md) | Medium | Anonymous FTP + writable cron script | SUID `/usr/bin/env` |
| [Broker](writeups/broker.md) | Medium | Apache ActiveMQ 5.9.0 unauth upload (Metasploit) | Sudo + writable Python script |
| [LazyAdmin](writeups/lazyadmin.md) | Easy | SweetRice CMS admin → PHP reverse shell | Sudo Perl chain → writable `copy.sh` → SUID bash |
| [Wonderland](writeups/wonderland.md) | Medium | Web directory enumeration → SSH credentials | CVE-2021-4034 (PwnKit) |

## Tools used across these rooms

`nmap` · `gobuster` · `ftp` · `nc` · `msfconsole` · `meterpreter` · `ssh` ·
`sudo -l` · `find` for SUID enumeration

## Disclaimer

All activity was performed inside the TryHackMe lab environment against
machines I had authorization to attack. Flags are redacted. Performing any
of these techniques against systems you do not have explicit permission to
test is illegal.
