# Appendix H: Agentic Workspace Governance Checklist

## Purpose

This checklist provides a practical, implementable governance framework for deploying agentic AI workspaces in regulated environments. An "agentic workspace" is any AI system that takes autonomous actions on behalf of users — sending communications, modifying records, triggering workflows, or surfacing data — based on natural language instructions.

This applies to any platform where non-technical users directly instruct AI agents that act on enterprise systems.

## Pre-Deployment Governance (Before First User)

### Identity & Access

- [ ] Every user has a verified identity linked to their agent session
- [ ] Role-based access control (RBAC) is configured per user group
- [ ] Agent data access is scoped to user's role (principle of least privilege)
- [ ] Cross-role data access is blocked architecturally (not just by policy)
- [ ] Service accounts used by agents have separate, auditable identities
- [ ] Session timeout and re-authentication policies are defined

### Action Classification

- [ ] All possible agent actions are inventoried and classified:
  - **Read-only** (query data, generate reports) — Low risk
  - **Internal modify** (update records, create documents) — Medium risk
  - **External communicate** (send emails, messages, notifications) — High risk
  - **Irreversible** (delete records, close accounts, trigger payments) — Critical risk
- [ ] High-risk and critical actions require explicit human approval before execution
- [ ] Action classification is enforced at runtime, not advisory

### Data Governance

- [ ] Sensitive data categories are defined (PII, PHI, financial, confidential)
- [ ] Data classification labels are applied to all sources the agent can access
- [ ] Agent responses are filtered for sensitive data before surfacing to user
- [ ] Data residency requirements are enforced (geographic, regulatory)
- [ ] Third-party data sources have data sharing agreements in place
- [ ] Agent cannot be used to circumvent existing data access controls

### Policy-as-Code

- [ ] Governance rules are codified (not documented in PDFs)
- [ ] Policy violations trigger automatic blocking (not just logging)
- [ ] Exceptions have mandatory expiry dates with automatic reversion
- [ ] Policy changes are versioned and deployed through CI/CD
- [ ] Policy testing environment exists for validating rule changes

## Runtime Governance (During Operation)

### Action Audit Trail

- [ ] Every user instruction is logged (natural language, timestamp, user identity)
- [ ] Agent interpretation of instruction is logged (what it understood)
- [ ] Boundary decision is logged (PERMIT / DENY / ESCALATE + reason)
- [ ] Action taken is logged (what systems were touched, what changed)
- [ ] Outcome is logged (success/failure, data returned, effects)
- [ ] Full chain is queryable: instruction → interpretation → decision → action → outcome

### Approval Workflows

- [ ] High-risk actions queue for human approval before execution
- [ ] Approval requests include: what was asked, what will be done, what data is involved
- [ ] Approvers have sufficient context to make informed decisions
- [ ] Approval timeout defined (action cancelled if not approved within N minutes)
- [ ] Approval history is part of the audit trail

### Anomaly Detection

- [ ] Baseline consumption patterns established per user role
- [ ] Alerts trigger on: unusual query volume, after-hours usage, boundary probing patterns
- [ ] Failed permission attempts are logged and trigger review after threshold
- [ ] "Boundary testing" behavior (repeated attempts at restricted actions) is flagged
- [ ] Consumption cost spikes are correlated with specific users/actions

### Sensitive Data Protection

- [ ] Real-time PII/PHI detection on agent outputs before display
- [ ] Tokenization or redaction applied to sensitive fields in responses
- [ ] Agent cannot include sensitive data in external communications
- [ ] Data access logs separate from action logs (who saw what vs. who did what)
- [ ] Periodic access review: are users accessing data appropriate to their role?

## Post-Deployment Governance (Ongoing)

### Measurement & Reporting

- [ ] Governance health metrics tracked per sprint/period:
  - Actions governed (% of total actions with full audit trail)
  - Boundary violations (attempted + blocked)
  - Escalations triggered (and resolution time)
  - Sensitive data exposure incidents (near-misses + actual)
  - Consumption efficiency (value generated per token consumed)
- [ ] Monthly governance report generated for compliance officers
- [ ] Quarterly review of role boundaries (are they still appropriate?)
- [ ] Annual penetration test of governance boundaries

### Continuous Improvement

- [ ] User feedback on false-positive governance blocks collected
- [ ] Governance rules refined based on operational data (not just initial design)
- [ ] New action types (from platform updates) are classified before enabling
- [ ] Agent capability expansions require governance review before deployment
- [ ] Lessons from incidents feed back into policy-as-code updates

### Compliance Evidence

- [ ] Audit trail satisfies regulatory retention requirements (duration, format, accessibility)
- [ ] Evidence package can be produced on-demand for auditors
- [ ] Chain of custody for audit data is documented and protected
- [ ] Governance architecture itself is documented and version-controlled
- [ ] Compliance mapping: each regulatory requirement → specific governance control

## Scoring: Governance Readiness Assessment

Score each item: 0 (not started), 1 (in progress), 2 (complete and verified)

| Section | Items | Max Score |
|---|---|---|
| Pre-Deployment: Identity & Access | 6 | 12 |
| Pre-Deployment: Action Classification | 4 | 8 |
| Pre-Deployment: Data Governance | 6 | 12 |
| Pre-Deployment: Policy-as-Code | 5 | 10 |
| Runtime: Action Audit Trail | 6 | 12 |
| Runtime: Approval Workflows | 5 | 10 |
| Runtime: Anomaly Detection | 5 | 10 |
| Runtime: Sensitive Data Protection | 5 | 10 |
| Post-Deployment: Measurement | 5 | 10 |
| Post-Deployment: Continuous Improvement | 5 | 10 |
| Post-Deployment: Compliance Evidence | 5 | 10 |
| **TOTAL** | **57** | **114** |

**Scoring interpretation:**
- **0-38:** Critical governance gaps — do not deploy to production
- **39-76:** Moderate readiness — deploy with strict monitoring and rapid remediation plan
- **77-100:** Strong readiness — deploy with standard monitoring
- **101-114:** Governance-complete — deploy with confidence

## Key Principle

> An agentic workspace without governance is an authorized insider threat. Every user instruction is a potential policy violation. Every agent action is a potential compliance incident. Governance must be structural, real-time, and independent of user awareness.
