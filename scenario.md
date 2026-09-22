# Scenario: Inherited Access to Draft Invoices

> Author: Faith Olofintuyi
> Fictional scenario. The company, people, and figures are invented for this portfolio.

## The Organization
Sierra Meridian Field Services, Inc. (fictional) is an oil and gas operations contractor headquartered in Sacramento, California. It has 1,610 employees, about 70% of them remote, and about $285M in annual revenue. It pays about $140M a year to roughly 900 active vendors. The company holds a SOC 2 Type II report, which its largest clients require, and is subject to the California Consumer Privacy Act (CCPA).

## The Gap
External partners and their staff, some located offshore, can open and edit the ShareFile folders where draft invoices sit just before vendors are paid. The access comes through inherited permissions from a site originally set up for joint field work.

The main risk is payment fraud: someone using a partner's account could change a vendor's bank details, and the company would likely pay the wrong account. Data exposure is a second risk, because the drafts contain vendor bank details and contract rates. File audit logging is not turned on, so misuse would go unseen. In the last 12 months, AP staff caught two suspicious bank-change requests by chance.

## Why It Was Not Fixed
- The engineer who built the permission model was laid off without a handover. Nobody still at the company fully understands it.
- InvoiceTrack, the in-house AP system built in 2014, depends on the current folder structure. Changing permissions without mapping those links could break the AP workflow.
- Replacing InvoiceTrack would mean downtime, retraining, and possible vendor data loss.
- No budget was set aside this fiscal year, and Engineering reported no capacity.

## The Stakeholders
- **Finance and Legal** favored accepting the risk, citing cost and the small number of known incidents.
- **Engineering and Technical Support** agreed the risk is real but said they could not take on the work.
- **Executive team** made the final call on August 19, 2026 and chose Option D (payment checks and logging only) over the Risk & Compliance team's recommendation of Option B.
- **My role:** I was the neutral fact-finder on the Risk & Compliance team. I ran the discovery meetings, documented each one, and presented the options. When the executive team chose Option D, I asked that the decision be documented, limited to six months, and reported to the Audit Committee each quarter.

## The Constraints
- No new budget this fiscal year.
- No internal engineering capacity.
- InvoiceTrack has to keep running. AP cannot stop paying field vendors without delaying job sites.
- Partners still need the shared site for legitimate joint work, so it cannot simply be shut down.
- The next SOC 2 audit cycle, and clients who expect a clean report.
