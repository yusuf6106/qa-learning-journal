# Manual Consumption Module

## Observation Date

06.07.2026

---

## Observation

A new page named "Manual Consumption" was added under the Warehouse module.

---

## Initial Test Ideas

- Verify stock deduction.
- Verify boundary values.
- Verify decimal input.
- Verify negative values.
- Verify maximum stock amount.
- Verify input validation.
- Verify authorization.

---

## Findings

- Decimal values are accepted.
- Dot (.) is not accepted.
- Comma (,) is accepted.
- Negative values are blocked.
- Plus sign is blocked.
- Maximum value cannot exceed current stock.

---

## Potential Business Rule

Products tracked as integer quantities should probably reject decimal values.

This should be confirmed with business requirements.
