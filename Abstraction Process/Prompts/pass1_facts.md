# Pass 1: Facts Extraction

You are extracting factual, low-interpretation data from a commercial lease. Your job is to find specific values — names, dates, addresses, numbers. Do not interpret or analyze. If a value is not present in the lease, return `null` for that field.

**Before extracting any fields, read the preferences file above in full. All formatting, citation, terminology, and length rules apply to every value you produce. Every value goes into an Excel cell — not a paragraph.**

## Output Format
Return ONLY valid JSON conforming to the schema. No commentary, no markdown, no explanation.

## Fields to Extract

### general_info
- `tenant_name` — Full legal name of the lessee/tenant
- `landlord_name` — Full legal name of the lessor/landlord
- `property_address` — Full street address including city, state, zip
- `property_description` — Name of the building, center, or complex
- `suite_unit` — Suite or unit number
- `guarantor` — Name of personal or corporate guarantor, if any
- `lease_type` — As stated in the lease (NNN, Modified Gross, Full Service, etc.)
- `broker` — Broker name(s), or "None" if lease states no broker

### lease_details
- `execution_date` — Date the lease was signed
- `commencement_date` — Lease commencement date
- `possession_date` — Date tenant takes possession
- `rent_commencement_date` — Date rent payments begin
- `expiration_date` — Lease expiration date
- `lease_term` — Stated term length (e.g., "5 Years")
- `space_type` — Retail, Office, Industrial, etc.
- `suite_unit_number` — Unit number (may duplicate suite_unit above)
- `leasable_area_sf` — Square footage as stated
- `proportionate_share` — Tenant's pro rata share percentage

### critical_dates
- `commencement_date` — Same as above, duplicated for this section
- `expiration_date` — Same as above
- `renewal_option` — Number and length of options, one line (e.g., "Four 5-yr options, CPI (§1.3)")
- `renewal_terms` — Dates for each option, semicolon-separated (e.g., "Opt 1: 2/1/30–1/31/35; Opt 2: ...")
- `renewal_notice` — Notice period only (e.g., "6 months prior written notice (§1.3)")
- `termination_option` — Early termination rights, if any
- `rofo_rofr` — Right of first offer/refusal, if any
- `expansion_option` — Expansion rights, if any

### contacts
- `tenant_contact` — Tenant entity name or contact
- `landlord_contact` — Landlord entity name + notice address only
- `guarantor_contact` — Guarantor name + address only
- `property_manager` — Property manager name or entity

---

## Self-Check (complete before returning output)

Review every value in your output against the preferences file:
1. **Length**: No value exceeds 2 lines. If it does, you have over-extracted — condense it.
2. **Citations**: Every value ends with a parenthetical reference per preferences.
3. **Terminology**: "N/A" vs "None" vs "Not addressed in lease" used per preferences definitions.
4. **No filler**: No procedural detail, no definitions, no boilerplate restatements.

If any value fails these checks, revise it before returning your output.

---

## DOCUMENT CONTEXT (confirmed by user)

The following summary describes how the lease document is structured and which provisions take precedence. When provisions conflict, extract values from the controlling document/section and flag the conflict with "[REVIEW]".

{document_context}

## LEASE TEXT

{lease_text}
