# TC-003 — Discount Rate Input Validation (EP & BVA)

## 1. Document Control

| Field | Value |
| :--- | :--- |
| **Test Case ID** | TC-003 |
| **Module** | Stock Management / Pricing & Discounts |
| **Test Level** | System / Functional Testing |
| **Test Technique** | Black-Box (Equivalence Partitioning & Boundary Value Analysis) |
| **Author** | Yusuf |
| **Status** | Ready for Execution |
| **Priority** | High (P2) |

---

## 2. Test Objective
Verify that the "Discount Rate" input field strictly enforces business rules by accepting only valid integer values between 0 and 100 (inclusive), rejecting boundary violations, non-integer numbers, negative inputs, and non-numeric characters.

---

## 3. Business Rules (BR)
* **BR-01:** Discount rate must be an integer between 0 and 100.
* **BR-02:** Decimal values (e.g., 10.5) are not permitted.
* **BR-03:** Values less than 0 or greater than 100 must trigger a validation error: `"Please enter a valid integer between 0 and 100."`
* **BR-04:** Empty/null inputs must not allow form submission.

---

## 4. Test Design (Equivalence Partitions & Boundaries)

| Partition ID | Partition Type | Input Range / Type | Expected Behavior | Representative Test Data |
| :--- | :--- | :--- | :--- | :--- |
| **VP-01** | Valid | Integer $0 \le X \le 100$ | Accepted | `20` (Nominal), `0` (Min), `100` (Max) |
| **IP-01** | Invalid | Integer $X < 0$ | Rejected | `-1` |
| **IP-02** | Invalid | Integer $X > 100$ | Rejected | `101` |
| **IP-03** | Invalid | Decimal / Float | Rejected | `15.5` |
| **IP-04** | Invalid | Non-numeric / Special Chars | Rejected | `ABC`, `@#$` |
| **IP-05** | Invalid | Empty / Null | Rejected | `[Empty]` |

---

## 5. Test Execution Steps

### Preconditions
1. User is logged into the ERP system with standard "Sales Representative" or "Inventory Specialist" permissions.
2. The Discount Management screen (`/stock/discounts/new`) is open and ready for data entry.

### Test Matrix

| Step | Action / Input Data | Input Classification | Expected Result | Actual Result | Status |
| :---: | :--- | :--- | :--- | :--- | :---: |
| **01** | Enter `20` and click **Save** | Valid Nominal (VP-01) | Value is accepted; discount is saved successfully. | As expected | Pass |
| **02** | Enter `0` and click **Save** | Valid Min Boundary (VP-01) | Value is accepted (0% discount applied). | As expected | Pass |
| **03** | Enter `100` and click **Save** | Valid Max Boundary (VP-01) | Value is accepted (100% discount applied). | As expected | Pass |
| **04** | Enter `-1` and click **Save** | Invalid Lower Boundary (IP-01) | Validation error displayed: `"Please enter a valid integer between 0 and 100."` | As expected | Pass |
| **05** | Enter `101` and click **Save** | Invalid Upper Boundary (IP-02) | Validation error displayed: `"Please enter a valid integer between 0 and 100."` | As expected | Pass |
| **06** | Enter `15.5` and click **Save** | Invalid Format (IP-03) | System blocks input or prompts: `"Decimal values are not allowed."` | As expected | Pass |
| **07** | Enter `ABC` and click **Save** | Invalid Type (IP-04) | Field rejects non-numeric keystrokes. | As expected | Pass |
| **08** | Leave field empty and click **Save** | Invalid Mandatory (IP-05) | Inline validation prompts: `"This field is required."` | As expected | Pass |

---

## 6. Traceability & Evidence
* **Related Requirement:** REQ-ERP-DISC-2026-v1
* **Evidence:** Sanitized UI mockup referenced under `../assets/tc-003-discount-validation-mockup.png`
