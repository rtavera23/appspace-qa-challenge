# Appspace — Registration Form · Bug Reports (From Manual Test Results)

> Date: 2025-10-02 · Source: HU01 execution checklist you provided  
> Ordering: **High → Medium → Low** (P1 → P4)  
> ID format: `BUG-YYYYMMDD-###` 

---

## Executive Summary (Priority Matrix)

| Priority | Key Issues |
|---|---|
| **P1 (Critical)** | Registration bypasses **T&C** (A7, F1), registers with **empty Full Name** (B1), registers **without Password** or with **mismatched Password/Confirm** (D1, E1, E2, E4), possible **HTML/JS injection** in Full Name (B10). |
| **P2 (High)** | **Sign Up** button enabled while form invalid (G1), missing **per-field validation** except Email (H1, Q2), **insufficient contrast** (J3), **mobile layout cropped** at 375×812 (N1), accepts **symbols/only-digits/extreme name lengths** (B6–B8). |
| **P3–P4 (Medium/Low)** | Accepts **weak passwords** (D12), no explicit **max length** for very long passwords (D13), **newline** allowed in name (B11), **NBSP/whitespace normalization** issue in Confirm (P4), emojis/non-BMP in name (policy decision) (B9). |
| **N/A** | G4, H2, H5, I1, I2 — not applicable if the app is fully client-side post load (no network dependency). |

---

## P1 — Critical Priority

### Bug Report

**ID:** BUG-20251002-001  
**Short Title:** Registration succeeds without accepting Terms & Conditions  
**Severity (S1) / Priority (P1):** S1 / P1  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
User can complete registration **without accepting T&C**, violating consent/legal requirements.

## Steps to Reproduce
1. Fill all fields with valid values.  
2. **Do not** check the T&C checkbox.  
3. Click **Sign Up**.

## Actual Result
Success modal shown; registration “simulation” completes.

## Expected Result
Submission should be **blocked** with a clear message requiring T&C acceptance.

## Evidence
- https://prnt.sc/YY1C345bPwhG
- F1 = Fail. (Also A7 behavior indicates a keyboard path bypass.)

## Scope/Impact
Legal/compliance risk; invalid consent.

## Additional Notes
Also see BUG-20251002-005 for keyboard interaction that triggers submit from T&C focus.

## Labels
`frontend` `form-validation` `legal`


---

### Bug Report

**ID:** BUG-20251002-002  
**Short Title:** Registration succeeds with empty Full Name  
**Severity (S1) / Priority (P1):** S1 / P1  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
Form allows registration **without a name**, impacting data quality and potential contractual needs.

## Steps to Reproduce
1. Leave **Full Name** empty.  
2. Fill Email/Password and check T&C.  
3. Submit.

## Actual Result
Registration succeeds.

## Expected Result
Block submission and show a **field-specific** error for Full Name.

## Evidence
- https://prnt.sc/pxtINeAutUpF

## Scope/Impact
Invalid user records; inability to identify/communicate.

## Labels
`frontend` `form-validation`


---

### Bug Report

**ID:** BUG-20251002-003  
**Short Title:** Registration succeeds without Password / with empty Confirm  
**Severity (S1) / Priority (P1):** S1 / P1  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
System allows registration **with empty Password** or **empty Confirm**, compromising credential integrity.

## Steps to Reproduce
- Case D1: Leave **Password** empty; Confirm empty.  
- Case E1: Password filled; **Confirm** empty.  
- Fill other fields validly; check T&C; submit.

## Actual Result
Registration succeeds.

## Expected Result
Block with clear **Password/Confirm** errors.

## Evidence
- https://prnt.sc/4pT0br139KYa
- D1 = Fail; E1 = Fail.

## Scope/Impact
Critical security risk (invalid credential setup).

## Labels
`frontend` `form-validation` `security`


---

### Bug Report

**ID:** BUG-20251002-004  
**Short Title:** Registration succeeds with mismatched Password & Confirm  
**Severity (S1) / Priority (P1):** S1 / P1  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
Form **accepts non-matching passwords** (Confirm differs; or user changes Password after matching).

## Steps to Reproduce
1. Set **different** Password and Confirm (E2) **or** set both equal then change Password only (E4).  
2. Submit.

## Actual Result
Registration succeeds.

## Expected Result
Block and show “Passwords must match”.

## Evidence
- https://prnt.sc/3LjFxIPQWPhU
- E2 = Fail; E4 = Fail.

## Scope/Impact
Broken credential semantics; user lockouts; security risk.

## Labels
`frontend` `form-validation` `security`


---

### Bug Report

**ID:** BUG-20251002-005  
**Short Title:** Pressing Enter on T&C area submits form without checking it  
**Severity (S1) / Priority (P1):** S1 / P1  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
With focus on the T&C area/text, **Enter** triggers form submission **without** toggling/validating the checkbox.

## Steps to Reproduce
1. Fill valid data; keep T&C **unchecked**.  
2. Focus the T&C label/text.  
3. Press **Enter**.

## Actual Result
Form submits and shows success.

## Expected Result
Enter should **not** submit here; it should toggle the checkbox or keep focus.

## Evidence
- https://prnt.sc/-LMHvf8DZjQY
- A7 = Fail.

## Scope/Impact
Keyboard path bypass of legal consent.

## Labels
`frontend` `form-validation` `accessibility`


---

### Bug Report

**ID:** BUG-20251002-006  
**Short Title:** Full Name accepts HTML/Script (potential XSS)  
**Severity (S1) / Priority (P1):** S1 / P1  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
**HTML/JS strings** (e.g., `<script>`) are accepted in Full Name, posing an XSS risk.

## Steps to Reproduce
1. Full Name: `<script>alert(1)</script>` (or `<b>Juan</b>`).  
2. Fill remaining fields validly; check T&C; submit.

## Actual Result
Value is accepted; potential rendering/storage issue.

## Expected Result
**Sanitize or reject** any HTML/JS input.

## Evidence
- https://prnt.sc/ho23K-w7JnbY
- B10 = Fail.

## Scope/Impact
Security risk (XSS), data corruption.

## Labels
`frontend` `security` `xss`

---

## P2 — High Priority

### Bug Report

**ID:** BUG-20251002-007  
**Short Title:** Sign Up button enabled while form is invalid  
**Severity (S2) / Priority (P2):** S2 / P2  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
**Sign Up** remains enabled even when required fields are invalid/empty.

## Steps to Reproduce
1. Leave required fields invalid/empty.  
2. Observe the button state.

## Actual Result
Button is enabled.

## Expected Result
Button should be **disabled** until the form is valid (or submission must reliably block).

## Evidence
- https://prnt.sc/DDi4mIVuwYDS
- G1 = Fail.

## Scope/Impact
Encourages invalid submissions; confusing UX.

## Labels
`frontend` `form-validation`


---

### Bug Report

**ID:** BUG-20251002-008  
**Short Title:** Full Name allows symbols/only digits and extreme lengths  
**Severity (S2) / Priority (P2):** S2 / P2  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
**Full Name** accepts `!!!`, `1234`, **1 char**, and **>255 chars**.

## Steps to Reproduce
- B6: `!!!` or `1234`  
- B7: `A`  
- B8: `A`×300

## Actual Result
Accepted and registration proceeds.

## Expected Result
Alphabetic name with optional hyphen/apostrophe; reasonable length (e.g., 2–100).

## Evidence
- https://prnt.sc/GYags2XZMy5e
- B6 = Fail; B7 = Fail; B8 = Fail.

## Scope/Impact
Garbage data; reporting/communication issues.

## Labels
`frontend` `form-validation`


---

### Bug Report

**ID:** BUG-20251002-009  
**Short Title:** Missing field-level validation except Email  
**Severity (S2) / Priority (P2):** S2 / P2  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
Only Email appears to validate; **Full Name**, **Password**, and **Confirm** lack blocking field errors.

## Steps to Reproduce
1. Submit with empty name, empty/mismatched passwords, etc.  
2. Observe feedback.

## Actual Result
No specific field errors (or do not block).

## Expected Result
Per-field errors with clear messages; focus to first invalid field.

## Evidence
- https://prnt.sc/ONxaLM-pSIw4
- H1 = Fail; Q2 = Fail.

## Scope/Impact
Invalid data persists; poor UX.

## Labels
`frontend` `form-validation` `ux`


---

### Bug Report

**ID:** BUG-20251002-010  
**Short Title:** Insufficient contrast for text/error messages (WCAG)  
**Severity (S2) / Priority (P2):** S2 / P2  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
Contrast for text and/or error messages does not meet **WCAG AA** requirements.

## Steps to Reproduce
1. Inspect text/error colors.  
2. Check ratio with a contrast checker (target ≥ 4.5:1).

## Actual Result
Insufficient contrast.

## Expected Result
Meet **WCAG AA** for normal text and critical UI elements.

## Evidence
- https://prnt.sc/y2gYAZ_kNEu4
- J3 = Fail.

## Scope/Impact
Impacts users with low vision; accessibility non-compliance.

## Labels
`frontend` `accessibility`


---

### Bug Report

**ID:** BUG-20251002-011  
**Short Title:** Missing live region announcements for errors/success  
**Severity (S2) / Priority (P2):** S2 / P2  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
Messages are not announced by screen readers; missing proper `aria-live`.

## Steps to Reproduce
1. Trigger field/global errors with a screen reader on.  
2. Submit successfully.

## Actual Result
No announcements.

## Expected Result
Use appropriate `aria-live` (polite/assertive) for dynamic messages.

## Evidence
- https://prnt.sc/y2gYAZ_kNEu4
- J4 = Fail.

## Scope/Impact
Blocks SR users from understanding state changes.

## Labels
`frontend` `accessibility`


---

### Bug Report

**ID:** BUG-20251002-012  
**Short Title:** Mobile layout cropped at 375×812 (form/modal)  
**Severity (S2) / Priority (P2):** S2 / P2  
**Environment:** [Registration URL], Chrome DevTools iPhone X (375×812)  
**Build/Date:** 2025-10-02

## Description
On mobile viewport the form/modal is **cropped**; not fully visible.

## Steps to Reproduce
1. Set 375×812 in DevTools.  
2. Open the form and trigger the success modal.

## Actual Result
Content is cut off.

## Expected Result
Responsive layout without cropping; proper scroll handling.

## Evidence
- https://prnt.sc/VV7pgE9Cu3P5
- N1 = Fail.

## Scope/Impact
Poor mobile UX.

## Labels
`frontend` `responsive` `ux`

---

## P3 — Medium Priority

### Bug Report

**ID:** BUG-20251002-013  
**Short Title:** Full Name allows newline characters (pasted)  
**Severity (S3) / Priority (P3):** S3 / P3  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
Pasting names with **newline** (`\n`) is allowed, producing malformed values.

## Steps to Reproduce
1. Paste `Juan\nPérez` into Full Name.  
2. Submit with valid other fields.

## Actual Result
Accepted.

## Expected Result
Sanitize or reject newline characters.

## Evidence
- https://prnt.sc/8h7_cx9BrCV6
- B11 = Fail.

## Scope/Impact
Data consistency issues.

## Labels
`frontend` `form-validation`


---

### Bug Report

**ID:** BUG-20251002-014  
**Short Title:** Weak/common passwords accepted without warning  
**Severity (S3) / Priority (P3):** S3 / P3  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
Passwords like `password`, `12345678`, `qwerty123` are accepted with no warning.

## Steps to Reproduce
1. Use any of the common weak passwords.  
2. Submit.

## Actual Result
Registration succeeds.

## Expected Result
Reject or warn; enforce a basic strength policy.

## Evidence
- https://prnt.sc/aw3qo0qH3pYL
- D12 = “pass” operational, but **security concern**.

## Scope/Impact
Security risk due to weak credentials.

## Labels
`frontend` `security` `password-policy`


---

### Bug Report

**ID:** BUG-20251002-015  
**Short Title:** Very long passwords accepted (no clear max length)  
**Severity (S3) / Priority (P3):** S3 / P3  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
No clear upper limit for password length (200–500 chars) → potential perf/UX issues.

## Steps to Reproduce
1. Enter a 200–500 char password (Confirm same).  
2. Submit.

## Actual Result
Accepted.

## Expected Result
Define a reasonable max (e.g., 128) and provide feedback.

## Evidence
- https://prnt.sc/1p9QNYai9019
- D13 = “pass”; **risk** noted.

## Scope/Impact
Possible freezes; poor UX.

## Labels
`frontend` `security` `performance`

---

## P4 — Low Priority

### Bug Report

**ID:** BUG-20251002-016  
**Short Title:** Confirm Password not normalized (NBSP/whitespace mismatch)  
**Severity (S4) / Priority (P4):** S4 / P4  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
If **Confirm** contains **NBSP** or invisible characters, mismatches may not be detected properly.

## Steps to Reproduce
1. PW: `Abc123!`; CPW: `Abc123!⍽` (NBSP at the end).  
2. Submit.

## Actual Result
Mismatch may not be flagged.

## Expected Result
Normalize whitespace (including NBSP/ZWSP) and validate equality.

## Evidence
- https://prnt.sc/kulXgdMHEea8
- P4 = Fail.

## Scope/Impact
Subtle mismatch errors; support burden.

## Labels
`frontend` `form-validation` `ux`


---

### Bug Report

**ID:** BUG-20251002-017  
**Short Title:** Emojis/non-BMP allowed in Full Name (policy decision)  
**Severity (S4) / Priority (P4):** S4 / P4  
**Environment:** [Registration URL], Chrome 140, macOS 10.15.7  
**Build/Date:** 2025-10-02

## Description
Emojis or non-BMP characters are accepted in name; whether this is allowed depends on business rules.

## Steps to Reproduce
1. Full Name: `Juan ✌🏾😂` or `𠜎𠜱𠝹`.  
2. Submit.

## Actual Result
Accepted.

## Expected Result
Define a clear policy (allow or sanitize/reject).

## Evidence
- https://prnt.sc/WJZd69WJTwIG
- B9 = pass, but potentially inconsistent data.

## Scope/Impact
Data quality; printing/search edge cases.

## Labels
`frontend` `data-quality`

---

## Not Applicable (N/A) Notes

- **G4 — Retry after network error**  
- **H2 — Global error offline**  
- **H5 — Offline submit**  
- **I1 — Refresh during request**  
- **I2 — Back/forward duplication**  

---

## Original Execution Checklist (normalized)

| ID  | Result | Notes |
|-----|--------|-------|
| A1  | pass | N/A |
| A2  | pass | N/A |
| A3  | pass | N/A |
| A4  | pass | N/A |
| A5  | pass | Password with spaces accepted; no AC provided. |
| A6  | pass | Re-registration allowed; expected for stateless demo. |
| A7  | **Fail** | Pressing Enter on T&C submits without checking. |
| B1  | **Fail** | Registration completes without name. |
| B2  | pass | N/A |
| B3  | pass | N/A |
| B4  | pass | N/A |
| B5  | pass | N/A |
| B6  | **Fail** | Special characters accepted as name. |
| B7  | **Fail** | No minimum length enforced. |
| B8  | **Fail** | No maximum length enforced. |
| B9  | pass | N/A |
| B10 | **Fail** | HTML/script not rejected. |
| B11 | **Fail** | Newline input not rejected. |
| C1  | pass | Missing Email field error noted, but overall Email validated. |
| C2–C12 | pass | N/A |
| C13 | pass | Normalization unclear (form stateless). |
| C14–C16 | pass | N/A |
| C17 | pass | No duplicate check (stateless). |
| D1  | **Fail** | Register without password. |
| D2–D4 | pass | No minimum/complexity required. |
| D5–D7 | pass | N/A |
| D8  | pass | Non-ASCII accepted; no complexity required. |
| D9–D11 | pass | N/A |
| D12 | pass | Weak password accepted (security concern). |
| D13 | pass | Should enforce a length limit. |
| D14 | pass | Should be rejected; not enforced. |
| E1  | **Fail** | Register without Confirm. |
| E2  | **Fail** | Register with mismatched passwords. |
| E3  | pass | Copy/paste allowed; not a defect by itself. |
| E4  | **Fail** | Changing PW after Confirm still registers. |
| F1  | **Fail** | Register without T&C. |
| F2  | **Fail** | Toggle behavior inconsistent. |
| F3  | **Fail** | Missing T&C link. |
| F4  | **Fail** | No T&C page; cannot confirm persistence. |
| G1  | **Fail** | Button always enabled. |
| G2–G3 | pass | N/A |
| G4  | N/A | Pure JS; no runtime network. |
| H1  | **Fail** | Only Email validated; missing others. |
| H2  | N/A | Pure JS; no network. |
| H3–H4 | pass | N/A |
| H5  | N/A | Pure JS; no network. |
| I1–I2 | N/A | Pure JS; no in-flight network. |
| I3–I4 | pass | N/A |
| J1–J2 | pass | N/A |
| J3  | **Fail** | Insufficient contrast. |
| J4  | **Fail** | No aria-live announcements. |
| J5  | pass | N/A |
| K1  | pass | N/A |
| K2  | N/A | No legal links beyond T&C (missing). |
| K3  | N/A | Only Email has validation. |
| K4  | pass | N/A |
| N1  | **Fail** | Layout cropped on mobile. |
| N2–N3 | pass | N/A |
| P1  | **Fail** | Email = Password accepted without warning. |
| P2  | **Fail** | Weak password accepted. |
| P3  | pass | N/A |
| P4  | **Fail** | NBSP mismatch not detected. |
| P5  | pass | N/A |
| Q1  | pass | N/A |
| Q2  | **Fail** | Only Email validated. |
| Q3  | pass | N/A |
| L/M/O | N/A | (client-only / feature not present). |

---
