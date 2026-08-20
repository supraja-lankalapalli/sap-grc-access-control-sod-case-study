# 05 - Risk Investigation

## B001 Risk Detail

After B001 was identified in the user-level analysis, I opened the risk details to understand what was actually creating the conflict.

B001 was configured as a Segregation of Duties risk between:

- `BS02 - Basis Development`
- `BS11 - System Administration`

### Evidence

![B001 Risk Detail](evidence/E10_B001_Risk_Detail.png)

*B001 risk definition showing the conflict between Basis Development and System Administration.*

---

## Reviewing the Conflict

The user-level analysis had already shown multiple rule and action combinations under B001.

Opening the risk definition made the conflict clearer. The issue was the combination of development-related and system-administration capability under the same user access.

At this point, I did not treat every action listed by GRC as something that should automatically be removed.

The access needed to be checked against the user's actual responsibility first.

If one side of the conflict was not required, that access could be removed or redesigned.

If both functions were required, the conflict would still remain and would need a controlled risk-treatment decision.

---

## Access Review

The B001 finding was traced from the user-level result to the two functions defined in the risk:

- `BS02 - Basis Development`
- `BS11 - System Administration`

The review confirmed that the risk was being triggered by the combination of these capabilities within the same access context.

Rather than treating the GRC result as a transaction-removal list, I used the function and action-level details to separate the access that required further review from the access that could remain justified.

This narrowed the treatment to two practical options: remove or redesign unnecessary conflicting access, or retain justified access under an appropriate mitigating control.

B001 was therefore carried forward as a confirmed SoD finding requiring risk treatment before the access could be considered acceptable.
