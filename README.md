# SAP GRC Access Control & SoD Case Study

## About This Project

I built this project to get hands-on experience with how SAP access is reviewed from a security and GRC perspective.

My goal was not just to create roles or run a risk report. I wanted to follow the access lifecycle and understand what happens after a user receives access:

**What access does the user have?  
Does that access create a Segregation of Duties risk?  
What exactly is causing the conflict?  
Should the access be removed, redesigned, or controlled?**

For this case study, I worked with SAP role and authorization configuration and then used SAP GRC Access Control to analyze the access assigned to a test user.

During the analysis, SAP GRC identified an active SoD risk for the user. I investigated the risk, reviewed the conflicting functions and actions, evaluated the access from a security perspective, and reviewed how mitigating controls are structured when conflicting access must be retained.

---

## Case Scenario

The test user used in this project was:

**User:** `BEST1`

The user had SAP access that needed to be reviewed from both an authorization and an SoD perspective.

Before performing the GRC analysis, I worked with the supporting SAP Security configuration, including:

- PFCG role configuration
- Authorization maintenance
- Organizational-level restrictions
- Authorization profile generation
- User-role assignment
- User comparison

After the access was available to the user, I moved to SAP GRC Access Control to analyze the user's effective access.

The purpose was to determine whether individually assigned permissions created a security risk when combined.

---

## What I Worked On

### 1. SAP Role and Authorization Review

I worked with SAP roles in PFCG and reviewed how business access is translated into technical authorizations.

I maintained the required authorization data and organizational restrictions instead of leaving the access unrestricted.

I also generated the authorization profile, assigned the required access to the test user, and completed the user comparison so that the user master reflected the role assignment.

This gave me the access baseline that I later analyzed through SAP GRC.

---

### 2. User-Level Access Risk Analysis

I then performed a **User Level Access Risk Analysis** in SAP GRC Access Control.

I analyzed:

**User:** `BEST1`  
**Rule Set:** `GLOBAL`  
**Analysis:** Access Risk Analysis

The purpose of this step was to check whether the user's combined access violated any configured GRC risk rules.

The analysis identified an active SoD risk.

---

## SoD Finding

The main risk identified during my analysis was:

| Risk Detail | Result |
|---|---|
| Risk ID | `B001` |
| Risk Type | Segregation of Duties |
| Risk Level | Medium |
| Status | Active |
| Conflicting Function | Basis Development |
| Conflicting Function | System Administration |

The result also showed multiple rule and action combinations associated with B001.

This was an important part of the project for me because it showed the difference between simply reviewing a user's roles and actually analyzing the risk created by the user's combined access.

A user may have valid access to individual functions, but the combination of those functions can still create an SoD exposure.

---

## Investigating the Risk

I did not treat the GRC result as an automatic decision to remove access.

I reviewed the B001 finding at the rule and action level to understand what SAP GRC was actually identifying.

My investigation focused on:

- the conflicting functions
- the associated rule IDs
- the actions involved in the conflict
- the user's existing access
- whether the conflicting access was necessary
- the security impact of keeping both capabilities

This is where I approached the result as an access-control case rather than just a GRC report.

The main question became:

> **Does BEST1 really need both sides of this access?**

If the answer is no, the unnecessary access should be removed or redesigned.

If both capabilities are required for a valid business reason, the risk needs additional control and oversight.

---

## Remediation Decision

My first choice for an SoD conflict is remediation whenever the business requirement allows it.

For B001, the remediation approach is:

1. Identify the access contributing to the conflicting capability.
2. Confirm whether that access is required for the user's job.
3. Remove or redesign unnecessary access.
4. Maintain least privilege instead of accepting excessive access.
5. Re-run the GRC analysis after the access change.

I would not remove access only because GRC generated a violation.

The business requirement and the security risk both need to be considered before making the final decision.

---

## Mitigating Control Review

I also reviewed an existing mitigating control in SAP GRC to understand how a compensating control is structured when conflicting access cannot be removed.

The control I reviewed included separate responsibilities for approval and monitoring.

| Responsibility | User ID |
|---|---|
| Mitigation Approver | `G_MIT_APP_01` |
| Mitigation Monitor | `G_MIT_MON_01` |

I also reviewed the control's organization, security process, subprocess, and owner assignments.

This helped me understand an important difference:

**Remediation changes the user's access.**

**Mitigation allows necessary conflicting access to remain, but places additional monitoring and accountability around the risk.**

I would use mitigation only when there is a valid business reason for retaining the conflicting access and an appropriate compensating control exists.

---

## My Access Decision

For this case, I would not automatically approve BEST1 with the identified B001 conflict.

My decision would depend on whether both conflicting capabilities are actually required.

### If both capabilities are not required

I would remediate the access by removing or redesigning the unnecessary authorization and then perform another GRC risk analysis.

### If both capabilities are required

I would require a formally approved mitigating control with:

- a clearly defined control
- an independent approver
- an assigned monitor
- defined monitoring responsibility
- appropriate validity dates
- documented evidence

The risk should be understood and controlled before the conflicting access is accepted.

---

## Project Flow

```text
Business Access Requirement
          |
          v
SAP Role & Authorization Configuration
          |
          v
User Access Assignment
          |
          v
SAP GRC User-Level Risk Analysis
          |
          v
B001 SoD Risk Identified
          |
          v
Rule & Action Investigation
          |
          v
Is All Conflicting Access Required?
        /     \
      No       Yes
      |         |
      v         v
 Remediate   Evaluate
   Access    Mitigation
      \         /
       \       /
          v
   Access Decision
          |
          v
     Evidence
