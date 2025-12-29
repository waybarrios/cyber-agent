# Red Team Agent

Penetration testing agent for HTB Expressway machine.

## Quick Start

1. Provide target IP
2. Agent executes 6-phase attack methodology
3. All findings logged to `output/` directory
4. Final report data in `sessions/attack_complete.json`

## Attack Phases

| Phase | Objective | Output |
|-------|-----------|--------|
| 1. Reconnaissance | Port scanning | tcp_scan.txt, udp_scan.txt |
| 2. Enumeration | IKE/IPsec analysis | ike_*.txt, ike.psk |
| 3. Credential Attack | PSK cracking | cracked_psk.txt |
| 4. Initial Access | SSH + user flag | user_flag.txt |
| 5. Privilege Escalation | CVE-2025-32463 | exploit_setup.txt |
| 6. Post-Exploitation | Root flag | root_flag.txt |

## Files

| File | Purpose |
|------|---------|
| AGENTS.md | Identity & responsibilities |
| CONTEXT.md | Quick start guide |
| SKILLS.md | Attack procedures |
| TODO.md | Attack plan & tasks |
| CHAT.md | How to interact |

## Scripts

### scan_progress.sh - Port Scanner with Progress Bar

Nmap wrapper with real-time progress and port presets. **Scan common ports first for speed.**

```bash
chmod +x scripts/scan_progress.sh
./scripts/scan_progress.sh <TARGET> [PRESET] [OUTPUT]
```

**Port Presets:**

| Preset | Ports | Use Case |
|--------|-------|----------|
| `common` | 21,22,23,25,53,80,110,111,135,139,143,443,445,993,995,1723,3306,3389,5432,5900,8080,8443 | Default |
| `web` | 80,443,8000,8080,8443,8888,9000,9090,9443 | Web apps |
| `db` | 1433,1521,3306,5432,6379,27017 | Databases |
| `vpn` | 500,1194,1701,1723,4500 | VPN/IKE |
| `remote` | 22,23,3389,5900,5985,5986 | Remote access |
| `all` | 1-65535 | Full (slow) |

**Examples:**
```bash
./scripts/scan_progress.sh 10.10.11.87           # Common ports
./scripts/scan_progress.sh 10.10.11.87 vpn       # VPN ports (IKE)
./scripts/scan_progress.sh 10.10.11.87 all       # All ports
./scripts/scan_progress.sh 10.10.11.87 22,80,443 # Specific ports
```

---

## Output for Report Agent

All outputs saved for Report Writer Agent consumption:
- `output/` - Raw command outputs
- `sessions/attack_complete.json` - Structured findings

## Vulnerabilities Discovered

1. **VULN-001**: IKE Service Exposed (MEDIUM)
2. **VULN-002**: IKE Aggressive Mode (HIGH)
3. **VULN-003**: CVE-2025-32463 Sudo (CRITICAL)

---

**Status**: Ready - Awaiting target IP
