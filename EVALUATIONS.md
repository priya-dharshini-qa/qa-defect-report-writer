# Evaluations — qa-defect-report-writer

Three scenarios covering the most common input types. Run each against the skill and verify
all expected behaviours are present in Claude's output.

---

## Evaluation 1: Vague one-liner (minimum viable input)

**Purpose:** Verify the skill produces a complete, usable report even with almost no detail.

**Input to Claude:**
> "login is broken"

**Expected behaviours:**
- [ ] Produces a full report — does not halt to ask clarifying questions
- [ ] Title describes the broken behaviour ("Login Fails" or similar), not a generic placeholder
- [ ] All unknown/missing fields are marked `Unknown` or `Not provided`, not left as raw placeholders
- [ ] Steps to Reproduce are reconstructed and each step flagged *(inferred)*
- [ ] Environment fields are marked *(assumed)* in Additional Notes
- [ ] Severity defaults to **High** (ambiguous case) and the user is invited to correct it
- [ ] Ends with exactly: *"Let me know if you'd like to adjust severity, priority, steps, or any other field."*

---

## Evaluation 2: Multi-bug report (two distinct issues in one message)

**Purpose:** Verify the skill produces separate reports when the input describes more than one bug.

**Input to Claude:**
> "Two problems I found today: first, the date picker on the checkout page won't open in Safari —
> clicking it does nothing. Second, after a successful order the confirmation email sometimes
> arrives with the wrong order total, showing $0.00 instead of the real amount."

**Expected behaviours:**
- [ ] Produces **two separate** defect reports, not one combined report
- [ ] Report 1 covers the Safari date picker issue
  - [ ] Browser field includes "Safari"
  - [ ] Severity is Medium or lower (workaround: use another browser)
- [ ] Report 2 covers the confirmation email wrong total issue
  - [ ] "Sometimes" is reflected — frequency noted as intermittent in Additional Notes
  - [ ] Severity is High or Critical (financial data incorrect)
  - [ ] Actual Result mentions "$0.00 instead of the real amount"
- [ ] Both reports are clean markdown code blocks
- [ ] Ends with the standard one-line offer to adjust

---

## Evaluation 3: Detailed, well-specified input (high-fidelity output)

**Purpose:** Verify the skill faithfully uses all provided detail and does not over-infer or override supplied information.

**Input to Claude:**
> "Environment: staging, Chrome 124 on Windows 11, build v3.2.1.
> Steps: 1. Log in as an admin. 2. Go to Settings > Users. 3. Click 'Export CSV'. 4. A spinner
> appears for 2 seconds, then a 500 Internal Server Error toast appears.
> Expected: CSV file download starts. Actual: 500 error toast.
> This has happened 100% of the time since yesterday's deployment. No workaround exists.
> Related ticket: PROJ-441."

**Expected behaviours:**
- [ ] Title is specific (references Export CSV or Settings > Users)
- [ ] Environment table is fully populated from the provided details — no *(assumed)* flags needed
  - [ ] Application: Settings — Users (or equivalent)
  - [ ] Version / Build: v3.2.1
  - [ ] OS / Platform: Windows 11
  - [ ] Browser / Client: Chrome 124
  - [ ] Test Environment: Staging
- [ ] Steps to Reproduce match the user's numbered steps exactly — none flagged *(inferred)*
- [ ] Actual Result mentions "500 Internal Server Error" toast
- [ ] Severity is **High** or **Critical** (100% repro rate, no workaround, since deployment)
- [ ] Priority is **P1** or **P2** (regression from recent deployment)
- [ ] Additional Notes includes PROJ-441 and notes "100% reproduction rate since yesterday's deployment"
- [ ] No placeholder text remains in any field
- [ ] Ends with the standard one-line offer to adjust
