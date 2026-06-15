# Abstraction Preferences

This file governs how extracted data should be presented and how judgment calls should be made. All prompts must follow these rules. Edit this file to reflect your firm's standards.

---

## Cell Value Format — CRITICAL

The purpose of an abstract is to synthesize, not transcribe. Every cell value must be the **shortest defensible statement** of the provision, followed by a short citation. The reader should grasp the term in under 5 seconds; the citation tells them where to verify.

**Hard rule: No value may exceed 2 lines. Amendments are the only exception.**

### Structure

```
{core value} ({short cite})
```

### Length Rules

- **Fact fields** (names, dates, dollar amounts, SF, percentages): Value only + short cite. One line. No explanation.
  - GOOD: `$7,597.33/mo (§1.4)`
  - BAD: `$7,597.33/mo (equal to one month Base Rent), non-interest bearing, held for performance security. Lessor may apply to damages. If premises sold, Lessor may transfer to purchaser (Sec 1, Para 6)`

- **Financial/expense fields** (rent structure, additional rent, NNN items): Lead with the key economic terms — dollar amounts, percentages, split. Use semicolons to separate sub-items. Max 1-2 lines.
  - GOOD: `Pro rata (25.3%) of taxes, insurance, CAM + 15% admin, shared utilities (§1.7-8)`
  - BAD: `Lessee's proportionate share of: (1) ad valorem and non-ad valorem taxes on Premises/Building/Site; (2) all insurance premiums including CGL, liability, flood, property casualty; (3) Common Area Maintenance including operating, cleaning, repairing...`

- **Key clauses** (holdover, insurance, force majeure, etc.): State the operative rule and key numbers. Lead with the number/rule, not "If Lessee..." No procedural detail. Max 1-2 lines.
  - GOOD: `200% of current rent + damages; no waiver of LL rights (§2.5)`
  - BAD: `If Lessee remains in possession after lease termination (expiration or otherwise), Rent increases to 2× the most recent monthly Rent amount, computed daily, plus all damages sustained by Lessor. Receipt of Rent after expiration does not waive Lessor's rights or remedies.`

- **NNN exceptions**: State who pays what. No quoting the maintenance matrix verbatim.
  - GOOD: `LL: foundation, roof, ext. walls, gutters, slabs, parking, shared systems (Ex. D)`
  - BAD (right length, wrong content): `LL responsible per Exhibit D Maintenance Matrix; Lessee responsible for systems exclusively serving Premises; shared systems maintained by LL (Ex. D)` — describes the structure instead of listing the items

### What to Omit

- Procedural mechanics (how notice is delivered, who holds deposit, transfer conditions)
- Definitions the reader already knows (don't define "NNN", "CAM", "CGL")
- Restatements of standard lease boilerplate
- Conditional language ("if...then") unless the condition is financially material
- How a provision is enforced — just state the rule

### What to Keep

- Dollar amounts, percentages, multipliers, limits, caps, floors
- Who pays / who is responsible
- Timeframes that create deadlines or financial exposure
- Material exceptions or carve-outs
- [REVIEW] flags on ambiguities

---

## Citations

Every value must end with a parenthetical reference so the reader knows where to verify it. Use whatever numbering the lease itself uses — don't translate into a different convention. Condense long references to the shortest unambiguous form (e.g., "Article XIV, Section 14.2(b)(iii)" → "(Art. XIV, §14.2(b)(iii))"). If a value draws from multiple locations, cite all of them separated by semicolons. If the location cannot be identified, state "(location not confirmed)" — never fabricate a reference.

---

## Terminology

- Use **"N/A"** when a provision category does not apply to this lease type (e.g., percentage rent on an industrial lease).
- Use **"None"** when the lease explicitly states there is none (e.g., "No broker was involved").
- Use **"Not addressed in lease"** when the lease is silent on a topic. Do not assume intent from silence.
- Use **"[REVIEW]"** prefix when a provision is ambiguous, contradictory, or requires human interpretation. Example: "[REVIEW] Amendment references Exhibit B but Exhibit B is not attached."

---

## Number & Date Formatting

- Dates: `2/1/25`
- Dollar amounts: comma separators, two decimal places for monthly rent. `$7,597.33/mo`
- Square footage: `2,849 SF`
- Percentages: one decimal place. `25.3%`

---

## Extraction Behavior

- If a clause is absent from the lease, state "Not addressed in lease" — do not infer market-standard meaning or fill in what a typical lease would say.
- If one clause supports multiple interpretations, give the narrowest supportable statement and flag with [REVIEW].
- If entity name differs across documents (e.g., tenant name in original lease vs. amendment), preserve both and flag — do not collapse to one.
- When the lease contains conflicting provisions (e.g., two different commencement dates), extract both and flag with "[REVIEW]" noting the conflict.
- If payment address differs from notice address, store each in its respective field separately. Do not assume they are the same.
- If maintenance obligations are split across multiple exhibits or riders, extract each obligation individually — do not summarize as a generic NNN responsibility.
- Do not editorialize or add commentary. Extract and synthesize only.

---

## NNN Exceptions

- Always evaluate NNN exceptions regardless of lease type label. Even if the lease says "NNN," review maintenance, CAM, taxes, insurance, and expense provisions for landlord-retained obligations.
- If the lease is a true NNN with no exceptions, state: "None — tenant responsible for all operating expenses per [section reference]"
- Do not characterize the lease as "modified" or "leaky" — just list the specific exceptions and let the reviewer draw conclusions.

---

## Amendments

Amendments are the **one exception** to the 2-line limit. All base fields in the abstract should reflect current/effective terms (the document hierarchy handles which document wins). The amendments section captures the change history so the reader knows what moved.

- If an amendment is referenced but not provided, flag it: "[REVIEW] Second Amendment referenced but not provided for review."
- Consolidate identical changes into one line (e.g., three exhibits with the same address update → one bullet).

### Format

```
First Amendment (8/22/24):
• Term Commencement: Not stated → 2/1/25 (1st Amd., Ex. B)
• Rent Commencement: Not stated → 2/1/25 (1st Amd., Ex. B)
• Premises Condition: Original Ex. B → replaced; As-Is, no LL improvements (1st Amd., Ex. B)
• Option Notices (Ex. E/F/G): LL address 505 S. Fifth St. → 616 E Green St. Ste. 203, Champaign, IL 61820 (1st Amd., Ex. E/F/G)
Surviving: All other terms unchanged; Ex. H (4th Option) not addressed [REVIEW]
```

---

## Rent Schedule

- **CPI-adjusted years**: Do not forecast or assume a CPI rate. For years where rent is subject to CPI adjustment, state: "Subject to CPI adjustment per [section reference]." Only populate dollar amounts for years with explicitly stated rents in the lease.
- **Fixed escalation years**: If the lease states a fixed percentage or dollar bump, those may be calculated and shown.
- **Renewal option rents**: Do not project renewal period rents unless the lease states specific renewal rates or a defined formula. If the renewal says "fair market value" or "to be negotiated," state that — do not estimate.
- **CPI assumption cell on Rent Schedule sheet**: Leave blank unless the user provides a rate. Do not default to 3% or any assumed rate.
