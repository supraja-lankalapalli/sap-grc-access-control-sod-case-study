# 04 - SoD Risk Analysis

## User-Level Risk Analysis

After the access was configured, I ran User-Level Access Risk Analysis in SAP GRC for:

**User:** `BEST1`  
**Rule Set:** `GLOBAL`

The analysis returned an active SoD risk.

### Evidence

![User-Level SoD Risk Analysis](evidence/E09_User_Level_SoD_Risk_Analysis.png)

*User-level GRC analysis showing the B001 risk identified for BEST1.*

---

## Risk Identified

| Field | Result |
|---|---|
| Risk ID | `B001` |
| Risk Level | Medium |
| Risk Type | Segregation of Duties |
| Business Process | Basis |

The result included multiple rule and action combinations under the same risk.

This confirmed that the issue was not a single transaction. The user's effective access was matching different parts of the B001 rule.

---

## Action-Level Review

The result included actions such as:

- `/SAPDMC/LSMW`
- `CMOD`
- `DMCWB`
- `DMWB`
- `DSA`

Multiple rule IDs were also listed under B001.

I reviewed the action-level result instead of stopping at the risk ID because I needed to understand what access was actually contributing to the conflict.

---

## Result

The user access was technically valid and synchronized, but GRC showed that the combined access still created a security conflict.

B001 became the main finding for the case.

The next step was to open the risk details, review the two conflicting functions, and decide how the access should be treated.