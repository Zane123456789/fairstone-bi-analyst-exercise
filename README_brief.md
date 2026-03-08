
# Fairstone BI Analyst – Take‑Home Exercise (60 minutes)

**Scenario:** You’ve been given flat exports from different systems to produce a quick Q4 snapshot for the next exec meeting. The data contains known issues. Prioritise clarity over completeness.

**Your objective (deliverables):**
1. A **1‑page overview** (PDF/PPT/PNG) with 3–4 visuals answering the prompts below.
2. A **≤1 page ‘Assumptions & Approach’ note** describing KPI definitions, data issues found, and quick fixes.
3. **Up to 3 query/measure snippets** (SQL or DAX/Tableau LOD) that reproduce your headline numbers.

**Tools:** Tableau, Power BI, Excel, or SQL of your choice.

**Timebox:** ~60 minutes. If you stop early, add a short note: *“If I had more time, I would…”*

---

## Data files
- `clients.csv`
- `advisers.csv`
- `policies.csv`
- `transactions.csv`
- `leads.csv`
- `appointments.csv`

> Dates are mostly ISO (`YYYY-MM-DD`) but some files intentionally mix UK format (`DD/MM/YYYY`) to test data handling.

---

## Business prompts
1. **CFO:** What are **YTD revenue**, **Q4 FY2025 revenue vs target (£1,200,000)**, and **average fee %**?
2. **COO:** Are we improving **client cross‑sell** (≥2 active products per client)? Show breakdowns by client segment and adviser band.
3. **Compliance:** Flag any **data quality issues** that could misstate KPIs (e.g., duplicate clients, transactions before policy inception, Active policies with no recent transactions, missing segments, adviser mismatches). Recommend fixes and ownership.
4. **BI Lead:** Use **defensible KPI definitions**. Briefly document how you calculated them and any governance notes you’d add if this became a certified report (e.g., data dictionary, KPI doc, owner).

---

## Minimal data dictionary

### clients.csv
- `client_id` (PK), `client_name`, `dob`, `primary_adviser_id`, `onboarded_date`, `client_segment` (Retail/Private/Corporate, sometimes missing)

### advisers.csv
- `adviser_id` (PK), `adviser_name`, `region`, `band` (A/B/C), `start_date`

### policies.csv
- `policy_id` (PK), `client_id`, `product_type` (ISA/GIA/SIPP/Protection/Mortgage), `inception_date`, `status` (Active/Lapsed)

### transactions.csv
- `txn_id` (PK), `policy_id`, `txn_date`, `gross_amount`, `fee_amount`, `fee_type` (Initial/Ongoing), `is_refund` (Y/N)

### leads.csv
- `lead_id` (PK), `client_id` (nullable), `lead_source` (Referral/Web/Partner), `created_date` (mixed formats), `status` (New/Qualified/Converted/Lost)

### appointments.csv
- `appt_id` (PK), `client_id`, `adviser_id`, `appt_date` (mixed formats), `appt_type` (Review/Onboarding/Advice), `attended` (Y/N)

---

## KPI guidance (you can adapt with clear justification)
- **Revenue** = sum of `fee_amount` where `is_refund='N'`.
- **Average fee %** = sum(`fee_amount`) / sum(`gross_amount`) for the period (exclude refunds).
- **Q4 FY2025** = 1 Oct 2025 – 31 Dec 2025 (target £1,200,000).
- **Cross‑sell** = % of **active** clients with **≥2 Active** products (count distinct `product_type` where related policy `status='Active'`).

---

## Hints (optional)
- Be explicit about date parsing and refund treatment.
- Sanity‑check fee % outliers (>10%).
- Note any clients with duplicate IDs but different names, mismatched adviser IDs, or transactions before inception.
- You don’t need to fix everything—just show good judgement and clear communication.
