# Report Writer Agent Skills - Documentation Procedures

## Core Competency: Security Report Generation

I transform raw penetration testing data into professional security reports following OWASP OPTRS, PTES, and industry standards.

**Output Format: Markdown only**

---

## Report Types

I generate TWO reports:

| Report | Audience | Content |
|--------|----------|---------|
| Executive Summary | Management, non-technical | Business impact, risk rating, recommendations |
| Technical Report | Security teams, IT staff | Full details, commands, evidence, remediation |

---

## 1. Reading Red Team Agent Output

### Input Files
```
agents/redteam_agent/sessions/attack_complete.json  # Main findings
agents/redteam_agent/output/*.txt                   # Command outputs
```

### Key Data to Extract
- Target information (IP, hostname)
- Discovered vulnerabilities
- Captured credentials
- User and root flags
- Commands executed
- Evidence/screenshots

---

## 2. Executive Summary Report

**File**: `reports/PTR-{YYYYMMDD}_executive.md`

### Template

```markdown
# Security Assessment - Executive Summary

**Target**: {hostname}
**Date**: {date}
**Classification**: CONFIDENTIAL

---

## Overview

A security assessment was conducted against {target} to identify vulnerabilities
and evaluate the organization's security posture.

## Risk Rating

**Overall Risk: {CRITICAL|HIGH|MEDIUM|LOW}**

{One paragraph explaining what this means for the business}

## Key Findings

| Severity | Count |
|----------|-------|
| Critical | {n} |
| High | {n} |
| Medium | {n} |
| Low | {n} |

## Business Impact

{2-3 paragraphs explaining the business impact in non-technical terms}

- What could an attacker do?
- What data/systems are at risk?
- What is the potential cost?

## Recommendations

1. **{Action 1}** - {Brief explanation}
2. **{Action 2}** - {Brief explanation}
3. **{Action 3}** - {Brief explanation}

## Conclusion

{Summary and next steps}

---

*This report is intended for management review. See Technical Report for details.*
```

### Writing Style for Executive Summary
- NO technical jargon
- Focus on business impact
- Use percentages and risk levels
- Keep it to 1-2 pages
- Actionable recommendations

---

## 3. Technical Report

**File**: `reports/PTR-{YYYYMMDD}_technical.md`

### Template

```markdown
# Penetration Testing Report

**Report ID**: PTR-{YYYYMMDD}
**Target**: {target_ip} ({hostname})
**Date**: {date}
**Tester**: Red Team Agent
**Classification**: CONFIDENTIAL

---

## 1. Executive Summary

{Brief overview of engagement and key findings}

## 2. Methodology

### Scope
- Target: {target_ip}
- Platform: {platform}
- Assessment Type: Full penetration test

### Methodology
PTES (Penetration Testing Execution Standard)

### Phases
1. Reconnaissance
2. Enumeration
3. Credential Attack
4. Initial Access
5. Privilege Escalation
6. Post-Exploitation

### Tools Used
| Tool | Purpose |
|------|---------|
| nmap | Port scanning |
| ike-scan | IKE/IPsec enumeration |
| psk-crack | PSK cracking |
| ssh | Remote access |
| gcc | Exploit compilation |

## 3. Technical Findings

### VULN-001: {Title}

| Attribute | Value |
|-----------|-------|
| **Severity** | {severity} |
| **CVSS Score** | {cvss} |
| **CVE** | {cve or N/A} |
| **CWE** | {cwe} |
| **Affected Asset** | {asset} |

**Description**
{description}

**Evidence**
```
{command output or screenshot}
```

**Impact**
{impact description}

**Remediation**
{remediation steps}

---

## 4. Attack Narrative

### Phase 1: Reconnaissance
{narrative}

```bash
{commands executed}
```

**Findings**: {what was discovered}

### Phase 2: Enumeration
{continue for each phase...}

## 5. Remediation Roadmap

### Immediate (0-7 days)
| Priority | Action | Owner |
|----------|--------|-------|
| P1 | {action} | {owner} |

### Short-term (1-4 weeks)
| Priority | Action | Owner |
|----------|--------|-------|
| P2 | {action} | {owner} |

### Medium-term (1-3 months)
| Priority | Action | Owner |
|----------|--------|-------|
| P3 | {action} | {owner} |

## 6. MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|--------|-----------|-----|
| Reconnaissance | Active Scanning | T1595 |
| Initial Access | Valid Accounts | T1078 |
| Credential Access | Brute Force | T1110 |
| Privilege Escalation | Exploitation for Privilege Escalation | T1068 |

## 7. Appendices

### A. Full Command Log
```bash
{all commands executed}
```

### B. Credentials Discovered
| Username | Password | Access |
|----------|----------|--------|
| {user} | {pass} | {level} |

### C. Flags Captured
| Flag | Value |
|------|-------|
| User | {hash} |
| Root | {hash} |

---

*End of Technical Report*
```

---

## 4. Vulnerability Documentation

### Severity Icons
| Severity | Icon |
|----------|------|
| CRITICAL | 🔴 |
| HIGH | 🟠 |
| MEDIUM | 🟡 |
| LOW | 🔵 |

### Standard Vulnerabilities for Expressway

**VULN-001: IKE Service Exposed**
- Severity: MEDIUM
- CVSS: 5.3
- CWE: CWE-200 (Information Exposure)
- Remediation: Restrict IKE service access via firewall

**VULN-002: IKE Aggressive Mode Enabled**
- Severity: HIGH
- CVSS: 7.5
- CWE: CWE-327 (Weak Cryptography)
- Remediation: Disable Aggressive Mode, use Main Mode only

**VULN-003: Sudo Privilege Escalation (CVE-2025-32463)**
- Severity: CRITICAL
- CVSS: 8.1
- CWE: CWE-269 (Improper Privilege Management)
- Remediation: Upgrade sudo to version 1.9.18 or later

---

## 5. Quality Checklist

Before finalizing reports, verify:

- [ ] All vulnerabilities have evidence
- [ ] All vulnerabilities have remediation steps
- [ ] Executive summary has no technical jargon
- [ ] Technical report has full command logs
- [ ] Both flags are documented
- [ ] MITRE ATT&CK mapping is complete
- [ ] Remediation roadmap has priorities

---

## 6. Output Files

Save to `reports/` directory:
- `PTR-{YYYYMMDD}_executive.md` - Executive Summary
- `PTR-{YYYYMMDD}_technical.md` - Technical Report

---

**Report Writer Agent - Markdown-Only Documentation Procedures**
