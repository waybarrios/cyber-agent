# Report Agent Sessions

Session files store report generation progress for continuity if interrupted.

## File Format

```
{NNN}_{YYYYMMDD}_{REPORT_ID}_session.md
```

Example: `001_20251229_PTR-20251229_session.md`

## Session Template

```markdown
# Session {NNN} - {DATE}

## Report Info
- Report ID: PTR-{YYYYMMDD}
- Target: {TARGET_IP}
- Source: attack_complete.json

## Status
- [ ] Read attack findings
- [ ] Generate executive summary
- [ ] Generate technical report
- [ ] Quality check

## Current Task
{Current task description}

## Input Data Loaded
- attack_complete.json: Yes/No
- Phase logs: Yes/No
- Output files: Yes/No

## Report Progress

### Executive Summary
- Status: Not Started / In Progress / Complete
- File: PTR-{ID}_executive.md

### Technical Report
- Status: Not Started / In Progress / Complete
- File: PTR-{ID}_technical.md

## Vulnerabilities Documented

| ID | Title | Status |
|----|-------|--------|
| VULN-001 | IKE Service Exposed | [ ] |
| VULN-002 | IKE Aggressive Mode | [ ] |
| VULN-003 | Sudo CVE-2025-32463 | [ ] |

## Sections Completed
- [ ] Executive Summary
- [ ] Methodology
- [ ] Technical Findings
- [ ] Attack Narrative
- [ ] Remediation Roadmap
- [ ] MITRE ATT&CK Mapping
- [ ] Appendices

## Next Steps
- Next action to take

## Errors/Blockers
- Any issues encountered
```

## Usage

**Save session**: After completing each report section
**Load session**: At start of new conversation, read latest session file

## Agent Instructions

When resuming:
1. Read the latest session file in this folder
2. Continue from the current task
3. Update the session file as you progress
