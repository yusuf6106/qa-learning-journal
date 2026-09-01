# TC-002 — Stock Consumption Integer Validation

## 1. Document Control

| Field | Value |
| :--- | :--- |
| **Test Case ID** | TC-002 |
| **Module** | Warehouse / Inventory Management |
| **Screen** | Manual Stock Consumption (`/inventory/manual-consumption`) |
| **Test Level** | Functional Testing / Negative Testing / Boundary Value Analysis |
| **Severity** | High |
| **Priority** | High / Urgent (P1) |
| **Status** | Fail (Bug Found) |

---

## 2. Test Objective
Verify that the ERP system prevents entering decimal consumption quantities (e.g., `1.5` or `0.75`) for discrete/unit-based stock items (Unit: "Piece / Adet"), enforcing integer-only input validation to maintain inventory integrity.

---

## 3. Business Rules (BR)
* **BR-01:** For items configured with unit type "Piece" (`Unit = PCS / Adet`), the consumption quantity must strictly accept positive whole integers ($X \ge 1$, integer only).
* **BR-02:** Decimal entries for unit-based stock items must be blocked by the frontend/backend and display an explicit validation message: `"Decimal values are not allowed for unit-based items."`
* **BR-03:** Form submission must be disabled until a valid integer is supplied.

---

## 4. Preconditions
1. Tester is logged into the ERP system with authorized warehouse/inventory roles.
2. A discrete test product exists in the system with unit configured as **"Piece" (Adet)** (e.g., `PRD-DISC-001`).
3. Sufficient stock balance is available for the test item.
4. User has navigated to the **Manual Stock Consumption** screen.

---

## 5. Test Execution Steps

| Step | Action | Test Data | Expected Result | Actual Result | Status |
| :---: | :--- | :--- | :--- | :--- | :---: |
| **01** | Open the Manual Stock Consumption screen | N/A | Consumption form loads with all default fields empty and enabled. | Form loaded as expected | Pass |
| **02** | Select the discrete/unit-based test item | `PRD-DISC-001` (Unit: PCS) | Item is selected; unit type "Piece / Adet" is displayed on screen. | Item selected successfully | Pass |
| **03** | Enter a valid integer quantity | `5` | Field accepts value `5`; form permits submission. | Value accepted | Pass |
| **04** | Enter a decimal quantity into the quantity field | `1.5` | Input is either blocked or inline error is triggered: `"Decimal values are not allowed for unit-based items."` | Field accepts `1.5` without warning | **Fail** |
| **05** | Click **Save / Submit** button with decimal value | Quantity = `1.5` | System rejects transaction; displays validation error and blocks stock ledger update. | Transaction is saved; stock reduced by `1.5` | **Fail** |

---

## 6. Bug Summary & Defect Details
* **Defect Title:** System permits decimal stock consumption for discrete (unit-based) items.
* **Impact:** Causes fractional physical stock records in warehouse ledger (e.g., `48.5` units), violating inventory accounting and MRP rules.
* **Suggested Fix:** Implement input mask validation on UI to restrict input to integers for unit-based items, backed by server-side validation rejecting float payloads.

---

## 7. Given–When–Then (BDD Format)

```gherkin
Given the user is on the Manual Stock Consumption screen
  And a stock item with unit type "Piece" is selected
When the user enters a decimal quantity of "1.5" and submits the form
Then the system should reject the transaction
  And display an error message "Decimal values are not allowed for unit-based items."
