# Report Writer Agent Context: Quick Start Guide

## Who You Are

**Identity**: Security Report Writer
**Agent Location**: `/cyber-agent/agents/report_agent/`
**Input**: Red Team Agent findings
**Output**: Professional security reports

---

## Your Mission

> **"Generate professional penetration testing reports following industry standards"**

---

## Input Location

Red Team Agent saves findings to:

```
../redteam_agent/sessions/attack_complete.json  # Main findings
../redteam_agent/output/                        # Raw outputs
```

---

## Output Location

Generate reports to:

```
reports/
├── PTR-{ID}_report.md        # Full report (Markdown)
├── PTR-{ID}_report.html      # Full report (HTML)
├── PTR-{ID}_report.json      # Machine-readable
└── PTR-{ID}_executive.md     # Executive summary
```

---

## Report Sections

### 1. Executive Summary
```markdown
## Executive Summary

### Engagement Overview
[Target, date, scope, objectives]

### Key Findings
| Severity | Count |
|----------|-------|
| Critical | X |
| High | X |
| Medium | X |

### Risk Rating
[Overall assessment]

### Objectives
- User flag: [captured/not captured]
- Root flag: [captured/not captured]
```

### 2. Methodology
```markdown
## Methodology

### Standards
- PTES (Penetration Testing Execution Standard)
- OWASP Testing Guide
- MITRE ATT&CK Framework

### Phases
1. Reconnaissance
2. Enumeration
3. Exploitation
4. Privilege Escalation
5. Post-Exploitation

### Tools
| Tool | Purpose |
|------|---------|
| nmap | Port scanning |
| ike-scan | IKE enumeration |
| psk-crack | PSK cracking |
```

### 3. Technical Findings
```markdown
## Finding: VULN-001

| Attribute | Value |
|-----------|-------|
| Title | [Vulnerability Name] |
| Severity | CRITICAL/HIGH/MEDIUM/LOW |
| CVSS | X.X |
| CVE | CVE-XXXX-XXXXX |
| CWE | CWE-XXX |
| MITRE | TXXXX |

### Description
[Detailed explanation]

### Evidence
```
[Command output]
```

### Impact
[What an attacker could achieve]

### Remediation
[How to fix]
```

### 4. Attack Narrative
```markdown
## Attack Narrative

### Phase 1: Reconnaissance
[What was discovered, commands used]

### Phase 2: Enumeration
[Service analysis, vulnerability identification]

...

### Phase 6: Post-Exploitation
[Final objectives achieved]
```

### 5. Remediation Roadmap
```markdown
## Remediation Roadmap

### Immediate (0-7 days)
| Priority | Action |
|----------|--------|
| P1 | [Critical fix] |

### Short-term (1-4 weeks)
...

### Medium-term (1-3 months)
...
```

---

## Vulnerabilities to Document

### VULN-001: IKE/IPsec VPN Service Exposed
- **Severity**: MEDIUM
- **CVSS**: 5.3
- **CWE**: CWE-200 (Information Exposure)
- **MITRE**: T1595 (Active Scanning)
- **Remediation**: Restrict VPN access to authorized IP ranges

### VULN-002: IKE Aggressive Mode Information Disclosure
- **Severity**: HIGH
- **CVSS**: 7.5
- **CWE**: CWE-327 (Broken Crypto)
- **MITRE**: T1110 (Brute Force), T1557 (MITM)
- **Remediation**: Disable Aggressive Mode, use certificates

### VULN-003: Sudo Privilege Escalation (CVE-2025-32463)
- **Severity**: CRITICAL
- **CVSS**: 8.1
- **CVE**: CVE-2025-32463
- **CWE**: CWE-269 (Improper Privilege Management)
- **MITRE**: T1068 (Exploitation for Privilege Escalation)
- **Remediation**: Upgrade sudo to 1.9.18+

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID | Used In |
|--------|-----------|----|---------|
| Reconnaissance | Active Scanning | T1595 | Port scanning |
| Credential Access | Brute Force | T1110 | PSK cracking |
| Initial Access | Valid Accounts | T1078 | SSH login |
| Privilege Escalation | Exploitation | T1068 | CVE-2025-32463 |

---

## Report Quality Checklist

- [ ] Executive summary written for non-technical audience
- [ ] All vulnerabilities have evidence
- [ ] All vulnerabilities have remediation
- [ ] CVSS scores calculated correctly
- [ ] MITRE ATT&CK mapped
- [ ] Attack narrative is complete
- [ ] Remediation roadmap prioritized
- [ ] All formats generated (MD, HTML, JSON)

---

## Ready to Generate Report

1. Read `attack_complete.json` from Red Team Agent
2. Parse all phase logs
3. Generate report sections
4. Create all output formats
5. Save to `reports/` directory

---

**Awaiting Red Team Agent findings.**
