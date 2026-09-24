# Risk Assessment and Treatment Request

> **Fictional scenario.** Sierra Meridian Field Services, Inc. is an invented company created for this portfolio. All figures are illustrative assumptions, and the basis for each is stated.

## Document Header

| Field | Value |
|-------|-------|
| Risk ID | RA-2026-005 |
| System/Asset | InvoiceTrack (in-house AP system), ShareFile permission model, and supporting scripts and credentials |
| Risk Owner | Chief Operating Officer |
| Control Owner | Chief Technology Officer |
| Prepared By | Faith Olofintuyi, Risk & Compliance |
| Date | September 24, 2026 |
| Review Date | December 24, 2026 |
| Version | 0.1 (Draft) |
| Status | **Submitted for decision.** No acceptance has been granted. |
| Linked Risk | RA-2026-004: Inherited partner access to draft invoices |

> **Why one owner and one control owner:** A risk with two owners tends to have none. The COO owns the business consequence (AP must keep running and vendors must be paid). The CTO owns the fix (documentation, credentials, cross-training). The COO signs the decision; the CTO is accountable for delivering it.

---

## 1. Executive Summary

One systems engineer built and maintained InvoiceTrack, the ShareFile permission model behind our invoice folders, and the scripts that connect them. That engineer was laid off in the last reduction in force with no handover. Nobody now employed understands how these systems fit together, and some passwords and scripts he used may still be active.

This matters for two reasons. First, if InvoiceTrack breaks, nobody can fix it quickly, and we cannot pay field vendors until someone does. Second, it is the root cause of RA-2026-004: we cannot safely cut partner access to draft invoices because we do not know what that change would break.

Risk & Compliance recommends Option B: rotate every credential the engineer knew, pay for a short documentation engagement, and cross-train one staff member. It costs about $35K to $60K and can be combined with the contractor work in RA-2026-004 Option B, which lowers the cost of both.

**Recommendation:**
- [ ] Option A: Full remediation. Document everything, cross-train two people, and adopt a company-wide knowledge-transfer policy.
- [x] Option B: Targeted remediation. Rotate credentials, document the critical systems, cross-train one person, add a handover checklist. **(Risk & Compliance recommendation)**
- [ ] Option C: Accept the risk as it stands.
- [ ] Option D: Minimal improvements. Rotate credentials and add a handover checklist only.

---

## 2. Risk Description

### 2.1 What Could Happen

| Scenario | Description |
|----------|-------------|
| Scenario 1: System failure with no one to fix it | A routine server update, certificate expiry, or ShareFile change breaks InvoiceTrack. Nobody knows how it works, so AP stops until an outside expert reverse-engineers it. Field vendors go unpaid and job sites are delayed. |
| Scenario 2: Access by a former employee | Passwords and service accounts the engineer used were never changed. Anyone who has them, including the former employee or someone who later obtains his notes, can still get into InvoiceTrack or the invoice folders. |
| Scenario 3: Unsafe changes | Staff attempt to fix RA-2026-004 or apply an update without understanding the dependencies, and break the AP workflow or delete vendor data. |
| Scenario 4: Stalled remediation | Because the change is too risky to attempt, RA-2026-004 stays open indefinitely and its exposure (payment fraud, SOC 2 exception) continues. |

### 2.2 How It Could Happen

1. **No documentation.** No runbook, diagram, or permission map exists for InvoiceTrack or the ShareFile structure.
2. **Unrotated credentials.** Service account passwords and script credentials were known only to the engineer and were not changed when he left.
3. **Scripts tied to a personal account.** Some automated jobs may run under the engineer's own account. Disabling it could break them; leaving it active leaves a former employee's account open.
4. **Layoff without transition.** The reduction in force was carried out without a knowledge-transfer step for critical roles.

### 2.3 Why We Might Not Know

- We have no inventory of the scripts, scheduled jobs, or service accounts the engineer created, so we cannot tell which are still running or who can use them.
- There is no logging on InvoiceTrack's service accounts, so use of an old password would look like normal system activity.
- A failure may not show up until the next AP payment run.

---

## 3. Risk Assessment

### 3.1 Likelihood Assessment

| Factor | Assessment | Evidence |
|--------|------------|----------|
| System fragility | High | InvoiceTrack was built in 2014 and depends on the current folder structure. Routine updates to servers, certificates, or ShareFile can break older integrations. |
| Available expertise | None | No current employee can support InvoiceTrack or safely change its permissions. |
| Credential exposure | Moderate | Credentials known to a former employee were not rotated. No evidence of misuse, but no logging that would show it. |
| Historical incidents | 0 outages since departure | The departure was recent, so a clean record so far says little. |

**Overall Likelihood:** Moderate-High

**Basis:**
Legacy systems do not break often, but when they do, someone has to understand them. Today nobody does, and the next update or certificate renewal is a matter of when, not if.

### 3.2 Impact Assessment

| Impact Category | Potential Consequence | Estimated Cost |
|-----------------|----------------------|----------------|
| Business Disruption | AP outage until an outside expert restores InvoiceTrack | $25K to $40K per day, 5 to 15 days (same daily basis as RA-2026-004) |
| Emergency Recovery | Outside specialists to reverse-engineer the system under time pressure | $60K to $150K |
| Unauthorized Access | Use of unrotated credentials to reach invoices or vendor data; investigation, counsel, notification if required | $150K to $400K |
| Stalled Remediation | RA-2026-004 cannot be fixed, so its exposure continues | See RA-2026-004 ($0.9M to $2.2M most likely) |
| Compliance | SOC 2 findings on succession planning, change management, and access removal | Remediation cost plus client scrutiny |

**Total Potential Impact (this risk alone):** $0.3M to $1.2M
**Most Likely Scenario:** $0.3M to $0.6M (one outage of about a week, emergency contractor recovery, and a SOC 2 finding)

*The larger cost is indirect: while this risk is open, RA-2026-004 cannot be safely closed.*

### 3.3 Risk Rating

```
                    IMPACT
                    Low    Med    High   Critical
           High   |  M   |  H   |  H   |   C    |
LIKELIHOOD Med    |  L   |  M   |  H   |   H    |
           Low    |  L   |  L   |  M   |   H    |

Current Risk Position: HIGH (Likelihood: Moderate-High, Impact: High when combined with RA-2026-004)
```

---

## 4. Existing Controls Assessment

### 4.1 Controls Currently in Place

| Control | Description | Effectiveness | Honest Assessment |
|---------|-------------|---------------|-------------------|
| HR offboarding checklist | Laptop return and account disablement on the last day | Weak | Covers personal accounts only. Does not cover service accounts, shared passwords, scripts, or knowledge transfer. |
| Vendor support | None | None | InvoiceTrack was built in-house. There is no vendor to call. |
| Backups | Nightly backups of InvoiceTrack data | Adequate for data | Protects the data, not the knowledge needed to restore or run the system. |

### 4.2 Controls NOT in Place

| Control | Why Missing | Impact |
|---------|-------------|--------|
| System documentation (runbook, diagrams, permission map) | Never required; one person held it all | No one can support or safely change the system |
| Credential rotation on departure | Offboarding covers personal accounts only | Former employee's knowledge may still grant access |
| Service account and script inventory | Never built | We do not know what is running or under whose account |
| Cross-training or backup for critical roles | No succession planning | A single departure stops a business-critical process |
| Knowledge transfer in the layoff process | Not part of the reduction-in-force plan | Critical knowledge left on the last day |

### 4.3 Overall Control Effectiveness

Current controls address the departing employee's laptop and personal login, not what he knew or the shared credentials he held.

**Rating:** [ ] Strong [ ] Adequate [ ] Weak [x] Insufficient

---

## 5. Options Analysis

### Option A: Full Remediation

| Aspect | Detail |
|--------|--------|
| Description | Fully document InvoiceTrack, the permission model, and all scripts; cross-train two staff; adopt a company-wide knowledge-transfer policy for all critical roles. |
| Actions Required | Rotate all credentials; build a service account and script inventory; contractor documentation engagement; train two staff members; identify all single-person dependencies company-wide. |
| Cost | $90K to $140K |
| Timeline | 4 to 6 months |
| Residual Risk | Low |
| Pros | Removes the dependency; prevents repeats elsewhere; strong SOC 2 evidence |
| Cons | Largest spend; takes two staff members away from other work during training |

### Option B: Targeted Remediation (Risk & Compliance Recommendation)

| Aspect | Detail |
|--------|--------|
| Description | Fix the immediate exposure and capture the critical knowledge, without a company-wide program. |
| Actions Required | Rotate every credential the engineer knew; inventory service accounts and scripts and move them off his personal account; 40 to 80 hours of documentation work (contractor, or the former engineer through a consulting firm; see Section 9.1); cross-train one IT staff member; add a knowledge-transfer checklist to offboarding and layoff planning. |
| Cost | $35K to $60K |
| Timeline | 60 to 90 days |
| Residual Risk | Medium |
| Pros | Closes the access gap within weeks; unblocks RA-2026-004; can share one contractor engagement with RA-2026-004 Option B |
| Cons | Still one trained backup; InvoiceTrack remains legacy |

### Option C: Accept Risk

| Aspect | Detail |
|--------|--------|
| Description | Continue as is. |
| Actions Required | Sign this document. |
| Direct Cost | $0 |
| Potential Cost | $0.3M to $1.2M, plus RA-2026-004 remaining open |
| Residual Risk | High |
| Pros | No spend |
| Cons | Former employee's credentials stay live; next outage has no fix; RA-2026-004 cannot close |

### Option D: Minimal Improvements

| Aspect | Detail |
|--------|--------|
| Description | Close the credential gap and prevent the next departure from repeating this, but do not document or cross-train. |
| Actions Required | Rotate credentials; add a knowledge-transfer checklist to offboarding. |
| Cost | About $5K plus staff time |
| Timeline | 2 weeks |
| Residual Risk | Med-High |
| Pros | Fast; removes the former-employee access risk |
| Cons | Nobody can still support InvoiceTrack; RA-2026-004 stays blocked |

### Options Comparison

| Factor | Option A | Option B | Option C | Option D |
|--------|----------|----------|----------|----------|
| Direct Cost | $90K to $140K | $35K to $60K | $0 | About $5K |
| Timeline | 4 to 6 months | 60 to 90 days | None | 2 weeks |
| Residual Risk | Low | Medium | High | Med-High |
| Unblocks RA-2026-004 | Yes | Yes | No | No |
| Compliance Impact | Resolves findings | Likely resolves findings | Probable findings | Partly resolves access finding |

---

## 6. Recommendation

### 6.1 Selected Option

**Risk & Compliance recommendation:** Option B, Targeted Remediation
**Executive decision:** Pending

### 6.2 Rationale

1. **It closes the most urgent gap first.** Rotating credentials the former engineer knew costs almost nothing and removes a live access risk this month.
2. **It unblocks RA-2026-004.** The documentation work maps the permission model, which is exactly what we need before we can safely cut partner access to draft invoices. One contractor engagement can serve both risks.
3. **It is proportionate.** Option A's company-wide program is good practice, but this is the only single-person dependency we have identified. Option B fixes the known problem and adds a checklist so the next layoff does not create a new one.

### 6.3 Immediate Actions

Within 2 weeks, regardless of which option is chosen:

- [ ] Rotate every password, key, and service account credential the engineer knew or managed (Owner: CTO)
- [ ] List all scripts and scheduled jobs running under the engineer's account and move them to service accounts (Owner: IT Operations)
- [ ] Confirm the engineer's personal accounts are disabled everywhere, including ShareFile and InvoiceTrack (Owner: IT Operations)
- [ ] Confirm that company documents, code, and notes were returned at separation (Owner: HR with Legal)

### 6.4 Future Commitments

| Action | Timeline | Owner | Budget |
|--------|----------|-------|--------|
| Documentation engagement (shared with RA-2026-004 Option B) | Within 60 days | CTO | $25K to $45K |
| Cross-train one IT staff member on InvoiceTrack | Within 90 days | CTO | Staff time |
| Add knowledge-transfer checklist to offboarding and reduction-in-force planning | Within 30 days | COO with HR and Legal | None |
| Add handover and cooperation terms to employment and severance templates | Within 90 days | General Counsel | None |

---

## 7. Residual Risk Acknowledgment

After implementing Option B, the following risks remain:

| Residual Risk | Description | Why Accepted |
|---------------|-------------|--------------|
| One trained backup | If both the new backup and the documentation are unavailable, the dependency returns | Proportionate to a single known dependency; revisit at review |
| Legacy system | InvoiceTrack is still old and fragile | Replacement is Option A in RA-2026-004; not funded this year |
| Unknown unknowns | The inventory may miss a script or credential | Documentation work will surface most; logging will help detect the rest |

### 7.1 What We Will NOT Be Able to Do if an Incident Occurs (Before Remediation)

1. Restore InvoiceTrack quickly after a failure.
2. Tell whether a service account was used by the former engineer or by someone else.
3. Safely change folder permissions to close RA-2026-004.

---

## 8. Conditions and Validity

### 8.1 This Request Assumes:
- [ ] Section 6.3 immediate actions are completed within 14 days, whatever option is chosen
- [ ] The decision is made by December 24, 2026
- [ ] Progress is reported alongside RA-2026-004 in the quarterly Audit Committee update

### 8.2 Review Date:
December 24, 2026 (three months). Shorter than RA-2026-004 because this risk blocks that one.

### 8.3 Re-evaluation Triggers
- [ ] Any InvoiceTrack outage or failed update
- [ ] Any sign that an old credential was used
- [ ] Another reduction in force or departure from IT
- [ ] A SOC 2 finding related to succession, change management, or access removal

---

## 9. Compliance Implications

| Framework | Requirement | Impact |
|-----------|-------------|--------|
| SOC 2 (CC1.4) | Commitment to competence, including planning and preparing for succession | No succession or backup for a business-critical role. Likely finding. |
| SOC 2 (CC3.4) | Changes that could significantly affect internal control are identified and assessed | A layoff that removed the only person able to run a critical system was not assessed for control impact |
| SOC 2 (CC6.2, CC6.3) | Access is removed when no longer needed | Shared credentials known to a former employee were not rotated |
| SOC 2 (CC8.1) | Changes to systems are authorized, tested, and documented | No documentation exists to support safe change to InvoiceTrack |

### 9.1 Legal Considerations

- **Severance can buy cooperation; final pay cannot.** California requires final wages to be paid at termination (Labor Code §201), so the company cannot hold back final pay to force a handover. It can, however, offer severance in exchange for a release and a paid transition or cooperation period. Future severance templates for critical roles should include a cooperation clause with defined hours and a consulting rate.
- **Employment agreements and job descriptions.** For roles that run critical systems, documentation and knowledge-transfer duties should be written into the job description and employment agreement, so a handover is part of the job and not a favor asked at the exit.
- **Company property.** Scripts, documentation, and notes the engineer created in the course of employment belong to the company (Labor Code §2860). The separation process should confirm they were returned, and the confidentiality agreement should require it. California generally voids non-competes (Business and Professions Code §16600), but confidentiality and return-of-property obligations remain enforceable.
- **Re-engaging the former engineer.** The fastest fix may be to hire him back for a short documentation project. Hiring a former employee as an independent contractor to do the same work he did as an employee is risky under California's ABC test for worker classification (Labor Code §2775). Engaging him through a staffing or consulting firm, or through his own established business that meets the business-to-business criteria, reduces that risk. Legal should review before any engagement.
- **Layoff planning.** If a future reduction in force triggers California WARN Act notice, the notice period is a natural window for knowledge transfer. Paying wages in lieu of notice gives that window up. Critical-role handover should be a standard step in layoff planning.

---

## 10. Signatures

### Risk Owner (Business)
By signing, I acknowledge that I understand the risk described above and approve the selected option on behalf of the organization.

| Field | Value |
|-------|-------|
| Name | |
| Title | Chief Operating Officer |
| Date | |
| Signature | |

### Control Owner
By signing, I accept responsibility for delivering the actions assigned to me.

| Field | Value |
|-------|-------|
| Name | |
| Title | Chief Technology Officer |
| Date | |
| Signature | |

### Compliance Review
By signing, I confirm that relevant compliance and legal implications have been considered.

| Field | Value |
|-------|-------|
| Name | |
| Title | General Counsel |
| Date | |
| Signature | |

---

## Version History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 0.1 | September 24, 2026 | Faith Olofintuyi | Initial draft |
