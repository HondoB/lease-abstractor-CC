# Cross-Field Validation Rules

This file defines cross-field validation logic for the Pass 3 supervisor. After completing field-level accuracy checks, the supervisor must review extracted outputs as integrated issue groups — checking whether related fields tell a coherent, internally consistent story.

## How to Use

For each issue group, review the relevant fields together as a cluster. Look for contradictions between fields, gaps where one field implies something another field doesn't address, and ambiguities in the source documents that the extraction should have surfaced but didn't.

## Reporting Findings

When a cross-validation check surfaces a concern, report it with:

- **Issue group** — which group the concern falls under
- **Finding** — plain-language description of the inconsistency or gap
- **Fields involved** — which extracted fields are in tension
- **Source references** — where in the lease the relevant provisions appear
- **Nature of concern** — whether this is a contradiction between extracted fields, a gap where one field implies something another doesn't address, or an ambiguity in the source documents that should have been flagged

Do not resolve ambiguities silently. If source documents are genuinely unclear or contradictory, preserve that uncertainty.

---

## Issue Groups

### Group 1: Parties / Entity Authority / Guarantor

Confirm tenant, landlord, and guarantor names are consistent across all fields. If the tenant is an entity, check whether signing authority is addressed. If a guarantor is listed, confirm the guaranty scope is captured. If amendments changed any party name, confirm the extraction reflects the current party and explains the chain.

### Group 2: Term / Commencement / Rent Commencement / Opening

Commencement plus stated term should equal the expiration date. If rent commencement differs from commencement, the gap should be explained by a fixturing period, free rent, or contingent trigger elsewhere in the extraction. If amendments modified the term, confirm the expiration reflects the amended term.

### Group 3: Base Rent / Percentage Rent / Escalations / Option Rent

Initial base rent in financials should match Year 1 of the rent schedule. Escalation descriptions should be consistent between the summary field and the schedule detail. If percentage rent exists, confirm the breakpoint is captured. If renewal options exist, confirm whether option rent is stated, FMV, or formula-based, and that the rent schedule reflects it.

### Group 4: CAM / Taxes / Insurance / Utilities / Admin Fees / Exclusions

If the lease is classified as NNN, the extraction should show the tenant paying CAM, taxes, and insurance — any exclusions should be documented or the classification questioned. If there's a CAM cap, check whether the admin fee sits inside or outside the cap. If a pro rata share is stated, it should be consistent with the tenant's SF and building SF if available.

### Group 5: Repair / Maintenance / Roof / Structure / Systems

Maintenance allocation should be consistent with the NNN classification from Group 4. If the extraction says the landlord handles roof and structure, confirm that's reflected in how capital expenditures are characterized. If an amendment shifted maintenance responsibilities, confirm the extraction reflects the current allocation.

### Group 6: Assignment / Subletting / Transfer / Change of Control

Confirm the extraction distinguishes between assignment, subletting, and change of control — they often have different consent requirements. If there's a profit-sharing or recapture right, confirm it's captured. If amendments modified transfer rights, confirm the extraction reflects current terms.

### Group 7: Use / Exclusives / Radius / Co-Tenancy / Continuous Operation

If the tenant has an exclusive, its scope should be consistent with the permitted use. If co-tenancy provisions exist, confirm triggers and remedies are captured. If there's a continuous operation requirement, check whether it conflicts with any termination or go-dark rights elsewhere.

### Group 8: Casualty / Condemnation / Default / Remedies / Termination

If termination rights exist on casualty or condemnation, confirm the thresholds are captured. Default provisions should have cure periods captured for both monetary and non-monetary defaults. If there's an early termination option, confirm the conditions, notice period, and any fee are all present and consistent.

### Group 9: Notice / Option Exercise / Estoppel / SNDA / Subordination

Option exercise deadlines should be consistent with critical dates — renewal notice deadlines should appear in both places. If amendments changed notice addresses or methods, confirm the extraction reflects current requirements. If SNDA provisions exist, confirm they're consistent with subordination terms.

---

## Cross-Field Validation Rules

These rules apply across all issue groups during validation.

- If an amendment changes only specified exhibits, do not assume all related schedules or provisions shifted — verify each field against the amendment's actual scope.
- If the extracted rent schedule implies escalation logic that contradicts the escalation summary field, flag the discrepancy.
- If the NNN classification contradicts specific landlord obligations found elsewhere in the extraction, flag it.
- If a termination option is extracted but no corresponding notice deadline appears in critical dates, flag the gap.
- If an abstract conclusion depends on reconciling provisions across multiple documents, note which documents were reconciled.
- If conflicting values exist across documents, verify the extraction preserved all values and flagged with [REVIEW] rather than silently picking one.
