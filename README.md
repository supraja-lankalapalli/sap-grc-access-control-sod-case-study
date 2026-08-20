# SAP GRC Access Control — Segregation of Duties Case Study

## About This Project

I built this project to work through an SAP access-control case from role design to SoD risk treatment instead of looking at GRC only from the risk-analysis screen.

The case follows a user who requires Accounts Payable access for invoice and payment processing. I created the required access through separate roles, maintained the authorizations, synchronized the access with the user, and then analyzed the user's effective access in SAP GRC.

The GRC analysis identified an active Segregation of Duties risk. From there, I traced the finding to the functions behind the conflict and documented how I would treat the access based on business need and least privilege.

The project covers the full path:

**Access requirement → Role design → Authorization configuration → User provisioning → GRC risk analysis → Risk investigation → Risk treatment**

---

## Case Scenario

The access requirement involved two Accounts Payable activities:

- entering incoming invoices
- processing automatic payments

Instead of combining both activities into one role, I maintained them separately:

| Role | Access |
|---|---|
| `Z_AP_INVOICE_PROCESSOR` | `FB60 — Enter Incoming Invoices` |
| `Z_AP_PAYMENT_PROCESSOR` | `F110 — Parameters for Automatic Payment` |

The roles were then synchronized with the user master and reviewed through SAP GRC at the user level.

This allowed the case to move beyond simply checking whether a transaction was available. The important part was understanding what the user's combined access looked like once GRC evaluated it against the configured rule set.

---

## What I Worked On

### 01 — Access Request

The case started by defining the required AP access and separating invoice processing from payment processing.

The access requirement was translated into role-level access that could be configured and reviewed independently.

[View Access Request](01-access-request/access-request.md)

---

### 02 — Authorization Configuration

The required access was configured through SAP role maintenance.

The authorization structure was reviewed so the role contained the access needed for the assigned responsibility rather than relying only on the transaction appearing in the role menu.

This part of the project connected the business access requirement to the SAP authorization layer behind it.

[View Authorization Configuration](02-authorization-configuration/authorization-configuration.md)

---

### 03 — User Provisioning

After the role configuration was completed, user comparison was performed so the access was reflected in the user master.

The payment-processing access was maintained separately through:

`Z_AP_PAYMENT_PROCESSOR`

with:

`F110 — Parameters for Automatic Payment`

User comparison was completed after the role changes to synchronize the updated access.

At this point the roles were technically configured and available to the user, but that alone did not establish that the combined access was acceptable from an SoD perspective.

[View User Provisioning](03-user-provisioning/user-provisioning.md)

---

### 04 — SoD Risk Analysis

The next step was to analyze the user's effective access in SAP GRC.

The User-Level Access Risk Analysis was run for:

**User:** `BEST1`  
**Rule Set:** `GLOBAL`

The analysis returned:

| Field | Result |
|---|---|
| Risk ID | `B001` |
| Risk Level | Medium |
| Risk Type | Segregation of Duties |
| Business Process | Basis |

The result also contained multiple rule and action combinations under the same risk.

Instead of treating the result as a simple pass/fail check, I reviewed the action-level output to understand what access was contributing to the finding.

[View SoD Risk Analysis](04-sod-risk-analysis/sod-risk-analysis.md)

---

### 05 — Risk Investigation

After identifying B001, I opened the risk definition and traced the finding to the two functions configured in the conflict:

- `BS02 — Basis Development`
- `BS11 — System Administration`

This was an important part of the analysis because the user-level result showed the risk, while the risk definition showed what GRC was actually evaluating underneath it.

The review confirmed that the finding was being triggered by the combination of these capabilities within the same access context.

Rather than treating every action returned by GRC as something that should automatically be removed, I used the function and action-level details to understand where the conflict was coming from and what would need to be addressed.

[View Risk Investigation](05-risk-investigation/risk-investigation.md)

---

### 06 — Risk Treatment

Once the conflict was understood, the final step was deciding how the access should be handled.

Where conflicting access is not required, remediation is the preferred treatment. The unnecessary access should be removed or redesigned, followed by user synchronization and another GRC analysis to validate the result.

Where both capabilities are required for a valid business reason, removing access only to clear the GRC finding would not be the right decision. In that situation, the remaining risk should be handled through an appropriate mitigating control with documented justification, independent approval, monitoring, evidence, and periodic review.

For this case, the treatment logic is:

```text
B001 Confirmed
      ↓
Review Required Access
      ↓
 ┌───────────────┴───────────────┐
 │                               │
Access Not Required       Both Capabilities Required
 │                               │
 ↓                               ↓
Remediate                     Mitigate
 │                               │
 ↓                               ↓
Re-run GRC              Monitor and Review
 └───────────────┬───────────────┘
                 ↓
         Controlled Access