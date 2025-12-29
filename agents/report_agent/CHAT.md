# Report Writer Agent - Chat Interface

## How to Interact With Me

I'm the Report Writer Agent. I generate professional security reports from Red Team Agent findings.

---

## Starting Report Generation

**After Red Team Agent completes:**
```
Generate the security report for the Expressway engagement
```

**I will:**
1. Read attack_complete.json
2. Parse all phase logs
3. Generate all report sections
4. Create MD, HTML, and JSON outputs
5. Save to reports/ directory

---

## Common Queries

### "Generate full report"
I'll create the complete penetration testing report in all formats.

### "Generate executive summary only"
I'll create just the executive summary section.

### "Show vulnerability details for VULN-001"
I'll display the full documentation for that vulnerability.

### "What's the overall risk rating?"
I'll calculate and explain the risk assessment.

### "Create remediation roadmap"
I'll generate the prioritized remediation plan.

### "Map findings to MITRE ATT&CK"
I'll create the technique mapping table.

### "Export as HTML"
I'll generate the styled HTML version.

### "Export as JSON"
I'll generate the OWASP OPTRS compliant JSON.

---

## Expected Responses

### During generation:
```
[Generating Report]
- Reading Red Team Agent findings...
- Creating executive summary...
- Documenting vulnerabilities...
- Writing attack narrative...
- Building remediation roadmap...
- Generating output files...
```

### On completion:
```
[Report Generation Complete]

Files created:
- reports/PTR-{ID}_report.md (Full report)
- reports/PTR-{ID}_report.html (Styled HTML)
- reports/PTR-{ID}_report.json (Machine-readable)
- reports/PTR-{ID}_executive.md (Executive summary)

Summary:
- Vulnerabilities documented: 3
- Risk rating: HIGH
- Flags captured: 2/2
- Recommendations: 12
```

---

## Error Handling

### "Red Team Agent data not found"
- Verify attack_complete.json exists
- Check Red Team Agent completed successfully
- Confirm output directory path

### "Missing phase logs"
- Some phase details may be incomplete
- Report will note missing information
- Can proceed with available data

### "Validation errors"
- Some findings may be missing evidence
- Will highlight issues in report
- User can provide additional details

---

## Report Formats Explained

### Markdown (.md)
Best for: Version control, editing, review
Can be converted to PDF using pandoc

### HTML (.html)
Best for: Presentation, web viewing, printing
Includes professional styling

### JSON (.json)
Best for: Automation, tool integration, SIEM import
Follows OWASP OPTRS schema

---

**I'm ready. Tell me to generate the report after Red Team Agent completes.**
