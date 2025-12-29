# Red Team Agent Sessions

Session files store attack progress for continuity if interrupted.

## File Format

```
{NNN}_{YYYYMMDD}_{TARGET_IP}_session.md
```

Example: `001_20251229_10.10.11.87_session.md`

## Session Template

```markdown
# Session {NNN} - {DATE}

## Target
- IP: {TARGET_IP}
- Platform: HackTheBox / Other

## Status
- [ ] Phase 1: Reconnaissance
- [ ] Phase 2: Enumeration
- [ ] Phase 3: Credential Attack
- [ ] Phase 4: Initial Access
- [ ] Phase 5: Privilege Escalation
- [ ] Phase 6: Post-Exploitation

## Current Phase
Phase {N}: {Name}

## Discovered Information

| Item | Value |
|------|-------|
| SSH Port | |
| IKE Port | |
| Identity | |
| Password | |
| User Flag | |
| Sudo Path | |
| Sudo Version | |
| Root Flag | |

## Commands Executed
```bash
# Phase 1
nmap -sV -sC -p- {IP}

# Phase 2
ike-scan -A {IP}
```

## Key Findings
- Finding 1
- Finding 2

## Next Steps
- Next action to take

## Errors/Blockers
- Any issues encountered
```

## Usage

**Save session**: After each phase or significant discovery
**Load session**: At start of new conversation, read latest session file

## Agent Instructions

When resuming:
1. Read the latest session file in this folder
2. Continue from the current phase
3. Update the session file as you progress
