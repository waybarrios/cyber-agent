# Cyber Agent - Claude Code Instructions

## Overview

This project contains two AI agents for penetration testing the HTB Expressway machine:

| Agent | Purpose |
|-------|---------|
| `redteam-agent` | Executes penetration testing attacks |
| `report-agent` | Generates professional security reports |

---

## How to Use the Agents

### Step 1: Navigate to the Project

```bash
cd /path/to/cyber-agent
```

### Step 2: Start Claude Code

```bash
claude
```

### Step 3: View Available Agents

```
/agents
```

This shows all available agents including `redteam-agent` and `report-agent`.

---

## Invoking Agents

### Method 1: Direct Invocation

Simply mention what you want to do and Claude will use the appropriate agent:

```
Attack the HTB Expressway machine at {TARGET_IP}
```

Claude will automatically invoke `redteam-agent` based on the task description.

### Method 2: Explicit Agent Request

```
Use the redteam-agent to attack {TARGET_IP}
```

```
Use the report-agent to generate the security report
```

### Method 3: Via /agents Menu

```
/agents
```

Then select the agent you want to use from the interactive menu.

---

## Agent Definitions

Agents are defined in `.claude/agents/`:

```
.claude/agents/
├── redteam-agent.md    # Penetration testing agent
└── report-agent.md     # Report generation agent
```

Each agent has:
- **YAML frontmatter**: name, description, tools, model
- **System prompt**: Instructions and procedures

---

## Red Team Agent

### Description
Executes penetration testing operations against HTB Expressway.

### Tools Available
- `Read`, `Write`, `Edit` - File operations
- `Bash` - Execute pentesting tools (nmap, ike-scan, psk-crack, ssh, gcc)
- `Grep`, `Glob` - Search operations

### Pentesting Tools (via Bash)
| Tool | Purpose |
|------|---------|
| `nmap` | Port scanning (TCP/UDP) |
| `ike-scan` | IKE/IPsec enumeration |
| `psk-crack` | IKE PSK cracking |
| `hashcat` | GPU password cracking |
| `ssh` | Remote shell access |
| `gcc` | Compile exploits |

### How to Invoke
```
Attack the HTB Expressway machine at {TARGET_IP}
```
or
```
Use the redteam-agent to pentest {TARGET_IP}
```

### Output
- Attack logs in `agents/redteam_agent/output/`
- Final findings in `agents/redteam_agent/sessions/attack_complete.json`

---

## Report Writer Agent

### Description
Generates professional penetration testing reports from Red Team Agent findings.

### Tools Available
- `Read`, `Write`, `Edit` - File operations
- `Bash` - Execute commands
- `Grep`, `Glob` - Search operations

### How to Invoke
```
Generate the security report for the Expressway engagement
```
or
```
Use the report-agent to create the penetration testing report
```

### Output
Reports saved to `reports/`:
- `PTR-{ID}_report.md` - Markdown
- `PTR-{ID}_report.html` - HTML
- `PTR-{ID}_report.json` - JSON

---

## Workflow Example

### 1. Start Attack
```
User: Attack the HTB Expressway machine at 10.129.x.x

Claude: [Invokes redteam-agent]
- Reading agent documentation...
- Starting Phase 1: Reconnaissance
- Running nmap TCP scan...
- Running nmap UDP scan...
[Continues through all 6 phases]
- Attack complete. Findings saved.
```

### 2. Generate Report
```
User: Generate the security report

Claude: [Invokes report-agent]
- Reading attack findings...
- Generating executive summary...
- Documenting vulnerabilities...
- Creating remediation roadmap...
- Reports saved to reports/
```

---

## Agent Documentation

Each agent has detailed documentation in its folder:

### Red Team Agent (`agents/redteam_agent/`)
| File | Purpose |
|------|---------|
| `AGENTS.md` | Identity & responsibilities |
| `CONTEXT.md` | Quick start guide |
| `SKILLS.md` | Attack procedures |
| `TODO.md` | Attack plan |
| `CHAT.md` | Interaction guide |

### Report Agent (`agents/report_agent/`)
| File | Purpose |
|------|---------|
| `AGENTS.md` | Identity & responsibilities |
| `CONTEXT.md` | Quick start guide |
| `SKILLS.md` | Report procedures |
| `TODO.md` | Report tasks |
| `CHAT.md` | Interaction guide |

---

## Quick Commands

```bash
# View available agents
/agents

# Read Red Team Agent skills
Read agents/redteam_agent/SKILLS.md

# Read Report Agent skills
Read agents/report_agent/SKILLS.md

# Check attack plan
Read agents/redteam_agent/TODO.md

# Check report tasks
Read agents/report_agent/TODO.md
```

---

## Prerequisites

Install pentesting tools on your attack machine (Kali/Parrot):

```bash
sudo apt update
sudo apt install nmap ike-scan hashcat sshpass gcc build-essential
```

---

## File Structure

```
cyber-agent/
├── CLAUDE.md                     # This file
├── README.md                     # Project overview
│
├── .claude/agents/               # Agent definitions
│   ├── redteam-agent.md
│   └── report-agent.md
│
├── agents/
│   ├── redteam_agent/            # Red Team documentation
│   │   ├── AGENTS.md
│   │   ├── CONTEXT.md
│   │   ├── SKILLS.md
│   │   ├── TODO.md
│   │   ├── CHAT.md
│   │   ├── output/               # Attack outputs
│   │   └── sessions/             # attack_complete.json
│   │
│   └── report_agent/             # Report Writer documentation
│       ├── AGENTS.md
│       ├── CONTEXT.md
│       ├── SKILLS.md
│       ├── TODO.md
│       └── CHAT.md
│
└── reports/                      # Generated reports
```

---

## Important Notes

1. **Target IP**: Always provided by user (from HTB when spawning machine)
2. **Authorization**: Only use on HTB lab environments
3. **Agent handoff**: Red Team Agent → Report Agent (via attack_complete.json)
4. **Output formats**: Reports in MD, HTML, and JSON

---

**Read this file first when working with the Cyber Agent system.**
