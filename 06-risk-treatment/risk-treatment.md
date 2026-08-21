# 06 - Risk Treatment

## Risk Treatment

The investigation confirmed `B001` as a Medium-level SoD conflict for `BEST1` between:

- `BS02 - Basis Development`
- `BS11 - System Administration`

Based on the risk details, I would first remediate any access that is not required for the user's responsibilities.

There is no reason to keep unnecessary conflicting access and place a mitigating control around it when the conflict can be removed through better access design.

---

## Remediation

If one side of B001 is not required, the affected role or authorization should be removed or redesigned while keeping the access BEST1 still needs.

After the change, I would complete user comparison where required and run User-Level Access Risk Analysis again.

The remediation would be considered complete only after confirming that:

- the required business access still works
- the unnecessary conflicting capability has been removed
- the updated access is reflected for the user
- B001 is removed or reduced in the follow-up GRC analysis

This keeps the remediation focused on the actual access problem rather than making changes only to clear a GRC result.

---

## Mitigation

If BEST1 has a valid requirement for both Basis Development and System Administration access, removing one side of the conflict may not be practical.

In that case, I would retain the required access and manage B001 through an appropriate mitigating control.

The control should include:

- documented justification for retaining the conflicting access
- a clearly defined control activity
- an independent approver
- an assigned monitor
- evidence of the review activity
- follow-up for identified exceptions
- periodic review of the user's continued access requirement

The underlying SoD conflict would still exist. Mitigation would provide the oversight required to manage the risk while the conflicting access remains.

---

## Final Decision

For `BEST1`, remediation is the preferred treatment where conflicting access is not required.

If both capabilities are justified by the user's responsibilities, the access should remain only with documented approval, an appropriate mitigating control, independent monitoring, and periodic review.

```text
B001 - Basis Development + System Administration
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

The treatment decision remains tied to the user's actual access requirement. Unnecessary access should be removed; justified conflicting access should remain visible, owned, and monitored.

No completed remediation or mitigation is claimed in this case because the captured evidence supports the analysis and treatment decision, not a post-treatment implementation.