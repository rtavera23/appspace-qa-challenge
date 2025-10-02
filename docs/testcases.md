# Appspace – Registration Form · Test Plan (HU01)

> Test case format **HUxx-TC** with datasets per variant. Save as `docs/HU01-registration-form.md`.

---

## Table of Contents
- [HU01-TC-A — Happy paths (valid)](#hu01-tc-a--happy-paths-valid)
- [HU01-TC-B — Full Name validations](#hu01-tc-b--full-name-validations)
- [HU01-TC-C — Email validations](#hu01-tc-c--email-validations)
- [HU01-TC-D — Password validations](#hu01-tc-d--password-validations)
- [HU01-TC-E — Confirm Password](#hu01-tc-e--confirm-password)
- [HU01-TC-F — Terms & Conditions](#hu01-tc-f--terms--conditions)
- [HU01-TC-G — Sign Up button states](#hu01-tc-g--sign-up-button-states)
- [HU01-TC-H — Messaging & states](#hu01-tc-h--messaging--states)
- [HU01-TC-I — Resubmission & resilience](#hu01-tc-i--resubmission--resilience)
- [HU01-TC-J — Accessibility (quick)](#hu01-tc-j--accessibility-quick)
- [HU01-TC-K — Usability & content](#hu01-tc-k--usability--content)
- [HU01-TC-L — Client-side security](#hu01-tc-l--client-side-security)
- [HU01-TC-M — Performance / network](#hu01-tc-m--performance--network)
- [HU01-TC-N — Responsive / devices](#hu01-tc-n--responsive--devices)
- [HU01-TC-O — Internationalization](#hu01-tc-o--internationalization)
- [HU01-TC-P — Rare / edge cases](#hu01-tc-p--rare--edge-cases)
- [HU01-TC-Q — Regression: success and error at once](#hu01-tc-q--regression-success-and-error-at-once)
- [Execution checklist](#execution-checklist)
- [Template for new cases](#template-for-new-cases)


---

## HU01-TC-A — Happy paths (valid)

**Objective**  
Verify the form accepts correct inputs and registers successfully.

**Preconditions**  
Registration page loaded, clean session, no network blockers.

**Data (dataset)**

| ID | Full Name | Email | Password | Confirm | T&C | Expected |
|---|---|---|---|---|---|---|
| A1 | Juan Pérez | juan.perez@example.com | Qwerty!23456 | Qwerty!23456 | check | Success. |
| A2 | María López García | maria.lg@example.com | StrongPass!2025 | StrongPass!2025 | check | Success. |
| A3 | John Doe | john.doe@example.com | Abcdef12!@ | Abcdef12!@ | check | Success. |
| A4 | Ana Test | ana.test@example.com | Sup3rStr0ng!Pass | Sup3rStr0ng!Pass | check | Success. |
| A5 | Carlos Space | c.space@example.com | Abc 123 !@# | Abc 123 !@# | check | Record whether spaces are accepted/trimmed/rejected. |
| A6 | Eva Clean | eva.clean+2@example.com | Qazxsw12!! | Qazxsw12!! | check | Success after a prior registration; no stale state. |
| A7 | (keyboard only) Juan Pérez | juan.perez2@example.com | Qwerty!23456 | Qwerty!23456 | check | Success using Tab/Enter/Space only. |

**Steps**  
1. Fill fields per dataset row.  
2. For A7, navigate using keyboard only.  
3. Check T&C if applicable.  
4. Click/press **Sign Up**.

**Verify**  
- Success modal appears.  
- “An error has occurred” does not appear.  
- In A6, previous state doesn’t leak.

---

## HU01-TC-B — Full Name validations

**Objective**  
Validate name rules and sanitization.

**Preconditions**  
Have valid Email and Password ready.

**Data (dataset)**

| ID | Full Name | Email | Password / Confirm | T&C | Expected |
|---|---|---|---|---|---|
| B1 | `(empty)` | vacio.fn@example.com | Abcdef12! | check | Block + field-specific error on Full Name. |
| B2 | Pepe | pepe.one@example.com | Abcdef12! | check | Note if single-word names are allowed. |
| B3 | `   Ana   Pérez   ` | ana.trim@example.com | Abcdef12! | check | Trim then validate OK. |
| B4 | João Muñoz | joao@example.com | Abcdef12! | check | Accepts accents/Ñ/ç. |
| B5 | O’Connor / Jean-Luc Picard | oname@example.com | Abcdef12! | check | Accepts apostrophes/hyphens. |
| B6 | `!!!` / `1234` | symbols@example.com | Abcdef12! | check | Should reject. |
| B7 | `A` (1 char) | a1@example.com | Abcdef12! | check | Ideally reject for minimum length. |
| B8 | `A`×300 | a300@example.com | Abcdef12! | check | Ideally reject for maximum length. |
| B9 | `Juan 😂✌🏾` / `𠜎𠜱𠝹` | emoji@example.com | Abcdef12! | check | Reject or sanitize; must not break UI. |
| B10 | `<b>Juan</b>` / `<script>alert(1)</script>` | inj@example.com | Abcdef12! | check | Not rendered/executed; preferably rejected. |
| B11 | `Juan\nPérez` (pasted with newline) | nl@example.com | Abcdef12! | check | Sanitize or reject. |

**Steps**  
1. Fill valid Email and Password.  
2. Try each **Full Name** from the table.  
3. Check T&C and submit.

**Verify**  
- Field-level messages.  
- Sanitization (trim, strip HTML).  
- No injection or visual breakage.

---

## HU01-TC-C — Email validations

**Objective**  
Ensure accepted formats and proper rejection of invalid emails.

**Data (dataset)**

| ID | Email | Full Name | Password / Confirm | T&C | Expected |
|---|---|---|---|---|---|
| C1 | `(empty)` | Valid Name | Abcdef12! | check | Block + Email field error. |
| C2 | a@b.co | John | Abcdef12! | check | Accepted. |
| C3 | john.doe+alias@gmail.com | John | Abcdef12! | check | Accepted. |
| C4 | name@sub.domain.com | John | Abcdef12! | check | Accepted. |
| C5 | john.doe | John | Abcdef12! | check | Reject (no @). |
| C6 | john@localhost | John | Abcdef12! | check | Reject (no TLD). |
| C7 | john@@example.com | John | Abcdef12! | check | Reject (multiple @). |
| C8 | john doe@example.com | John | Abcdef12! | check | Reject (space). |
| C9 | john..doe@gmail.com | John | Abcdef12! | check | Reject (double dot). |
| C10 | john@-domain.com | John | Abcdef12! | check | Reject (invalid domain). |
| C11 | 用户@例子.广告 | John | Abcdef12! | check | Reject (unusual unicode). |
| C12 | john@domain.com. | John | Abcdef12! | check | Reject (trailing dot). |
| C13 | JOHN.DOE@EXAMPLE.COM | John | Abcdef12! | check | Accept/normalize (case-insensitive). |
| C14 | "john..doe"@example.com | John | Abcdef12! | check | System decision (likely reject). |
| C15 | `  john.doe@example.com  ` | John | Abcdef12! | check | Trim → accept. |
| C16 | `<img src=x onerror=alert(1)>@mail.com` | John | Abcdef12! | check | Reject; no execution. |
| C17 | (duplicate) reuse same email | John | Abcdef12! | check | Note whether duplicates are allowed. |

**Steps**  
1. Fill valid Full Name and Password.  
2. Try each **Email** entry.  
3. Check T&C and submit.

**Verify**  
- Field-level error when applicable.  
- Trim applied.  
- No XSS.

---

## HU01-TC-D — Password validations

**Objective**  
Discover the password policy and edge behaviors.

**Data (dataset)**

| ID | Password / Confirm | Expected |
|---|---|---|
| D1 | `(empty)` | Block + error. |
| D2 | `abcd` (4) | Find minimum length (likely reject). |
| D3 | `abcdef` (6) | Same. |
| D4 | `abcdefgh` (8) | Accept or require complexity. |
| D5 | `Abcdefg1` (8 mixed) | Accept if rule is 8 + mix. |
| D6 | `Abcdefg1!2` (10) | Accept. |
| D7 | `Abcdefg1!23` (11–12) | Accept. |
| D8 | `contraseña` (only lowercase / non-ASCII) | Accept or reject (record). |
| D9 | `Aa1!aaaaaa` (full mix) | Accept. |
| D10 | `Camiñö€漢123!` (non-ASCII) | Ideally accept. |
| D11 | `  Abc123!` / `Abc 123 !` / `Abc123!  ` | Note if spaces are trimmed/allowed. |
| D12 | `password`, `12345678`, `qwerty123` | Any weak-password check? Note. |
| D13 | 200–500 chars (`A…A!1`) | Accept without freeze, or show clear length limit. |
| D14 | `"><script>alert(1)</script>` | Not executed; ideally rejected. |

**Steps**  
1. Use valid Full Name and Email.  
2. Try each password (Confirm same except mismatch, covered in E).  
3. Submit and observe.

**Verify**  
- Minimum/strength rules.  
- Handling of spaces and non-ASCII.  
- No leaks in console/DOM.

---

## HU01-TC-E — Confirm Password

**Objective**  
Validate match and live revalidation.

**Data (dataset)**

| ID | Password | Confirm | Expected |
|---|---|---|---|
| E1 | Abcdef12! | `(empty)` | Block + error on Confirm. |
| E2 | Abcdef12! | Abcdef12? | Clear mismatch error + block. |
| E3 | Sup3rStr0ng! | (paste from Password) | Matches → allowed. |
| E4 | (start equal) then change PW to `Sup3rStr0ng!!` | (unchanged) | Revalidates mismatch. |

**Steps**  
1. Fill valid Full Name and Email.  
2. Apply dataset.  
3. Submit.

**Verify**  
- Immediate mismatch validation.  
- Clear messaging.

---

## HU01-TC-F — Terms & Conditions

**Objective**  
Ensure legal compliance and checkbox UX.

**Data (dataset)**

| ID | Setup | Expected |
|---|---|---|
| F1 | Everything valid (A1) but T&C uncheck | Block + legal message. |
| F2 | Toggle check/uncheck multiple times | Final state respected. |
| F3 | Click on “Terms & Conditions” text/link | Opens document in new tab; accessible. |
| F4 | Navigate Back from T&C page | Checkbox state persists. |

**Steps**  
1. Prepare valid data.  
2. Apply each setup.  
3. Submit / navigate as per case.

---

## HU01-TC-G — Sign Up button states

**Objective**  
Availability, loading feedback, and deduped submissions.

**Data (dataset)**

| ID | Case | Expected |
|---|---|---|
| G1 | Empty form → progressively fill | Button disabled until form valid. |
| G2 | Submit A1 | Loading feedback; no multiple submissions. |
| G3 | Double-click / repeated Enter | Single submission only. |
| G4 | Network error → retry | Retry allowed and works. |

**Steps**  
Complete per case; watch button/requests behavior.

---

## HU01-TC-H — Messaging & states

**Objective**  
Consistency between field errors, global banner, and success.

**Data (dataset)**

| ID | Case | Expected |
|---|---|---|
| H1 | FN empty, EM invalid, PW mismatch, T&C uncheck | **Field**-level errors (not just a banner). |
| H2 | Force global error (offline) | Shows **An error has occurred** without success. |
| H3 | EM invalid | No success modal. |
| H4 | Close success modal | Focus returns logically; define clear vs. preserve fields. |
| H5 | Offline and submit | Clear error; never “success”. |

---

## HU01-TC-I — Resubmission & resilience

**Objective**  
Avoid inconsistent state or double submits.

**Data (dataset)**

| ID | Case | Expected |
|---|---|---|
| I1 | Submit and refresh during request | No zombie states. |
| I2 | Navigate back/forward after submit | No duplicate submissions. |
| I3 | Huge inputs (FN 300, PW 200) | UI stable. |
| I4 | 5–10 rapid submissions (EM `test+N@ex.com`) | Correct handling; no freeze. |

---

## HU01-TC-J — Accessibility (quick)

**Objective**  
Confirm basic a11y requirements.

**Data (dataset)**

| ID | Check | Expected |
|---|---|---|
| J1 | Keyboard-only navigation | Smooth; no focus traps. |
| J2 | Labels `for`/`id` and `aria-invalid` | Properly set on errors. |
| J3 | Error/button contrast | Sufficient. |
| J4 | `aria-live` for errors/success | Announced by SR. |
| J5 | Focus moves to first invalid field on submit | Works. |

---

## HU01-TC-K — Usability & content

**Objective**  
Copy, links, and data persistence checks.

**Data (dataset)**

| ID | Case | Expected |
|---|---|---|
| K1 | Labels/placeholders | Clear and consistent. |
| K2 | Legal links | Open correct destination. |
| K3 | Fix one field after error | Other fields persist. |
| K4 | Password autocomplete | Controlled; no unsafe autofill. |

---

## HU01-TC-L — Client-side security

**Objective**  
Visible client protections & no sensitive leaks.

**Data (dataset)**

| ID | Check | Expected |
|---|---|---|
| L1 | Console/Network | No sensitive data. |
| L2 | Reflected XSS via FN/EM | Not executed/injected. |
| L3 | Method/URL | PW not sent via GET or query. |
| L4 | CSRF surface | Tokens/headers don’t expose secrets. |
| L5 | local/sessionStorage | PW not stored. |

---

## HU01-TC-M — Performance / network

**Objective**  
Behavior under slow/failed networks.

**Data (dataset)**

| ID | Case | Expected |
|---|---|---|
| M1 | Slow 3G + submit | Clear loading; reasonable timeout. |
| M2 | Simulated 500 (intercept) | Error message; no success modal. |
| M3 | Cancel request (switch tab) | No delayed success. |

---

## HU01-TC-N — Responsive / devices

**Objective**  
Mobile/zoom usability.

**Data (dataset)**

| ID | Case | Expected |
|---|---|---|
| N1 | 375×812 (mobile) | Form and modal visible; not cropped. |
| N2 | Orientation change | State preserved. |
| N3 | 200% zoom | Usable layout. |

---

## HU01-TC-O — Internationalization

**Objective**  
Basic i18n and non-Western names.

**Data (dataset)**

| ID | Case | Expected |
|---|---|---|
| O1 | Switch language (if available) | Consistent texts. |
| O2 | `山田 太郎` / `Nguyễn Văn A` as Full Name | Accepted without breakage. |

---

## HU01-TC-P — Rare / edge cases

**Objective**  
Uncommon UX/security risks.

**Data (dataset)**

| ID | Case | Data | Expected |
|---|---|---|---|
| P1 | Email = Password | EM = PW = `same@same.com` | Accept or warn (record). |
| P2 | PW ≈ Name | FN `Luis`, PW `Luis123!` | Weakness? Record. |
| P3 | RTLO/control chars | EM `test@\u202Ecom.example` | Reject; UI intact. |
| P4 | NBSP in Confirm | PW `Abc123!`, CPW `Abc123! ` (NBSP) | Detect real mismatch. |
| P5 | Zero-width in Email | `john\u200B.doe@example.com` | Sanitize or reject. |

---

## HU01-TC-Q — Regression: success and error at once

**Objective**  
Ensure success and error never show simultaneously.

**Data (dataset)**

| ID | Case | Data | Expected |
|---|---|---|---|
| Q1 | All empty + T&C uncheck | FN/EM/PW/CPW empty | No success modal; field errors shown. |
| Q2 | Multiple invalids | EM `john.doe`, PW/CPW `abc`, T&C uncheck | No success modal; specific errors. |
| Q3 | Fail → fix → success | First a field failure, then A1 | Never “success + error” at the same time. |

**Steps (Q)**  
1. Execute Q1 and Q2, verify banners/modals.  
2. Correct to valid data (A1) and submit.

---

## Execution checklist

| ID  | Result | Notes |
|-----|--------|-------|
| A1  |   pass   |   N/A    |
| A2  |   pass   |   N/A    |
| A3  |   pass   |   N/A    |
| A4  |   pass   |   N/A    |
| A5  |   pass   |   user is register succesfully with space in the password, since we dont have any AC to know the behavior expected    |
| A6  |   pass   |   User can be register twice, but thats expected cause the form do not save data.   |
| A7  |   Fail   |   When user is over the T&C and press enter the form is submited. without the T&C being clicked  |
| B1  |   Fail   |   The registration is completed even without the name    |
| B2  |   pass   |   N/A    |
| B3  |   pass   |   N/A    |
| B4  |   pass   |   N/A    |
| B5  |   pass   |   N/A    |
| B6  |   Fail   |   Accept special chatacters   |
| B7  |   Fail   |   Ideally reject for minimum length.   |
| B8  |   Fail   |   Should have a maximun lenght    |
| B9  |   pass   |   N/A    |
| B10  |   Fail   |   should reject this type of imput   |
| B11  |   Fail   |   should reject this type of imput    |
| C1  |   pass   |   missing Email field error   |
| C2  |   pass   |   N/A    |
| C3  |   pass   |   N/A    |
| C4  |   pass   |   N/A    |
| C5  |   pass   |   N/A    |
| C6  |   pass   |   N/A    |
| C7  |   pass   |   N/A    |
| C8  |   pass   |   N/A    |
| C9  |   pass   |   N/A    |
| C10  |   pass   |   N/A    |
| C11  |   pass   |   N/A    |
| C12  |   pass   |   N/A    |
| C13  |   pass   |   dont know if was normalize form do not save data    |
| C14  |   pass   |   N/A    |
| C15  |   pass   |   N/A    |
| C16  |   pass   |   N/A    |
| C17  |   pass   |   form do not save data    |
| D1  |   Fail   |   user is register without password   |
| D2  |   pass   |   not minimun lenght required    |
| D3  |   pass   |   not minimun lenght required    |
| D4  |   pass   |   accept without complexity   |
| D5  |   pass   |   N/A    |
| D6  |   pass   |   N/A    |
| D7  |   pass   |   N/A    |
| D8  |   pass   |   accept without complexity   |
| D9  |   pass   |   N/A    |
| D10  |   pass   |   N/A    |
| D11  |   pass   |   N/A    |
| D12  |   pass   |   weak password accepted    |
| D13  |   pass   |   set a lenght limit    |
| D14  |   pass   |   should be rejected this kind of pass    |
| E1  |   Fail   |   user is register without password   |
| E2  |   Fail   |   user is register without matching pass   |
| E3  |   pass   |   user shouldn't allowed to copy and paste passwords    |
| E4  |   Fail   |   user is register without matching pass   |
| F1  |   Fail   |   user is register without T&C   |
| F2  |   Fail   |   N/A   |
| F3  |   Fail   |   missing link to terms and condition page    |
| F4  |   Fail   |   missing page to terms and condition page, therefore unable to confirm this    |
| G1  |   Fail   |   Button is always active   |
| G2  |   pass   |   N/A    |
| G3  |   pass   |   N/A    |
| G4  |   Fail   |   form works with internet, this is normal cause the app is pure JS and do not use internet once load   |
| H1  |   Fail   |   Only email is check, the others fields errors are missing  |
| H2  |   Fail   |   this is normal cause the app is pure JS and do not use internet once load    |
| H3  |   pass   |   N/A    |
| H4  |   pass   |   N/A    |
| H5  |   Fail   |   this is normal cause the app is pure JS and do not use internet once load   |
| I1  |   N/A    |   this is normal cause the app is pure JS and do not use internet once load   |
| I2  |   N/A    |   this is normal cause the app is pure JS and do not use internet once load    |
| I3  |   pass   |   N/A    |
| I4  |   pass   |   N/A    |
| J1  |   pass   |   N/A    |
| J2  |   pass   |   N/A    |
| J3  |   Fail   |   N/A    |
| J4  |   Fail   |   N/A    |
| J5  |   pass   |   N/A    |
| K1  |   pass   |   N/A    |
| K2  |   N/A    |   N/A    |
| K3  |   N/A    |   only the email field has validations the rest dont   |
| K4  |   pass   |   N/A    |
| N1  |   Fail   |   form looks cropped    |
| N2  |   pass   |   N/A    |
| N3  |   pass   |   N/A    |
| P1  |   Fail   |   all the field can be the same and we dont see any warning    |
| P2  |   Fail   |   Weakness password accepted    |
| P3  |   pass   |   N/A    |
| P4  |   Fail   |   no detected    |
| P5  |   pass   |   N/A    |
| Q1  |   pass   |   N/A    |
| Q2  |   Fail   |   Only email field is validated    |
| Q3  |   pass   |   N/A    |
| HU01-TC-L — Client-side security --Ignore this only use JS |
| HU01-TC-M — Performance / network --Ignore this only use JS |
| HU01-TC-O — Internationalization --Ignore this form do not have this feature |



---
---
---

## Template for new cases

```markdown
#### HUxx-TC-yy — {Title}

**Objective**  
{What this case validates}

**Preconditions**  
{Initial state / mocks / flags}

**Data (dataset)**  
| ID | Fields | Expected |
|---|---|---|
|  |  |  |

**Steps**  
1) …  
2) …  
3) …

**Verify**  
- …
- …

**Notes**  
- …
