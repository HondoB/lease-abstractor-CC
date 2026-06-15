# Pass 2: Analysis Extraction

You are extracting financial terms, expense structures, escalation mechanics, key clauses, and amendments from a commercial lease. This requires interpretation — you must read provisions carefully and synthesize their economic impact. If a provision is not present, return `null`. If a provision exists but is ambiguous, extract what is stated and note the ambiguity.

**Before extracting any fields, read the preferences file above in full. All formatting, citation, terminology, and length rules apply to every value you produce. Every value goes into an Excel cell — not a paragraph. Lead with numbers and rules, not "If Lessee..."**

## Output Format
Return ONLY valid JSON conforming to the schema. No commentary, no markdown, no explanation.

## Fields to Extract

### lease_financials
- `rent_structure` — Lease type + what tenant pays beyond base rent. One line. (e.g., "NNN — base rent + pro rata OpEx (§1.7)")
- `base_rent_monthly` — Dollar amount only (e.g., "$7,597.33/mo")
- `base_rent_annual` — Dollar amount only
- `base_rent_psf` — $/SF/yr only
- `rent_escalations` — Mechanism + rate/cap/floor if stated. One line.
- `free_rent_concessions` — Amount/period or "None"
- `ti_allowance` — Dollar amount or "None — As-Is"
- `additional_rent` — Pro rata share % + list of pass-through categories. One line.
- `percentage_rent` — Rate + breakpoint or "N/A"
- `security_deposit` — Dollar amount + key terms (interest-bearing?) only
- `total_lease_value` — Dollar amount + term + what's excluded. One line.
- `late_fee` — Rate + admin fee + returned check fee. One line.

### rent_schedule
- `escalation_type` — "CPI", "Fixed Percentage", "Fixed Dollar", "Stepped", or "Hybrid"
- `escalation_rate` — Specific rate if fixed (e.g., "3%" or "$0.50/SF/yr")
- `cpi_index` — Which CPI index if CPI-based (e.g., "CPI-U")
- `cpi_cap` — Annual or cumulative cap on CPI increases, if any
- `cpi_floor` — Minimum annual increase, if any
- `initial_rent_psf` — Starting $/SF/yr for Year 1
- `leasable_sf` — Square footage (for formula building)
- `initial_term_years` — Number of years in initial term
- `option_periods` — Array of option periods, each with: label, years, escalation_type, escalation_detail
- `year_1_start` — Start date of Year 1
- `year_1_end` — End date of Year 1
- `stated_rent_schedule` — Array of any explicitly stated rent amounts by year. Each entry: year, monthly_rent, annual_rent, rent_psf. Only include years where the lease states specific dollar amounts (not CPI-adjusted projections)

### nnn_exceptions
- `nnn_classification` — Stated lease type + one-line reality check. (e.g., "NNN per lease; confirmed by expense provisions (§1.7)")
- `cam_cap` — Cap amount/formula or "No cap stated [REVIEW]"
- `structural_roof` — Who pays: list items by party. Do NOT quote the maintenance matrix verbatim.
- `capital_expenditures` — Who pays + threshold/amortization method or "Not addressed"
- `cam_exclusions` — List excluded items or "Not explicitly stated"
- `mgmt_fee_markup` — Percentage + what it applies to. One line.
- `insurance_exceptions` — LL-retained coverage or unusual splits only. One line.
- `utility_exceptions` — Who pays what: exclusive vs shared. One line.
- `other_landlord_obligations` — List of LL-retained items beyond structural. One line.

### key_clauses
Each key clause must be 1 line (2 lines max for insurance). Lead with the rule/number, not "If Lessee...":
- `holdover` — Rent multiplier + consequences. (e.g., "200% of current rent + damages (§2.5)")
- `insurance` — Coverage types + limits + add'l insured status. Semicolons between items. (e.g., "CGL $1M/$2M; Dram Shop $2M; property all-risk; LL add'l insured (§2.3)")
- `force_majeure` — What it excuses + carve-outs. (e.g., "Excuses performance except rent payment (§2.29h)")
- `assignment_subletting` — Consent requirement + fee. (e.g., "LL consent required; $500 review fee; tenant remains liable (§2.6)")
- `audit_rights` — Timeframe + scope. (e.g., "30 days after reconciliation; 10 BD notice (§1.8b)")
- `landlord_maintenance` — What LL maintains. List items. (e.g., "Structural + shared HVAC/elec/plumbing/fire (§2.12)")
- `alterations` — Consent requirement + key thresholds. (e.g., "LL approval required; >$2,500 requires surety bond (§2.9)")
- `landlord_work` — What LL builds or "As-Is"
- `indemnity` — Direction + scope in brief. (e.g., "Tenant indemnifies LL: possession, acts, breach + environmental (§2.23; §2.15c)")
- `exclusive_use` — Permitted use + prohibited uses. (e.g., "Mexican restaurant only; no cannabis/alcohol w/o consent/gambling (§2.9-10)")

### amendments
Array of amendments. Each is a plain-text summary — **not** a structured diff. The base fields above already reflect current/effective terms per the document hierarchy. The amendment summary gives the reader context on what changed.

For each amendment:
- `amendment_name` — "First Amendment", "Second Amendment", etc.
- `summary` — Bullet-point text: what changed (original → amended with cite), what was replaced vs. what survives, and [REVIEW] flags for anything unresolved. Follow the amendment format example in preferences.md.

---

## Self-Check (complete before returning output)

Review every value in your output against the preferences file:
1. **Length**: No value exceeds 2 lines (exception: amendments). If it does, you have over-extracted — condense it.
2. **Citations**: Every value ends with a parenthetical reference per preferences.
3. **Terminology**: "N/A" vs "None" vs "Not addressed in lease" used per preferences definitions.
4. **No filler**: No procedural detail, no definitions, no boilerplate restatements. Lead with the number/rule, not "If Lessee..."
5. **NNN exceptions**: Evaluated regardless of lease type label, per preferences.

If any value fails these checks, revise it before returning your output.

---

## DOCUMENT CONTEXT (confirmed by user)

The following summary describes how the lease document is structured and which provisions take precedence. When provisions conflict, extract values from the controlling document/section and flag the conflict with "[REVIEW]".

{document_context}

## LEASE TEXT

{lease_text}
