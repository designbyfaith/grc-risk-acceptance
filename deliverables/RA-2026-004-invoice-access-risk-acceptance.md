# Risk Acceptance Document

> **Fictional scenario.** Sierra Meridian Field Services, Inc. is an invented company created for this portfolio. Any resemblance to a real organization is coincidental. All figures are illustrative assumptions, and the basis for each is stated.

## Document Header

| Field | Value |
|-------|-------|
| Risk ID | RA-2026-004 |
| System/Asset | Accounts Payable invoice review folders ("AP Draft Invoices") and the legacy InvoiceTrack workflow |
| Risk Owner | Chief Financial Officer |
| Prepared By | Faith Olofintuyi, Risk & Compliance |
| Date | September 22, 2026 |
| Review Date | March 22, 2027 |
| Version | 0.1 (Draft) |
| Linked Risk | RA-2026-005: Key-person dependency (loss of systems engineering knowledge) |

### Organization Profile

| Attribute | Detail |
|-----------|--------|
| Company | Sierra Meridian Field Services, Inc. (fictional) |
| Industry | Oil and gas operations contracting |
| Headquarters | Sacramento, California |
| Workforce | 1,610 employees; about 70% remote |
| Annual revenue | About $285M (assumption) |
| Vendor payments | About $140M per year to about 900 active vendors (assumption) |
| Frameworks and laws | SOC 2 Type II; California Consumer Privacy Act (CCPA) |

---

## 1. Executive Summary

External partners and their staff (some located offshore) can open and edit the ShareFile folders where draft invoices sit just before vendors are paid. They get this access through inherited permissions that nobody still at the company fully understands. The main danger is payment fraud rather than data leakage, though both risks exist. Someone using a partner's account could change a vendor's bank details, and we would likely pay the wrong account without noticing until the real vendor chased payment weeks later.

Fixing this is hard right now. The engineer who built the permission model was laid off without a handover, and changing the legacy InvoiceTrack system would mean downtime and retraining. Upgrading the software version could also result in vendor data loss.

The Risk & Compliance team recommended Option B, which removes partner access and adds payment checks for about $70K to $110K. On August 19, 2026, the executive team chose Option D instead: payment checks and logging only, for about $15K, because of budget and staffing limits.

This acceptance runs for six months and holds only if the Section 8 conditions are met.

**Recommendation:**
- [ ] Option A: Full remediation. Rebuild the invoice repository with role-based access and retire the legacy permission model.
- [x] Option B: Partial remediation. Move invoice folders to an internal-only location, cut partner access, and add payment verification controls. **(Risk & Compliance team recommendation)**
- [ ] Option C: Accept the risk as it stands.
- [ ] Option D: Accept with minimal improvements. Add payment verification and logging only, and keep the current folder structure.

> **Decision status:** On August 19, 2026, the executive team declined Options A and B, citing budget and staffing. This document records that decision under Option D (Section 6) and preserves the Risk & Compliance team's recommendation and the reasons for it.

---

## 2. Risk Description

### 2.1 What Could Happen

We could send vendor payments to the wrong bank account, expose confidential pricing and vendor data, and fail our next SOC 2 audit, which several major clients require us to pass.

| Scenario | Description |
|----------|-------------|
| Scenario 1: Payment redirection | A partner user, or an attacker using a partner's stolen login, edits a draft invoice or the attached remittance instructions to show a new bank account. The invoice passes final approval because it looks routine, and the payment goes to the attacker. |
| Scenario 2: Data exposure | Draft invoices contain vendor bank details, contact names, contract rates, and job site information. A partner downloads or forwards them, deliberately or by mistake, exposing personal and commercially sensitive information. |
| Scenario 3: Audit failure and client loss | The SOC 2 auditor finds that external users can reach internal financial records. We barely passed last year, and a qualified opinion could trigger contract reviews with clients whose agreements require a clean report. |

### 2.2 How It Could Happen

1. **Inherited permissions.** The invoice folders sit inside a ShareFile folder structure that is also shared with external partners. Access flows down automatically, and nobody still here fully understands how it was set up.
2. **Compromised partner accounts.** We do not control partners' password or MFA practices. If any one of them is phished, the attacker inherits their access to our folders.
3. **Insider misuse at a partner.** A partner employee with a legitimate reason to use the shared site can also see invoices that are none of their business.
4. **Offshore access.** Some partner staff work outside the United States. We do not know which countries or how many people, and misuse from abroad is harder to investigate and much harder to recover money from.
5. **Social engineering after exposure.** Even read-only access gives a criminal the exact vendor names, invoice numbers, and amounts needed to send a convincing fake "please update our bank details" email.

### 2.3 Why We Might Not Know

- File-level audit logging is not enabled on the invoice folders, so we cannot see who opened or edited a draft.
- No alerts fire when an external user touches finance folders.
- Approvers review the invoice document itself. If the document has been altered, the approver sees the altered version and has no reason to question it.
- Diverted payments usually surface only when the real vendor calls asking why they were not paid, often 30 to 60 days later, when recovery is unlikely.

---

## 3. Risk Assessment

### 3.1 Likelihood Assessment

| Factor | Assessment | Evidence |
|--------|------------|----------|
| Threat Actor Interest | High | The FBI's 2025 IC3 report attributes about $3.05B in reported losses to business email compromise, the fraud category that includes payment redirection. Companies with high-volume, high-value vendor payments are attractive targets. |
| Attack Complexity | Low | No hacking is required. Anyone with partner-level access can open or edit the folders. Taking over a partner account through phishing is routine. |
| Detection Capability | Low | No file audit logging, no external access alerts, and approvers cannot tell whether a document was altered. |
| Historical Incidents | 0 confirmed; 2 suspicious | No confirmed fraud. In the last 12 months, AP staff caught two unexpected bank-change requests by chance. Neither was investigated for a link to folder access. |

**Overall Likelihood:** Moderate-High

**Basis:**
The attack is cheap, the reward is large, and we have no way to see it happening. The absence of confirmed incidents is weak comfort, because we lack the logging that would reveal one.

### 3.2 Impact Assessment

| Impact Category | Potential Consequence | Estimated Cost |
|-----------------|----------------------|----------------|
| Data Breach | Exposure of vendor bank details, contact data, and contract rates; forensic investigation, outside counsel, notification where required | $150K to $400K |
| Financial Loss | One or more diverted vendor payments. Our largest single invoices run about $750K. Funds are rarely recovered once moved. | $75K to $750K per event |
| Regulatory Action | CCPA: statutory damages of $107 to $799 per consumer per incident if qualifying personal information is breached (about 900 vendors, but only sole proprietors count as consumers), plus possible administrative fines | $0 to $720K (upper bound; assumes all 900 vendors qualify, so real exposure is lower) |
| Reputation | A qualified SOC 2 opinion leads a major client to rebid or terminate. Our three largest clients account for about 38% of revenue (assumption). | $5M to $20M in annual revenue at risk |
| Business Disruption | AP frozen during investigation; late payments to field vendors delay job sites | $25K to $40K per day, 5 to 10 days |

**Total Potential Impact:** $0.4M to about $22M range
**Most Likely Scenario:** $0.9M to $2.2M (one diverted payment partly recovered, investigation costs, SOC 2 exception, remediation under deadline pressure, and heightened client scrutiny short of losing a client)

*Estimate basis: invoice sizes and client concentration are scenario assumptions. CCPA figures are the amounts in effect since January 1, 2025. Ranges are deliberately wide, because the honest answer is that we do not know which scenario would occur.*

### 3.3 Risk Rating

```
                    IMPACT
                    Low    Med    High   Critical
           High   |  M   |  H   |  H   |   C    |
LIKELIHOOD Med    |  L   |  M   |  H   |   H    |
           Low    |  L   |  L   |  M   |   H    |

Current Risk Position: HIGH (Likelihood: Moderate-High, Impact: High)
```

---

## 4. Existing Controls Assessment

### 4.1 Controls Currently in Place

| Control | Description | Effectiveness | Honest Assessment |
|---------|-------------|---------------|-------------------|
| MFA for employees | All internal staff use MFA | Strong (internal only) | Protects our accounts, not partner accounts, which is where the exposure is. |
| AP manager approval | A manager approves each invoice before payment | Weak | The approver reviews the same document an attacker can edit, so a tampered invoice is approved as genuine. |
| Vendor bank-change form | Bank detail changes require a signed form | Weak | Forms are accepted by email, and email is how the fraud arrives. No call-back verification. |
| Partner NDAs | Partners are contractually bound to confidentiality | Weak | Gives us a legal remedy after harm. Does not prevent harm and does not bind an outside attacker. |
| Annual SOC 2 access review | Access lists are reviewed once a year | Weak | Last year's review did not catch inherited external access to finance folders. |

### 4.2 Controls NOT in Place

| Control | Why Missing | Impact |
|---------|-------------|--------|
| Separation of finance folders from partner-shared areas | Permission model undocumented; the engineer who built it was laid off without handover | Core cause of the exposure |
| File audit logging and alerting | Never enabled; no owner | We cannot detect or investigate misuse |
| Call-back verification of bank changes | Not part of the AP procedure | Main defense against payment redirection is missing |
| Expiry of guest and partner accounts | No process | Former partner staff may still have access |
| Documented permission design and succession plan | Knowledge left with one person | Fixes are slow and risky (see RA-2026-005) |

### 4.3 Overall Control Effectiveness

Current controls protect our own users well but do almost nothing about the actual exposure, which is external users reaching internal financial documents. The two controls that look relevant, manager approval and the bank-change form, can both be defeated by the same edited document or email.

**Rating:** [ ] Strong [ ] Adequate [ ] Weak [x] Insufficient

---

## 5. Options Analysis

### Option A: Full Remediation

| Aspect | Detail |
|--------|--------|
| Description | Replace the legacy folder structure and InvoiceTrack workflow with a modern AP platform using role-based access, separate internal-only finance storage, and scoped partner portals. |
| Actions Required | Engage an outside integrator; document and rebuild permissions; migrate invoice history; retrain AP staff; plan cutover downtime. |
| Cost | $450K to $650K (integrator, licensing, training and backfill, productivity loss during cutover) |
| Timeline | 9 to 12 months |
| Residual Risk | Low |
| Pros | Fixes the root cause; strongest SOC 2 position; reduces key-person dependency |
| Cons | Not budgeted this fiscal year; requires outside help; disrupts AP during migration |

### Option B: Partial Remediation (Risk & Compliance Recommendation)

| Aspect | Detail |
|--------|--------|
| Description | Keep the legacy system but move invoice folders to a new internal-only location with no inherited partner access, and add payment-integrity controls. |
| Actions Required | Short contractor engagement to map and document current permissions; create internal-only invoice location; remove partner access; call-back verification for all bank changes; two approvers for payments of $25K or more; turn on audit logging and external-access alerts; quarterly access reviews; expire inactive guest accounts. |
| Cost | $70K to $110K |
| Timeline | 60 to 90 days |
| Residual Risk | Medium |
| Pros | Removes the external exposure at a fraction of Option A's cost; no platform migration or major retraining; strong evidence for the next SOC 2 audit |
| Cons | Legacy system remains; relies on manual reviews; requires a small unbudgeted spend |

### Option C: Accept Risk

| Aspect | Detail |
|--------|--------|
| Description | Continue as is and document the decision. |
| Actions Required | Sign this document. |
| Direct Cost | $0 |
| Potential Cost | $0.9M to $2.2M most likely; up to about $22M (if an incident occurs) |
| Residual Risk | High |
| Pros | No spend; no disruption |
| Cons | Exposure unchanged; likely SOC 2 exception; weak position with clients, auditors, and in any later dispute |

### Option D: Accept with Minimal Improvements

| Aspect | Detail |
|--------|--------|
| Description | Keep the folder structure and partner access, but add low-cost controls that make payment fraud harder and misuse visible. |
| Actions Required | Call-back verification of all bank changes; two approvers for payments of $25K or more; one-time removal of inactive partner accounts; enable file audit logging on invoice folders. |
| Cost | About $15K plus staff time |
| Timeline | 2 to 4 weeks |
| Residual Risk | Med-High |
| Pros | Cheap and fast; directly targets the most costly scenario (diverted payments) |
| Cons | Partners can still see invoices; data exposure and SOC 2 exposure remain largely unchanged |

### Options Comparison

| Factor | Option A | Option B | Option C | Option D |
|--------|----------|----------|----------|----------|
| Direct Cost | $450K to $650K | $70K to $110K | $0 | About $15K |
| Timeline | 9 to 12 months | 60 to 90 days | None | 2 to 4 weeks |
| Residual Risk | Low | Medium | High | Med-High |
| Compliance Impact | Resolves SOC 2 access findings | Likely resolves them, with evidence | Probable SOC 2 exception | Probable exception, partly mitigated |
| Business Impact | AP disruption during migration | Minor process changes | None now; high if incident | Small added AP workload |

---

## 6. Recommendation

### 6.1 Selected Option

**Risk & Compliance team recommendation:** Option B, Partial Remediation
**Executive decision (August 19, 2026):** Option D, Accept with Minimal Improvements

### 6.2 Rationale

**The Risk & Compliance team recommends Option B because:**
1. It closes the actual gap. Moving the draft invoices out of the partner-shared ShareFile site cuts off partner staff, including those offshore, instead of only watching what they do. Option D adds checks but leaves the door open.
2. It costs $70K to $110K, less than a fifth of Option A, and does not require replacing InvoiceTrack. That avoids the downtime, retraining and possible vendor data loss that a platform change would bring.
3. It gives the SOC 2 auditor evidence of real remediation before the March 2027 review, which matters because our largest clients expect a clean report.

**The executive team selected Option D because:**
1. No money is set aside for Option A or B in this fiscal year.
2. Engineering and Technical Support say they have no capacity to take on the work.
3. Option D targets the most expensive scenario, a diverted payment, for about $15K and can be in place within a month.

**Risk & Compliance team position:** Option D lowers the chance of a diverted payment, but it is not a fix. The core exposure stays open: partners can still see and change draft invoices, and a SOC 2 access exception remains likely. For that reason this acceptance is limited to six months and depends on the Section 8 conditions, including submitting Option B in the FY2027 budget.

### 6.3 Immediate Actions

Within 2 weeks:

- [ ] Require call-back verification, to a phone number already on file, for every vendor bank detail change (Owner: AP Manager)
- [ ] Require two approvers for any payment of $25K or more (Owner: Controller)
- [ ] Remove partner accounts inactive for 90 days or more (Owner: IT Operations)
- [ ] Enable file audit logging on AP invoice folders (Owner: IT Operations)
- [ ] Review the two suspicious bank-change requests from the past 12 months (Owner: Risk & Compliance with AP)

### 6.4 Future Commitments

| Action | Timeline | Owner | Budget |
|--------|----------|-------|--------|
| Submit Option B as an FY2027 budget request | Q1 FY2027 planning cycle | CFO | $70K to $110K |
| Document current permission model (see RA-2026-005) | Within 90 days | IT Director | Staff time or contractor |
| Quarterly status report to Audit Committee | Each quarter until closed | Risk & Compliance | None |

---

## 7. Residual Risk Acknowledgment

After implementing Option D, the following risks remain:

| Residual Risk | Description | Why Accepted |
|---------------|-------------|--------------|
| External visibility of invoices | Partners can still open draft invoices and see vendor bank details and pricing | Executive decision based on budget and staffing |
| Document tampering | Partners, or anyone using their accounts, can still edit drafts | Partly mitigated by call-back verification and dual approval |
| SOC 2 access exception | Auditor will likely still find external access to finance data | Accepted with remediation plan for FY2027 |
| Undocumented permissions | Nobody fully understands the current access model | Tracked separately as RA-2026-005 |

### 7.1 What We Will NOT Be Able to Do if an Incident Occurs

1. Determine who viewed or changed a draft invoice before audit logging was enabled.
2. Show an auditor or client that access to financial records followed least privilege.
3. Reliably find and delete every copy of a person's information in response to a CCPA request, because copies may sit in partner-accessible folders.
4. Recover a diverted payment once the funds have moved, which typically happens within days.

---

## 8. Conditions and Validity

### 8.1 This Acceptance is Valid Only If:
- [ ] All Section 6.3 actions are completed within 14 days of signature
- [ ] Option B is formally submitted in the FY2027 budget process
- [ ] Quarterly status is reported to the Audit Committee

If any condition is not met, this acceptance lapses and the risk returns to the executive team for a new decision.

### 8.2 This Acceptance Expires On:
March 22, 2027 (six months, before SOC 2 fieldwork begins)

### 8.3 Re-evaluation Triggers
This acceptance must be re-evaluated immediately if:
- [ ] Any suspected or confirmed payment redirection or invoice tampering occurs
- [ ] A partner with folder access reports a security incident
- [ ] The SOC 2 auditor raises an access-related finding
- [ ] A client requests evidence of access controls over financial data
- [ ] The company meets the CCPA cybersecurity audit thresholds

---

## 9. Compliance Implications

| Framework | Requirement | Impact of Acceptance |
|-----------|-------------|---------------------|
| SOC 2 (CC6.1) | Logical access to protected information is restricted | External users can reach internal financial records. Likely exception. |
| SOC 2 (CC6.2, CC6.3) | Access is granted, changed, and removed based on role and least privilege | Inherited partner access is not role-based; no guest expiry |
| SOC 2 (CC6.6) | Protection against threats from outside system boundaries | Partner accounts sit outside our control yet reach finance data |
| SOC 2 (CC9.2) | Risks from vendors and business partners are assessed and managed | Partner access has not been risk-assessed |
| SOC 2 (CC3.2) | Risks to objectives are identified and analyzed | **Positive:** this document shows the risk process working. Acceptance does not remove the exceptions above, but it shows management identified and owned the risk. |
| CCPA (Civ. Code §1798.140) | Applicability | The company's revenue exceeds the adjusted $26,625,000 threshold, so CCPA applies. Vendor contacts' personal information is covered since the B2B exemption expired January 1, 2023. |
| CCPA (§1798.100(e), §1798.150) | Reasonable security; private right of action for breaches caused by unreasonable security | A documented, known, unremediated gap weakens any "reasonable security" defense. Statutory damages apply only to categories defined in §1798.81.5. A bank account number generally qualifies only when paired with an access code, so exposure depends on what invoices actually contain. **Legal review needed.** |
| CCPA (§1798.105, §1798.110) | Rights to delete and to know | Uncontrolled copies in partner-accessible folders make complete responses hard to guarantee |
| CCPA cybersecurity audit regulations (effective January 1, 2026) | Annual audit for businesses meeting significant-risk thresholds | Likely not triggered: as a B2B contractor, the company is unlikely to process personal information of 250,000 or more consumers, or sensitive personal information of 50,000 or more. Confirm annually. If triggered, first certification would be due April 1, 2028 (revenue over $100M). |
| Cross-border access | Partner staff located outside the United States can reach vendor personal information | CCPA does not ban access from abroad, but partners who receive personal information need contracts with CCPA-required terms (§1798.100(d)). Client contracts may also limit offshore access to their data. Recovery and investigation are harder across borders. **Legal to confirm which countries and what partner contracts say.** |
| Client contracts | Security and audit clauses requiring an unqualified SOC 2 Type II report | A qualified opinion could trigger notice, cure, or termination rights. Legal to review top client agreements. |

### 9.1 Legal Considerations

- **Authority to accept.** Most-likely losses exceed $1M, with a worst case over $20M. The CFO's delegated authority may not cover a risk of this size. Executive approval is required (Section 10), and the Audit Committee should be notified.
- **This document is discoverable.** After an incident, a signed acceptance of a known gap may be offered as evidence that the company knew and chose not to act. That is a reason for accuracy and follow-through, not a reason to avoid writing it down. Keep it under the records retention schedule, and do not edit it after signature except through versioned amendments.
- **Privilege.** Legal analysis of liability exposure (the §1798.150 and contract analysis above) should be prepared at the direction of counsel and kept separate from this business document, to preserve attorney-client privilege and work-product protection where available.
- **Meeting recordings.** California requires the consent of all parties to record confidential communications (Penal Code §632). It is standard practice here to record meetings like these on Microsoft Teams, so anyone who missed the meeting can catch up and so accurate notes can be kept. Everyone in the Appendix C meetings knew they were being recorded.

---

## 10. Signatures

### Risk Owner (Business)
By signing, I acknowledge that I understand the risk described above and accept responsibility for this risk on behalf of the organization.

| Field | Value |
|-------|-------|
| Name | |
| Title | Chief Financial Officer |
| Date | |
| Signature | |

### Security Review
By signing, I confirm that the risk has been accurately described and the assessment is complete.

| Field | Value |
|-------|-------|
| Name | |
| Title | Director of IT Security |
| Date | |
| Signature | |

### Compliance Review
By signing, I confirm that relevant compliance implications have been considered.

| Field | Value |
|-------|-------|
| Name | |
| Title | General Counsel |
| Date | |
| Signature | |

### Executive Approval (if required)
By signing, I approve this risk acceptance at the executive level.

| Field | Value |
|-------|-------|
| Name | |
| Title | Chief Executive Officer |
| Date | |
| Signature | |

---

## Version History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 0.1 | September 22, 2026 | Faith Olofintuyi | Initial draft |

---

## Appendices

### A. Technical Details

- **Storage:** Invoice drafts sit in "AP Draft Invoices," a subfolder of a ShareFile site originally set up for joint work with field partners, including partner staff located offshore. Permissions are inherited from the parent site.
- **Workflow:** InvoiceTrack (in-house, built in 2014) reads from and writes to these folders. The permission mapping between InvoiceTrack and the folder structure was built and maintained by one systems engineer, whose role was eliminated in the most recent reduction in force without a knowledge transfer.
- **Why separation is hard:** Breaking inheritance on the invoice folders without understanding InvoiceTrack's dependencies risks breaking the AP workflow. That is the source of the downtime and retraining concern.

### B. Reference Documents

| Document | Location |
|----------|----------|
| FBI IC3 2025 Internet Crime Report | https://www.ic3.gov/AnnualReport/Reports/2025_IC3Report.pdf |
| CPPA: Updated Monetary Thresholds in CCPA | https://www.cppa.ca.gov/regulations/cpi_adjustment.html |
| AICPA Trust Services Criteria (2017, revised points of focus 2022) | AICPA website |
| RA-2026-005: Key-Person Dependency | Risk register |
| Prior-year SOC 2 Type II report | Internal (restricted) |

### C. Meeting Minutes

All meetings were recorded on Microsoft Teams, as is standard for meetings like these, and every participant knew they were being recorded. Minutes are kept at the direction of the General Counsel.

| Date | Attendees | Summary |
|------|-----------|---------|
| July 14, 2026 | Risk & Compliance, Finance, Legal | The Risk & Compliance team presented the finding. Finance and Legal favored acceptance, citing cost and the low number of known incidents. |
| July 28, 2026 | Risk & Compliance, Engineering, Technical Support | Engineering agreed the risk is real but reported no capacity. Nobody on staff can safely change the permission model. |
| August 19, 2026 | C-suite, Risk & Compliance, Finance, Legal, Engineering | The Risk & Compliance team presented Options A to D and recommended Option B. The executive team declined A and B for budget and staffing reasons and directed acceptance with minimal improvements (Option D). The Risk & Compliance team asked that the decision be documented, time-limited, and reported to the Audit Committee. |
