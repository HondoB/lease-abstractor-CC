# Cell Mapping — Lease Abstract Template

Maps validated JSON fields to Excel cells. Sheet: **Lease Abstract**

## Rules
- Only write to **value cells** (columns C and E)
- Never overwrite **label cells** (columns B and D)
- Preserve all existing formatting — do not modify fonts, fills, borders, or alignment
- Row B1: Replace `{Tenant Name}` with `general_info.tenant_name` in the title string

## General Information (Rows 4–7)

| Cell | JSON Path | Notes |
|------|-----------|-------|
| C4 | `general_info.tenant_name` | |
| E4 | `general_info.landlord_name` | |
| C5 | `general_info.property_address` | |
| E5 | `general_info.property_description` | |
| C6 | `general_info.suite_unit` | |
| E6 | `general_info.guarantor` | |
| C7 | `general_info.lease_type` | |
| E7 | `general_info.broker` | |

## Lease Details (Rows 10–14)

| Cell | JSON Path | Notes |
|------|-----------|-------|
| C10 | `lease_details.execution_date` | |
| E10 | `lease_details.commencement_date` | |
| C11 | `lease_details.possession_date` | |
| E11 | `lease_details.rent_commencement_date` | |
| C12 | `lease_details.expiration_date` | |
| E12 | `lease_details.lease_term` | |
| C13 | `lease_details.space_type` | |
| E13 | `lease_details.suite_unit_number` | |
| C14 | `lease_details.leasable_area_sf` | |
| E14 | `lease_details.proportionate_share` | |

## Lease Financials (Rows 17–22)

| Cell | JSON Path | Notes |
|------|-----------|-------|
| C17 | `lease_financials.rent_structure` | Wrap text |
| E17 | `lease_financials.base_rent_monthly` | Wrap text |
| C18 | `lease_financials.base_rent_annual` | Wrap text |
| E18 | `lease_financials.base_rent_psf` | |
| C19 | `lease_financials.rent_escalations` | Wrap text |
| E19 | `lease_financials.free_rent_concessions` | |
| C20 | `lease_financials.ti_allowance` | Wrap text |
| E20 | `lease_financials.additional_rent` | Wrap text |
| C21 | `lease_financials.percentage_rent` | |
| E21 | `lease_financials.security_deposit` | |
| C22 | `lease_financials.total_lease_value` | Wrap text |
| E22 | `lease_financials.late_fee` | Wrap text |

## NNN Exceptions / Landlord Cost Exposure (Rows 24–28)

| Cell | JSON Path | Notes |
|------|-----------|-------|
| C24 | `nnn_exceptions.nnn_classification` | Wrap text |
| E24 | `nnn_exceptions.cam_cap` | Wrap text |
| C25 | `nnn_exceptions.structural_roof` | Wrap text |
| E25 | `nnn_exceptions.capital_expenditures` | Wrap text |
| C26 | `nnn_exceptions.cam_exclusions` | Wrap text |
| E26 | `nnn_exceptions.mgmt_fee_markup` | Wrap text |
| C27 | `nnn_exceptions.insurance_exceptions` | Wrap text |
| E27 | `nnn_exceptions.utility_exceptions` | Wrap text |
| C28 | `nnn_exceptions.other_landlord_obligations` | Wrap text, spans conceptually across C-E |

## Rent Schedule Summary (Rows 32–38)
*Note: Rows shifted +7 due to NNN section insertion*

| Cell | JSON Path | Notes |
|------|-----------|-------|
| B33 | Dynamically built | Year period label, e.g., "Year 1 (Feb 2025 – Jan 2026)" |
| C33 | `rent_schedule.stated_rent_schedule[0].monthly_rent` | Numeric |
| D33 | `rent_schedule.stated_rent_schedule[0].annual_rent` | Numeric |
| E33 | `rent_schedule.stated_rent_schedule[0].rent_psf` | Numeric |
| B34–B37 | Dynamically built | Subsequent years — use escalation text if no stated amounts |

## Critical Dates (Rows 41–44)

| Cell | JSON Path | Notes |
|------|-----------|-------|
| C41 | `critical_dates.commencement_date` | |
| E41 | `critical_dates.expiration_date` | |
| C42 | `critical_dates.renewal_option` | Wrap text |
| E42 | `critical_dates.renewal_terms` | Wrap text |
| C43 | `critical_dates.renewal_notice` | Wrap text |
| E43 | `critical_dates.termination_option` | |
| C44 | `critical_dates.rofo_rofr` | |
| E44 | `critical_dates.expansion_option` | |

## Key Clauses (Rows 47–51)

| Cell | JSON Path | Notes |
|------|-----------|-------|
| C47 | `key_clauses.holdover` | Wrap text |
| E47 | `key_clauses.insurance` | Wrap text |
| C48 | `key_clauses.force_majeure` | Wrap text |
| E48 | `key_clauses.assignment_subletting` | Wrap text |
| C49 | `key_clauses.audit_rights` | Wrap text |
| E49 | `key_clauses.landlord_maintenance` | Wrap text |
| C50 | `key_clauses.alterations` | Wrap text |
| E50 | `key_clauses.landlord_work` | Wrap text |
| C51 | `key_clauses.indemnity` | Wrap text |
| E51 | `key_clauses.exclusive_use` | Wrap text |

## Amendments (Rows 53+)

Each amendment gets a sub-header row (bold, amendment name) followed by its summary text written directly into column B. The summary is already formatted as bullet-point text from the extraction — write it as-is. Wrap text on all amendment cells.

| Cell | JSON Path | Notes |
|------|-----------|-------|
| B54 | `amendments[0].amendment_name` | Sub-header style, bold |
| B55+ | `amendments[0].summary` | Write the summary text as-is into column B. Wrap text. |
| +2 rows | `amendments[1]` (if exists) | Repeat pattern for additional amendments |

### Row Management — CRITICAL

The amendment section may need more rows than the template provides between the Amendments header (row 53) and the Contacts section. When this happens:

1. **Count the total rows needed** for all amendment content (name row + summary rows + blank spacer per amendment)
2. **Insert rows BEFORE the Contacts section** to make room — use `ws.insert_rows()` at the correct position
3. **After inserting, recalculate the Contacts section row numbers** — do NOT hardcode rows 59-60
4. **Copy formatting** from adjacent amendment rows for any newly inserted rows (font, alignment, wrap text)
5. **Never write amendment content into Contacts label cells** (columns D) — amendment content goes in column B only
6. **Row heights**: Set all amendment data rows to 30-40pt to accommodate wrapped text. Do not let openpyxl auto-size.
7. **Leave a blank spacer row** between the last amendment row and the Contacts header

### Formatting Rules for Amendment Cells

- Column B only — amendments span B through E via merged cells or column B wrap
- Font: match the body font from the template (not bold, except amendment_name which is bold)
- Wrap text: True on all amendment data rows
- Do NOT inherit bold from the section header row

## Contacts (Dynamic — after Amendments)

| Cell | JSON Path | Notes |
|------|-----------|-------|
| C59 | `contacts.tenant_contact` | |
| E59 | `contacts.landlord_contact` | |
| C60 | `contacts.guarantor_contact` | |
| E60 | `contacts.property_manager` | |

## Reviewer Flags (Dynamic — after Contacts)

This section surfaces all supervisor findings so the human reviewer can see at a glance what needs attention. It populates from `validated_extraction.reviewer_flags` — a merged list of Pass 3 warnings, cross-validation flags, and any fields tagged with [REVIEW] during extraction.

### Template Structure

- **Row 61**: Spacer (matches other inter-section spacers)
- **Row 62**: Section header — "REVIEWER FLAGS"
- **Rows 63–67**: 5 pre-formatted flag data rows (placeholder layout)

### Data Layout

Each flag gets one row. For each row:

- **Column B**: Flag type — one of: "Warning", "Cross-Validation", "[REVIEW] Field". Bold label font.
- **Column C**: Flag description — plain-language finding. Include issue group (for cross-validation flags), the fields involved, and the source reference. Wrap text.

| Cell | JSON Path | Notes |
|------|-----------|-------|
| B63 | `reviewer_flags[0].flag_type` | Bold |
| C63 | `reviewer_flags[0].description` | Wrap text |
| B64+ | `reviewer_flags[1+]` | Repeat pattern for additional flags |

### Row Management

The template provides 5 placeholder rows (63–67). When there are more flags:

1. **Insert rows BEFORE the row after the last flag row** — use `ws.insert_rows()` at the correct position
2. **Copy formatting** from adjacent flag rows for any newly inserted rows (font, alignment, wrap text, alternating borders)
3. **Row heights**: Set all flag data rows to 24pt

When there are fewer flags than 5, leave remaining rows empty — do not delete them.

### Formatting Rules

- Column B: bold label font, left-aligned, vertical center
- Column C: regular value font, left-aligned, vertical center, wrap text
- Alternating bottom borders on odd-positioned flag rows (matching the pattern in other sections)
- Do NOT inherit bold from the section header row onto description cells

---

## Rent Schedule Sheet — Dynamic Build

Sheet: **Rent Schedule**

This sheet is built dynamically based on `rent_schedule` data. The template includes a pre-formatted layout with row styling, section divider bars, and a grand total row. The population logic must adapt this layout to fit the actual lease — the template structure is a starting point, not a constraint.

### Template Structure Reference

The template has this pre-built layout:

- **Row 1**: Title — "RENT SCHEDULE — {Tenant Name}"
- **Row 2**: Metadata — SF label (F2), SF value (G2), CPI/Escalation label (H2), escalation rate (I2)
- **Row 3**: Spacer
- **Row 4**: Column headers (Lease Year, Lease Year Detail, Period Start, Period End, Escalation Type, Escalation Detail, Base Rent $/SF/Yr, Monthly Base Rent, Annual Base Rent)
- **Rows 5–9**: Initial term block (5 pre-formatted data rows, alternating white/gray fill)
- **Row 10**: Section bar — dark gray (FF414042), bold white text. Use for initial term subtotal.
- **Rows 11–15**: Option 1 block (5 data rows)
- **Row 16**: Section bar — Option 1 subtotal
- **Rows 17–21**: Option 2 block
- **Row 22**: Section bar — Option 2 subtotal
- **Rows 23–27**: Option 3 block
- **Row 28**: Section bar — Option 3 subtotal
- **Rows 29–33**: Option 4 block
- **Row 34**: Section bar — Option 4 subtotal
- **Row 35**: Spacer
- **Row 36**: Grand total row — red (FFA31D20), bold white text

### Adapting the Layout

The template assumes 5 years initial + 4 option periods of 5 years each. Most leases won't match this. The population logic must resize the layout to fit.

**When the initial term has fewer than 5 years:** Populate only the rows needed. Leave remaining rows in the block empty — do not delete them or shift the section bar.

**When the initial term has more than 5 years:** Insert additional rows before the section bar (row 10). New rows must replicate the alternating fill pattern: odd data rows get white fill (FFFFFFFF), even data rows get gray fill (FFE8E9EA). All data rows get thin bottom borders, Calibri 10pt font, center alignment on columns A–E and G, wrap text on column F, and the currency number format ("$"#,##0.00) on column G. Columns H and I use formula-driven values. After inserting, the section bar and all subsequent rows shift down accordingly.

**When there are fewer option periods than 4:** Populate the option blocks that apply. For unused option blocks, leave them empty — do not write data or subtotals. Alternatively, if cleaner output is preferred, delete the unused blocks and shift the grand total row up.

**When there are more than 4 option periods:** After the last pre-formatted block, insert new blocks following the same pattern: data rows with alternating white/gray fill, followed by a section bar row (full-width FF414042 fill, bold white Calibri 10pt). Then reposition the grand total row (FFA31D20) after the last block.

**When there are no option periods:** Leave all option blocks empty or remove them. Position the grand total row immediately after the initial term subtotal bar.

### Header Area

- `A1`: "RENT SCHEDULE — {tenant_name}"
- `F2`: "SF:" (label)
- `G2`: leasable_sf (numeric)
- `H2`: "CPI Assumption:" or "Escalation:" (label, based on escalation type)
- `I2`: escalation_rate (numeric, e.g., 0.03 for 3%). Leave blank if CPI with no stated assumption per preferences.

### Data Rows

Each year of each period gets its own row. Option periods are broken out year by year, not summarized as a single row.

For each row:

- **Column A**: Sequential year number across the entire schedule (1, 2, 3... continuing through options)
- **Column B**: Period label — "Year 1", "Year 2" for initial term; "Opt 1, Yr 1", "Opt 1, Yr 2" for option periods
- **Column C**: Period start date
- **Column D**: Period end date
- **Column E**: Escalation type (e.g., "Initial", "CPI", "Fixed 3%", "Stepped", "FMV")
- **Column F**: Escalation detail + lease reference (e.g., "3% annual increase per Sec 4.2" or "Subject to CPI adjustment per Sec 4.3, capped at 5%"). Wrap text.
- **Column G**: Base Rent $/SF/Yr
  - Year 1: hardcoded from `initial_rent_psf`
  - Subsequent years with fixed escalation: formula `=G{prev}*(1+I$2)`
  - Stepped leases with stated amounts: hardcoded values per the schedule
  - CPI years: leave blank or state "CPI" per preferences (do not forecast)
  - Option years with stated rent: hardcoded
  - Option years at FMV: leave blank, note "FMV" in column E
- **Column H**: Monthly rent formula `=IF(G{row}="","",G{row}*G$2/12)`
- **Column I**: Annual rent formula `=IF(G{row}="","",G{row}*G$2)`

### Section Bars and Subtotals

Each section bar row serves as the subtotal for the block above it.

- **Column B**: Label — "Initial Term Subtotal", "Option 1 Subtotal", etc.
- **Column H**: SUM formula for the monthly rent column of that block, e.g., `=SUM(H5:H9)`
- **Column I**: SUM formula for the annual rent column of that block, e.g., `=SUM(I5:I9)`

Preserve the existing formatting on section bar rows — do not overwrite the fill, font color, or bold styling. Only write values to columns B, H, and I.

### Grand Total Row

- **Column B**: "GRAND TOTAL"
- **Column H**: Sum of all monthly subtotal cells, e.g., `=H10+H16`
- **Column I**: Sum of all annual subtotal cells, e.g., `=I10+I16`

Only reference subtotal rows that actually contain data. If only the initial term and one option exist, the grand total should only sum those two subtotals.

### Verification Checks

After building the rent schedule, run these checks before finalizing:

- Every subtotal formula range matches the actual data rows in its block — no rows missed, no empty rows accidentally included
- The grand total references exactly the subtotal rows that contain data — no more, no less
- Monthly rent × 12 equals annual rent for every row that has values
- $/SF/Yr × SF equals annual rent for every row (catches broken SF references)
- Year 1 $/SF matches `initial_rent_psf` from the extraction
- Sequential year numbering in column A has no gaps or duplicates
- Period start/end dates are contiguous — each year's start date follows the previous year's end date
- Escalation formulas reference the correct previous row (not a section bar or subtotal row)
- No subtotal or grand total formula references a row that contains no data
- The total number of initial term rows matches `initial_term_years` from the extraction
