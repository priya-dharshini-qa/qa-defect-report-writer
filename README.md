# qa-defect-report-writer

A Claude AI skill that turns raw bug descriptions into structured, copy-paste-ready QA defect reports. Give it a one-liner, a wall of logs, or anything in between — it outputs a complete report formatted for Confluence, Notion, or GitHub.

---

## What it does

QA engineers and developers often have to convert messy, informal bug notes into formal defect reports. This skill automates that translation. It extracts or infers every required field, applies consistent severity and priority criteria, flags assumptions transparently, and outputs clean markdown — ready to paste directly into your issue tracker.

**Key behaviours:**
- Works from minimal input — even a vague one-liner like "login is broken" produces a full report
- Flags inferred or assumed fields with *(inferred)* / *(assumed)* so nothing is silently invented
- Handles multiple bugs in one message by producing a separate report per bug
- Runs a self-review checklist before outputting to catch mis-classified severity, leftover placeholders, or copy-pasted priority
- Offers to adjust any field after presenting the report

---

## How to trigger it

Use any of these phrases when talking to Claude:

| Phrase | Example |
|--------|---------|
| `write a defect report` | "Write a defect report for this issue: clicking Save does nothing" |
| `create a bug report` | "Can you create a bug report from my notes below?" |
| `log this bug` | "Log this bug — the date picker crashes on Safari" |
| `turn this into a defect` | "Turn this into a defect: 500 error on checkout" |
| `document this issue` | "Document this issue for our Jira board" |
| `fill in a defect template` | "Fill in a defect template for what I'm about to describe" |

You can also just paste logs, error output, or rough notes and Claude will detect that a defect report is needed.

---

## Output format

Every report follows this structure:

```markdown
# Defect Report: [Title]

## Summary
## Environment
## Steps to Reproduce
## Expected Result
## Actual Result
## Severity
## Priority
## Additional Notes
```

Output is rendered as a markdown code block, ready to copy into Confluence, Notion, GitHub Issues, or Jira.

---

## Example

**Input:**
> "the export button on the invoices page does nothing when I click it. tried in chrome and firefox. happens for all users not just me."

**Output:**

```markdown
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
```

---

## Severity & Priority reference

| Severity | When to use |
|----------|-------------|
| **Critical** | System crash, data loss, security breach, complete feature failure blocking all users |
| **High** | Major feature broken, no workaround, significant user impact |
| **Medium** | Feature partially broken; a workaround exists |
| **Low** | Cosmetic issue, minor UX problem, rarely-hit edge case |

| Priority | When to use |
|----------|-------------|
| **P1 - Urgent** | Must fix before next release or immediately |
| **P2 - High** | Fix in current sprint |
| **P3 - Medium** | Fix in upcoming sprint |
| **P4 - Low** | Fix when capacity allows |

> Severity and priority are independent. A cosmetic issue on a high-traffic checkout button might be Low severity but P2 priority.

---

## Repo structure

```
qa-defect-report-writer/
├── SKILL.md          # Skill instructions loaded by Claude
├── EVALUATIONS.md    # Three test scenarios with expected-behaviour checklists
└── README.md         # This file
```

---

## Running the evaluations

`EVALUATIONS.md` contains three scenarios that cover the most common input types:

| Evaluation | Tests |
|------------|-------|
| **Eval 1 — Vague one-liner** | Skill produces a complete report from minimal input; no fields left as placeholders |
| **Eval 2 — Multi-bug message** | Skill produces two separate reports; severity differs correctly between bugs |
| **Eval 3 — Detailed input** | Skill uses all provided detail verbatim; no spurious *(assumed)* flags added |

To run an evaluation, paste the input from `EVALUATIONS.md` into Claude with the skill active, then tick each expected behaviour in the checklist. Recommended: test against both Claude Haiku and Claude Sonnet.

---

## Installation

Place `SKILL.md` in the skills directory your Claude environment reads from. For Claude.ai Agent skills, upload it via the Skills configuration panel and use the metadata block at the top of the file as-is.

---

## Contributing

Bug reports and improvements welcome. If you extend the skill (e.g. adding Jira field mappings or a new output format), add a matching evaluation scenario to `EVALUATIONS.md` before opening a PR.

---

## License

MIT
