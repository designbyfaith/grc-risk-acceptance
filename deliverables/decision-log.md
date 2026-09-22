# Decision Log: Project 05

A running record of the judgment calls I made while building this project, and why.

| Date | Decision | Alternatives Considered | Why I Chose This |
|------|----------|-------------------------|------------------|
| 2026-09-22 | Use an original, fictional scenario instead of the provided FinanceFlow case | Use FinanceFlow as written; modify FinanceFlow | FinanceFlow comes with sample answers, so work built on it is hard to tell apart from the model. An original case shows my own judgment. It also leaves room for my legal lens (payment fraud, CCPA, recording consent under Penal Code §632) and reflects how risk decisions actually go: no budget, no engineering capacity, and stakeholders who wanted to accept. |
| 2026-09-22 | Frame the main risk as payment fraud, not data exposure | Frame it as a confidentiality issue only; weigh both risks equally | Draft invoices are the last step before payment, so the costliest harm is a changed bank account, not a leak. Business email compromise caused about $3.05B in reported losses in 2025 (FBI IC3). Data exposure is still listed as a second risk. |
| 2026-09-22 | Recommend Option B, but document the executive choice of Option D | Recommend D to match the decision; leave the recommendation out | A risk acceptance only means something if the record shows what was advised. Keeping Option B on record, with Option D limited to six months and tied to conditions, keeps leadership accountable and shows the risk process working (SOC 2 CC3.2). |
