# SAP GRC Access Control — SoD Risk Analysis Case Study

## About This Project

I built this project to work through two connected areas of SAP access security: controlled role design in SAP and user-level Segregation of Duties analysis in SAP GRC Access Control.

The first part focuses on an Accounts Payable access requirement. I created separate roles for invoice processing and payment processing, maintained the authorization scope, generated the authorization profile, and completed user comparison.

The second part moves to the effective-access review for `BEST1` in SAP GRC. User-Level Access Risk Analysis identified the active `B001` SoD risk. I then expanded the result to review the rule, function, action, and Role/Profile details behind the finding and opened the B001 risk definition to confirm the conflicting functions.

The AP role configuration and the B001 finding are kept as separate parts of the analysis. The available evidence does not establish that the AP roles themselves caused B001, so I did not assume a relationship that was not demonstrated by the system output.

The project follows this flow:

**Access Requirement → Role Design → Authorization Configuration → User Provisioning → GRC Risk Analysis → Risk Investigation → Risk Treatment**

---

## Case Overview

### SAP Role Configuration

The access requirement included two Accounts Payable activities:

- entering incoming invoices
- processing automatic payments

I maintained these responsibilities through separate roles:

| Role | Business Activity | Transaction |
|---|---|---|
| `Z_AP_INVOICE_PROCESSOR` | Invoice Processing | `FB60 — Enter Incoming Invoices` |
| `Z_AP_PAYMENT_PROCESSOR` | Payment Processing | `F110 — Parameters for Automatic Payment` |

The invoice-processing role was restricted to Company Code `1710`, the authorization data was reviewed, and the authorization profile was generated before user synchronization.

Payment processing was maintained separately instead of adding both responsibilities to the same role.

### SAP GRC Analysis

After completing the SAP role work, I moved to SAP GRC to review `BEST1` at the user level.

The analysis identified:

| Field | Result |
|---|---|
| User | `BEST1` |
| Risk ID | `B001` |
| Risk Level | Medium |
| Risk Type | Segregation of Duties |
| Business Process | Basis |
| Function 1 | `BS02 — Basis Development` |
| Function 2 | `BS11 — System Administration` |

The detailed technical result showed multiple rule and action combinations associated with B001. I used this output to trace the finding beyond the summary risk count and then reviewed the B001 definition to confirm the two functions configured in the conflict.

---

# Project Walkthrough

## 01 — Access Request & Role Design

The case started with the invoice-processing requirement.

I created:

`Z_AP_INVOICE_PROCESSOR`

and maintained:

`FB60 — Enter Incoming Invoices`

The role was kept focused on invoice-processing access instead of combining unrelated activities into the same role.

**Evidence captured:**

- AP invoice-processing role definition
- FB60 maintained in the PFCG role menu

[View Access Request & Role Design](01-access-request/access-request.md)

---

## 02 — Authorization Configuration

After defining the role, I moved into the authorization configuration behind it.

Company Code was restricted to:

`1710`

I reviewed the authorization data and field values generated for the role and then generated the authorization profile.

This ensured that the role was not defined only by the transaction in the menu; the authorization scope behind the transaction was also reviewed before moving to the user level.

**Evidence captured:**

- Company Code 1710 restriction
- authorization details behind the role
- generated authorization profile

[View Authorization Configuration](02-authorization-configuration/authorization-configuration.md)

---

## 03 — User Provisioning & Payment Access

After completing the invoice-processing role, I ran user comparison so the role assignment was synchronized with the user master.

Payment-processing access was maintained separately through:

`Z_AP_PAYMENT_PROCESSOR`

with:

`F110 — Parameters for Automatic Payment`

User comparison was completed again after maintaining the payment role.

At this point, the SAP role configuration and user synchronization were complete.

I then moved to SAP GRC to review `BEST1` at the user level and identify active SoD exposure in the user's effective access.

**Evidence captured:**

- successful invoice-role user comparison
- separate F110 payment-processing role
- successful payment-role user comparison

[View User Provisioning](03-user-provisioning/user-provisioning.md)

---

## 04 — SoD Risk Analysis

User-Level Access Risk Analysis was run in SAP GRC for:

**User:** `BEST1`  
**Rule Set:** `GLOBAL`

The analysis identified `B001` as an active Medium-level Segregation of Duties risk.

I expanded the result in the detailed technical view instead of stopping at the summary finding.

The detailed result connected `BEST1` to B001 and showed the associated:

- rule combinations
- conflicting functions
- individual actions
- Role/Profile values

Actions visible in the analysis included:

- `/SAPDMC/LSMW`
- `CMOD`
- `DMCWB`
- `DMWB`
- `DSA`

This provided the technical trace needed to understand how BEST1's effective access was being evaluated against the B001 rule.

The B001 result is treated as a finding in BEST1's effective access. I did not assume that it was caused by the AP roles configured earlier in the project because the available system evidence does not establish that relationship.

**Evidence captured:**

- BEST1 user-level B001 finding
- detailed B001 rule/function/action/Role-Profile trace

[View SoD Risk Analysis](04-sod-risk-analysis/sod-risk-analysis.md)

---

## 05 — Risk Investigation

After identifying B001, I opened the risk definition to confirm what the rule represented.

The risk was configured between:

`BS02 — Basis Development`

and

`BS11 — System Administration`

The detailed user-level result and the risk definition were reviewed together.

The user-level analysis showed the technical access contributing to the finding, while the risk definition confirmed the functions configured on both sides of B001.

I did not treat the GRC output as a list of transactions that should automatically be removed. The access still needs to be evaluated against the user's responsibilities before deciding whether it should be removed, redesigned, or retained under additional control.

B001 was therefore carried forward as a confirmed SoD finding requiring a risk-treatment decision.

**Evidence captured:**

- B001 risk definition
- BS02 Basis Development
- BS11 System Administration
- supporting technical trace from the user-level analysis

[View Risk Investigation](05-risk-investigation/risk-investigation.md)

---

## 06 — Risk Treatment

The treatment decision depends on whether the conflicting access is actually required.

### Remediation

If one side of B001 is not required, the preferred treatment is to remove or redesign the unnecessary access.

After the change, user comparison should be completed where required and User-Level Access Risk Analysis should be run again to confirm that the access change produced the expected result.

### Mitigation

If both Basis Development and System Administration capabilities are required for a valid reason, removing one side only to clear the GRC finding would not be the right treatment.

The remaining conflict should instead be governed through an appropriate mitigating control with:

- documented business justification
- defined control activity
- independent approval
- assigned monitoring responsibility
- retained review evidence
- exception follow-up
- periodic access review

Mitigation does not remove the underlying SoD conflict. It provides oversight around conflicting access that has been deliberately retained.

### Treatment Logic

```text
B001 — Basis Development + System Administration
                     ↓
          Review required access
                     ↓
        ┌────────────┴────────────┐
        │                         │
 Unnecessary access        Both required
        │                         │
        ↓                         ↓
 Remove / redesign        Apply mitigation
        │                         │
        ↓                         ↓
 User comparison          Approve & monitor
        │                         │
        ↓                         ↓
 Re-run GRC analysis      Periodic review
        └────────────┬────────────┘
                     ↓
             Controlled Access
```

No completed remediation or mitigation is claimed in this case because the captured evidence supports the analysis and treatment decision, not a post-treatment implementation.

[View Risk Treatment](06-risk-treatment/risk-treatment.md)

---

## Evidence Map

| Evidence | What It Supports |
|---|---|
| `E01` | AP invoice-processing role definition |
| `E02` | FB60 maintained in the invoice-processing role |
| `E03` | Company Code 1710 organizational restriction |
| `E04` | Authorization details behind the role |
| `E05` | Generated authorization profile |
| `E06` | Invoice-role user comparison |
| `E07` | Separate F110 payment-processing role |
| `E08` | Payment-role user comparison |
| `E09` | BEST1 user-level B001 SoD finding |
| `E10` | B001 definition and conflicting functions |
| `E11` | BEST1 B001 rule/function/action/Role-Profile technical trace |

---

## Repository Structure

```text
sap-grc-access-control-sod-case-study/
│
├── README.md
│
├── 01-access-request/
│   ├── access-request.md
│   └── evidence/
│
├── 02-authorization-configuration/
│   ├── authorization-configuration.md
│   └── evidence/
│
├── 03-user-provisioning/
│   ├── user-provisioning.md
│   └── evidence/
│
├── 04-sod-risk-analysis/
│   ├── sod-risk-analysis.md
│   └── evidence/
│
├── 05-risk-investigation/
│   ├── risk-investigation.md
│   └── evidence/
│
└── 06-risk-treatment/
    └── risk-treatment.md
```

---

## Technical Areas Covered

- SAP PFCG role design
- transaction-based role configuration
- authorization maintenance
- organizational-level restrictions
- authorization profile generation
- user comparison
- SAP GRC Access Control
- User-Level Access Risk Analysis
- Segregation of Duties analysis
- rule and action-level investigation
- function-level risk analysis
- Role/Profile trace review
- least-privilege access review
- remediation vs. mitigation
- risk-treatment decision making

---

## Final Result

The SAP role configuration established controlled business access and completed the user-side synchronization required for the case.

The GRC portion then identified B001 in BEST1's effective access and traced the finding from the user-level result to the underlying rules, actions, Role/Profile information, and conflicting functions.

The final treatment is based on the access requirement rather than simply trying to remove the GRC finding:

**remove access that is not required, and control justified conflicting access that must remain.**

That keeps the access decision tied to least privilege, technical evidence, and ongoing risk ownership.