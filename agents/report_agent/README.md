# Report Writer Agent

Security report generation agent for HTB Expressway engagement.

## Quick Start

1. Wait for Red Team Agent to complete attack
2. Agent reads findings from `sessions/attack_complete.json`
3. Generates professional reports in multiple formats
4. Outputs saved to `reports/` directory

## Input

From Red Team Agent:
- `sessions/attack_complete.json` - Main findings
- `output/phase*.json` - Phase logs
- `output/*.txt` - Raw outputs

## Output

| File | Format | Purpose |
|------|--------|---------|
| PTR-{ID}_report.md | Markdown | Full report |
| PTR-{ID}_report.html | HTML | Styled presentation |
| PTR-{ID}_report.json | JSON | Machine-readable |
| PTR-{ID}_executive.md | Markdown | Executive summary |

## Report Sections

1. Executive Summary
2. Methodology
3. Technical Findings
4. Attack Narrative
5. Remediation Roadmap
6. MITRE ATT&CK Mapping
7. Appendices

## Standards Followed

- OWASP OPTRS (Penetration Test Reporting Standard)
- PTES (Penetration Testing Execution Standard)
- NIST Cybersecurity Framework
- MITRE ATT&CK Framework

## Files

| File | Purpose |
|------|---------|
| AGENTS.md | Identity & responsibilities |
| CONTEXT.md | Quick start guide |
| SKILLS.md | Documentation procedures |
| TODO.md | Report tasks |
| CHAT.md | How to interact |

---

**Status**: Ready - Awaiting Red Team Agent findings
