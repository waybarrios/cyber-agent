# Report Writer Agent - Tasks

**Status**: Awaiting Red Team Agent Findings
**Last Updated**: Pending first session

---

## Pre-Report Checklist

- [ ] Red Team Agent attack complete
- [ ] `attack_complete.json` available
- [ ] All phase logs available
- [ ] Create report output directory

---

## Report Generation Tasks

### 1. Load Input Data
- [ ] Read `../redteam_agent/sessions/attack_complete.json`
- [ ] Parse all phase logs from `../redteam_agent/output/`
- [ ] Validate all required data is present

### 2. Generate Executive Summary
- [ ] Calculate vulnerability counts by severity
- [ ] Determine overall risk rating
- [ ] Document objectives achieved (flags)
- [ ] List top priority actions

### 3. Document Technical Findings

**VULN-001: IKE Service Exposed**
- [ ] Write description
- [ ] Include evidence from UDP scan
- [ ] Add CVSS score (5.3)
- [ ] Map to CWE-200, MITRE T1595
- [ ] Write remediation steps

**VULN-002: IKE Aggressive Mode**
- [ ] Write description
- [ ] Include evidence from ike-scan
- [ ] Add CVSS score (7.5)
- [ ] Map to CWE-327, MITRE T1110
- [ ] Write remediation steps

**VULN-003: CVE-2025-32463**
- [ ] Write description
- [ ] Include evidence from sudo version check
- [ ] Add CVSS score (8.1)
- [ ] Map to CWE-269, MITRE T1068
- [ ] Write remediation steps

### 4. Write Attack Narrative
- [ ] Phase 1: Reconnaissance narrative
- [ ] Phase 2: Enumeration narrative
- [ ] Phase 3: Credential attack narrative
- [ ] Phase 4: Initial access narrative
- [ ] Phase 5: Privilege escalation narrative
- [ ] Phase 6: Post-exploitation narrative

### 5. Create MITRE ATT&CK Mapping
- [ ] Map all techniques used
- [ ] Create mapping table
- [ ] Add descriptions

### 6. Build Remediation Roadmap
- [ ] Immediate actions (P1)
- [ ] Short-term actions (P2)
- [ ] Medium-term actions (P3)
- [ ] Long-term improvements

### 7. Generate Output Formats
- [ ] Markdown report (.md)
- [ ] HTML report (.html)
- [ ] JSON report (.json)
- [ ] Executive summary (.md)

### 8. Quality Checks
- [ ] All findings have evidence
- [ ] All findings have remediation
- [ ] CVSS scores are correct
- [ ] Narrative is complete
- [ ] No spelling/grammar errors

### 9. Final Output
- [ ] Save all reports to `reports/` directory
- [ ] Confirm file creation
- [ ] Report completion to user

---

## Output Files

```
reports/
├── PTR-{ID}_report.md        # Full Markdown report
├── PTR-{ID}_report.html      # Full HTML report
├── PTR-{ID}_report.json      # Machine-readable JSON
└── PTR-{ID}_executive.md     # Executive summary only
```

---

## Success Criteria

- [ ] All three vulnerabilities documented
- [ ] Attack narrative covers all 6 phases
- [ ] Both flags referenced in report
- [ ] All output formats generated
- [ ] Quality checks passed

---

**Status**: READY - Awaiting Red Team Agent Output
