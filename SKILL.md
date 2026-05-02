---
name: qa-defect-report-writer
description: >
  Use this skill whenever a user wants to write, generate, draft, or create a QA defect report,
  bug report, or issue report from a raw description of unexpected behavior. Triggers include:
  "write a defect report", "create a bug report", "log this bug", "turn this into a defect",
  "document this issue", "fill in a defect template", or any time the user describes broken,
  unexpected, or incorrect software behavior and wants it documented. Also trigger when the
  user pastes logs, error output, or a rough description of a bug and asks you to structure it.
  Always use this skill even for simple bugs to ensure consistent, professional output.
---

# QA Defect Report Writer

Given a raw description of unexpected software behavior — plain text, pasted logs, error messages,
or rough notes — produce a complete, well-structured defect report in markdown format suitable for
Confluence, Notion, or GitHub.

---

## Your Job

1. **Extract or infer** all required fields from whatever the user provides, even if vague.
2. **Ask only if critical information is truly missing** and cannot be inferred. Prefer making a reasonable assumption and flagging it with *(assumed)* rather than halting to ask.
3. **Self-review** before outputting — run through the checklist in the Self-Review section below.
4. **Output** the full defect report using the standard template below.
5. **Offer to adjust** any field after presenting the report.

---

## Severity Guide

Use this to assign **Severity** (impact on the system):

| Severity | Criteria |
|----------|----------|
| **Critical** | System crash, data loss, security breach, complete feature failure blocking all users |
| **High** | Major feature broken, no workaround, significant user impact |
| **Medium** | Feature partially broken or degraded; a workaround exists |
| **Low** | Cosmetic issue, minor UX problem, rarely-hit edge case |

## Priority Guide

Use this to assign **Priority** (urgency to fix):

| Priority | Criteria |
|----------|----------|
| **P1 - Urgent** | Must fix before next release or immediately |
| **P2 - High** | Fix in current sprint |
| **P3 - Medium** | Fix in upcoming sprint |
| **P4 - Low** | Fix when capacity allows |

> Severity and priority are related but independent. A cosmetic issue on a high-traffic checkout
> button might be Low severity but P2 priority. Use context to calibrate both independently.

---

## Defect Report Template

Produce this exact markdown structure for every report:

~~~markdown
# Defect Report: [Concise Defect Title]

## Summary
[One to two sentences: what is broken and what is the impact on the user or system.]

## Environment
| Field            | Value |
|------------------|-------|
| Application      | [App or module name] |
| Version / Build  | [Version, commit hash, or "Unknown"] |
| OS / Platform    | [e.g. Windows 11, iOS 17, Ubuntu 22.04] |
| Browser / Client | [e.g. Chrome 124, Mobile App, N/A] |
| Test Environment | [e.g. Staging, Production, Local] |

## Steps to Reproduce
1. [First step — specific and numbered]
2. [Second step]
3. [Continue until defect is triggered]

## Expected Result
[What SHOULD happen according to requirements or reasonable user expectation.]

## Actual Result
[What ACTUALLY happens. Include error messages, log excerpts, or screenshot references if provided.]

## Severity
**[Critical / High / Medium / Low]** — [One-line justification]

## Priority
**[P1 - Urgent / P2 - High / P3 - Medium / P4 - Low]** — [One-line justification]

## Additional Notes
[Extra context: frequency of occurrence, affected user scope, related tickets, suspected root cause,
available workaround, or "None".]
~~~

---

## Worked Example

**Input:** "the export button on the invoices page does nothing when I click it. tried in chrome and firefox. happens for all users not just me."

**Output:**

~~~markdown
# Defect Report: Invoice Export Button Unresponsive on Click

## Summary
Clicking the Export button on the Invoices page produces no action or feedback,
preventing users from downloading invoice data.

## Environment
| Field            | Value |
|------------------|-------|
| Application      | Invoices — Export feature |
| Version / Build  | Not provided |
| OS / Platform    | Unknown *(assumed)* |
| Browser / Client | Chrome and Firefox (confirmed) |
| Test Environment | Unknown *(assumed)* |

## Steps to Reproduce
1. Log in to the application *(inferred)*
2. Navigate to the Invoices page *(inferred)*
3. Click the Export button

## Expected Result
A file download should begin (e.g. CSV or PDF export of invoice data).

## Actual Result
Nothing happens — no download, no error message, no visual feedback of any kind.
Issue reproduces in both Chrome and Firefox, and affects all users.

## Severity
**High** — Core export functionality is completely broken with no known workaround,
affecting all users.

## Priority
**P2 - High** — All users are impacted; should be addressed in the current sprint.

## Additional Notes
- Confirmed across Chrome and Firefox; other browsers not tested.
- Affects all user accounts, not role-specific.
- No error message displayed; check browser console for JS errors and network tab
  for failed requests.
~~~

---

## Self-Review Checklist

Before outputting the report, silently verify:

- [ ] Title is concise and describes the broken behaviour, not the expected behaviour
- [ ] Severity matches the criteria in the Severity Guide (not just gut feel)
- [ ] Priority is set independently of Severity based on urgency, not copy-pasted from it
- [ ] All inferred or assumed fields are flagged with *(inferred)* or *(assumed)*
- [ ] Steps to Reproduce are numbered and specific enough to reproduce without guessing
- [ ] Actual Result includes the exact error message or behaviour observed (if provided)
- [ ] No fields are left as raw placeholder text like "[App or module name]"

If any item fails, correct it before outputting.

---

## Handling Incomplete Input

| Missing info | What to do |
|---|---|
| No environment details | Write `Unknown` and mark *(assumed)* in Additional Notes |
| No steps to reproduce | Reconstruct likely steps from the description; flag each as *(inferred)* |
| Ambiguous severity | Default to **High** and invite the user to correct it |
| No version or build | Write `Not provided` — do not block report generation |
| Multiple bugs in one message | Produce a separate report for each bug |

---

## Output Instructions

- Render the report as a clean markdown code block so it can be copied directly into Confluence, Notion, or GitHub.
- After the report, add exactly one line: *"Let me know if you'd like to adjust severity, priority, steps, or any other field."*
- Do not add commentary between fields. Keep the report clean and copy-paste ready.
