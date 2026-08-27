# Appendix I: Definition of Done — Scoring Framework for Governance Artifacts

## Purpose

This appendix provides a practical scoring framework for determining whether governance artifacts are "done" — meaning they are structurally complete, enforceable, and ready for production use. It addresses a common challenge: teams find it easier to evaluate governance intuitively than to implement consistent, defensible scoring.

The solution is a phased approach: start with human-scored ratings, progress to automation, and ultimately reach policy-as-code enforcement.

## The Six Scoring Dimensions

Every governance artifact is scored across six dimensions on a 1-5 scale:

### 1. Structural Completeness

*Does the governance structure form a complete loop?*

| Score | Criteria |
|---|---|
| 1 | No governance structure defined. Roles, decision rights, and accountability undocumented. |
| 2 | Partial structure exists but incomplete or inconsistent. |
| 3 | Complete structure documented. Roles, decision rights, and accountability defined. |
| 4 | Structure documented AND enforced. Evidence of compliance. |
| 5 | Structure documented, enforced, AND continuously improved. Evidence of optimization. |

### 2. Verifiable Accountability

*Can you trace every governance outcome to a named owner?*

| Score | Criteria |
|---|---|
| 1 | No accountability mapping. No named owners or escalation paths. |
| 2 | Some accountability defined but incomplete. Missing escalation paths or workflows. |
| 3 | Full accountability mapping. Named owners, escalation paths, approval workflows documented. |
| 4 | Accountability enforced. Evidence of compliance with the mapping. |
| 5 | Accountability continuously improved. Evidence of optimization. |

**Evidence required:** Named owner for each outcome, clear escalation paths, documented approval workflows, role-based access controls.

### 3. Machine-Readable Structure

*Can automated systems parse and enforce this artifact?*

| Score | Criteria |
|---|---|
| 1 | Not machine-readable. Only human-readable format exists. |
| 2 | Some machine-readable structure but inconsistent or incomplete. |
| 3 | Full machine-readable structure (JSON, YAML, schema-validated markdown). |
| 4 | Machine-readable structure validated and versioned. |
| 5 | Machine-readable structure continuously improved with schema evolution. |

### 4. Traceability to Source

*Can you trace this artifact's provenance — who created it, when, why, and what changed?*

| Score | Criteria |
|---|---|
| 1 | No traceability. No unique identifier or provenance. |
| 2 | Some traceability but incomplete or inconsistent. |
| 3 | Full traceability. Unique ID, timestamps, author attribution, change history documented. |
| 4 | Traceability enforced. Evidence of provenance tracking. |
| 5 | Traceability continuously improved with automated provenance. |

### 5. Sealed Against Entropy

*Is this artifact protected from unauthorized modification, decay, or loss?*

| Score | Criteria |
|---|---|
| 1 | No sealing. No integrity verification or access controls. |
| 2 | Some sealing but incomplete or inconsistent. |
| 3 | Full sealing. Integrity verification, access controls, and review cadence defined. |
| 4 | Sealing enforced. Evidence of immutable logging. |
| 5 | Sealing continuously improved with automated verification. |

### 6. Maturity Alignment

*Does this artifact reflect the organization's current governance maturity level?*

| Score | Criteria |
|---|---|
| 1 | No maturity alignment. Ad-hoc, undefined. |
| 2 | Reactive. Governance exists only in response to incidents. |
| 3 | Defined. Governance is structured, documented, and repeatable. |
| 4 | Managed. Governance is measured, enforced, and tracked. |
| 5 | Optimized. Governance is self-improving and preventive. |

## Composite Score Calculation

The composite score (G_done) is a weighted average:

```
G_done = (w1×S1 + w2×S2 + w3×S3 + w4×S4 + w5×S5 + w6×S6) / 5.0
```

**Weights:**

| Dimension | Weight |
|---|---|
| Structural Completeness | 0.20 |
| Verifiable Accountability | 0.20 |
| Machine-Readable Structure | 0.15 |
| Traceability to Source | 0.15 |
| Sealed Against Entropy | 0.15 |
| Maturity Alignment | 0.15 |

**Threshold:** G_done ≥ 0.80 = Governance-Complete

**Additional rule:** No individual dimension may score below 3.

### Worked Example

| Dimension | Score | Weight | Weighted |
|---|---|---|---|
| Structural Completeness | 4 | 0.20 | 0.80 |
| Verifiable Accountability | 5 | 0.20 | 1.00 |
| Machine-Readable Structure | 3 | 0.15 | 0.45 |
| Traceability to Source | 4 | 0.15 | 0.60 |
| Sealed Against Entropy | 4 | 0.15 | 0.60 |
| Maturity Alignment | 3 | 0.15 | 0.45 |
| **Total** | | | **3.90** |

G_done = 3.90 / 5.0 = **0.78** — Below threshold. Requires remediation.

Remediation path: Improve Machine-Readable Structure (3→4) and Maturity Alignment (3→4) to achieve G_done = 0.84.

## Scorer Calibration

To ensure consistency across multiple scorers:

1. **Score a reference artifact independently** — each scorer evaluates the same artifact without discussion
2. **Compare scores** — identify dimensions where scorers diverge by more than 1 point
3. **Discuss discrepancies** — align on interpretation of criteria for those dimensions
4. **Re-score** — repeat until consistency is within ±0.5 per dimension
5. **Document consensus** — record the calibrated interpretation for future reference

**Calibration frequency:** Every time a new scorer joins, and quarterly thereafter.

## Maturity Progression

The scoring approach itself matures over time:

### Phase 1: Manual Scoring (Current)

Human scorers use the 1-5 rubric and calculate composite scores manually or with a spreadsheet.

**Gate to next phase:** Consistent scoring across all scorers (±0.5 per dimension) for 3 consecutive artifacts.

### Phase 2: Automation-Assisted Scoring

Automation collects scores, calculates composites, generates remediation reports, and tracks trends. Human scorers still assign the 1-5 ratings.

**Gate to next phase:** Automated scoring reports used as input for governance decisions for 3 consecutive quarters.

### Phase 3: Policy-as-Code Enforcement

Scoring is machine-enforced. Artifacts that do not meet G_done ≥ 0.80 cannot proceed through the workflow. Human override requires documented exception with expiry date.

**Gate to maturity:** Fully automated scoring with machine-enforced gates for 2 consecutive quarters.

## Exception Process

If an artifact does not meet the threshold but must proceed:

1. Exception request documented with rationale
2. Approved by designated governance authority
3. Remediation plan with timeline attached
4. Exception tracked in governance debt register
5. Exception has mandatory expiry date — automatic reversion if not remediated

> Exceptions are governance debt. Track them. Expire them. Never let them become permanent policy.

## Scoring Template

For each artifact, record:

- Artifact ID and title
- Scorer name and date
- Score (1-5) for each of the six dimensions
- Evidence statement for each score
- Calculated composite score (G_done)
- Governance-complete determination (yes/no)
- Remediation actions (if below threshold)

## Key Principle

> Scoring is a progression, not an event. Start manual, build automation, enforce with code. The threshold is G_done ≥ 0.80. The rule is simple: score it, calculate it, verify it. If it doesn't meet the threshold, remediate it before proceeding.
