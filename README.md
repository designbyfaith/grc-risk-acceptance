# Risk Acceptance: Inherited Partner Access to Draft Invoices

**Author:** Faith Olofintuyi, GRC professional and attorney

A formal risk acceptance for a realistic security gap, written the way it would be presented to a CFO and executive team: plain language, specific figures, honest control ratings, real options, and a clear record of who decided what.

> The company, people, and figures are fictional.

## The Situation in Brief

Sierra Meridian Field Services, an oil and gas operations contractor in Sacramento, California, lets external partners (some offshore) open and edit the folders where draft invoices sit just before vendors are paid. The access comes through inherited permissions that nobody still at the company understands, because the engineer who built them was laid off without a handover.

The main risk is **payment fraud**: someone using a partner's account could change a vendor's bank details, and the company would likely pay the wrong account. Risk & Compliance recommended cutting partner access (Option B, about $70K to $110K). The executive team chose a cheaper interim fix (Option D, about $15K). This document records both, and limits the acceptance to six months with conditions.

## What's in This Repo

| File | What It Is |
|------|------------|
| [Risk Acceptance RA-2026-004](deliverables/RA-2026-004-invoice-access-risk-acceptance.md) | The main deliverable: risk description, likelihood and impact, control assessment, four options, recommendation vs. decision, residual risk, conditions, compliance and legal analysis |
| [Scenario](scenario.md) | The organization, the gap, why it was not fixed, stakeholders, and constraints |
| [Decision Log](deliverables/decision-log.md) | The judgment calls I made while building this project, and why |
| [Project Brief](PROJECT-BRIEF.md) | The original exercise brief (credited to its author) |

## Key Judgment Calls

- **Payment fraud first, data exposure second.** Draft invoices are the last step before payment, so the costliest harm is a changed bank account, not a leak.
- **Keep the recommendation on the record.** The document shows what Risk & Compliance advised and what the executives chose. An acceptance that hides the advice is not an honest record.
- **Honest control ratings.** Manager approval and bank-change forms look relevant, but both can be defeated by the same edited document or email, so overall control effectiveness is rated Insufficient.
- **Acceptance with limits.** Six months, tied to conditions, with triggers that force a new decision.

## The Legal Lens

As an licensed attorney, I added analysis a typical risk acceptance leaves out:

- **Authority to accept:** whether a CFO can accept a risk of this size, or it needs executive and Audit Committee involvement
- **Discoverability:** a signed acceptance of a known gap can become evidence after an incident, which affects how it is drafted and kept
- **Privilege:** keeping liability analysis separate and under counsel's direction
- **CCPA:** applicability, the limits of the private right of action, and the new cybersecurity audit rule
- **Recording consent:** California's all-party consent rule for the documented meetings

## Status

- [x] Scenario
- [x] Risk acceptance draft (v0.1)
- [x] Decision log
- [ ] Linked risk RA-2026-005: key-person dependency
- [ ] Reflection questions
- [ ] 5-minute video walkthrough
