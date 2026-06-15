# Lease Abstractor — Claude Code Playbook

## What This Does
Extracts structured data from a commercial lease document and populates a standardized lease abstract Excel template. Uses a 2-pass extraction pipeline with supervisor validation and human review checkpoints.

## Trigger
When the user provides a lease document (PDF, DOCX, or images) and asks to analyze/abstract it, follow this workflow exactly.

## Output Location
The final lease abstract Excel file is saved **next to the source lease file** — in whichever folder the lease was provided from. If the user grants access to a folder containing leases (separate from the Abstraction Process folder), the output goes there. The filename should be `Lease Abstract — {Tenant Name}.xlsx`.

All intermediate working files (extracted text, JSON outputs, document context) are temporary and should be kept in the session working directory only — never saved into the Abstraction Process folder or the lease folder.

## Workflow

### Phase 0 — Ingest Document
1. Identify the file type in the user's upload
2. For **PDF**: extract text using PyMuPDF (`fitz`). If text extraction returns mostly empty/garbage, fall back to reading pages as images and using vision
3. For **DOCX**: extract text using `python-docx`
4. For **scanned images**: read directly as images, use vision for extraction passes
5. Store the extracted lease text as `lease_text` for use in all passes

### Phase 0.5 — Structural Read & Checkpoint
1. Read the ingested lease text and produce a short plain-text summary (3–6 sentences) describing:
   - What documents or sections are present (original lease, amendments, exhibits, addenda — whether in separate files or combined in one)
   - The order of information and which provisions supersede others where they conflict
   - Any documents or exhibits referenced but not provided
2. Present the summary to the user, e.g.:
   ```
   This is a single PDF containing the original lease (dated 6/15/23) followed by a
   First Amendment (dated 8/22/24) starting around page 12. The amendment modifies the
   rent escalation in §1.5 and extends the term by 2 years. I'll treat amendment terms
   as controlling where they conflict with the original.
   ⚠️ A Second Amendment is referenced in the First Amendment but is not included.
   ```
3. **Ask the user: "Does this look right? Anything missing or misread?"**
4. **Wait for user confirmation before proceeding**
5. If user corrects anything, revise the summary accordingly
6. Keep the confirmed summary in working memory as `document_context` — this is prepended to all subsequent passes (Pass 1, 2, and 3) so the model understands the information hierarchy before extracting terms

### Phase 1 — Pass 1: Facts Extraction
1. Read `preferences.md` in full and prepend its contents to the prompt
2. Read `prompts/pass1_facts.md`
3. Inject `lease_text` and `document_context` where indicated
4. Execute the prompt — output must conform to `schemas/pass1_schema.json`
5. Validate the output against the schema. If fields are missing, re-run with a targeted follow-up asking for the missing fields only
6. Hold result in working memory as `pass1_output`

### Phase 2 — Pass 2: Analysis Extraction
1. Read `preferences.md` in full and prepend its contents to the prompt
2. Read `prompts/pass2_analysis.md`
3. Inject `lease_text` and `document_context` where indicated
4. Execute the prompt — output must conform to `schemas/pass2_schema.json`
5. Validate the output against the schema
6. Hold result in working memory as `pass2_output`

### Phase 3 — Pass 3: Supervisor Validation
1. Read `preferences.md` in full and prepend its contents to the prompt
2. Read `prompts/pass3_supervisor.md`
3. Read `validation_rules.md` and inject where indicated in the prompt
4. Inject `lease_text`, `document_context`, `pass1_output`, and `pass2_output` where indicated
5. Execute the prompt — output must conform to `schemas/pass3_schema.json`
6. If corrections are returned, apply them to the relevant pass output
7. Merge pass1 + pass2 + corrections into `validated_extraction` (held in working memory)
8. Store `reviewer_flags` from pass3 output — this is the flat list of all warnings, cross-validation findings, and [REVIEW]-tagged fields compiled by the supervisor

### Checkpoint B — Human Review
1. Present the validated extraction to the user in a readable summary format
2. Group the summary by template section (General Info, Lease Details, Financials, NNN Exceptions, Rent Schedule, Critical Dates, Key Clauses, Amendments, Contacts, Reviewer Flags)
3. Highlight any fields the supervisor flagged or corrected, including cross-validation findings grouped by issue group
4. Present the full list of reviewer flags — these will populate the Reviewer Flags section of the abstract
4. **Wait for user approval before proceeding to population**
5. If user requests changes, update the extraction and re-present

### Phase 4 — Template Population
1. Copy `templates/Final_Commercial_Lease_Abstract.xlsx` to the same folder as the source lease file, named `Lease Abstract — {Tenant Name}.xlsx`
2. Read `mapping.md` for cell-to-field mapping rules
3. Using openpyxl, populate the Lease Abstract sheet:
   - Write values to mapped cells
   - Preserve all existing formatting (fonts, fills, borders, alignment)
   - Do NOT overwrite label cells (column B and D) — only write to value cells (column C and E)
   - For text values, set wrap text alignment on cells that contain long content
4. Build the Rent Schedule sheet dynamically:
   - Read the escalation structure from validated extraction
   - Write data rows for the initial term
   - Write formula rows for escalation years (CPI, fixed bump, or stated amounts)
   - Write option period rows if renewal options exist
   - Add subtotal formulas per period block and a grand total
   - Set the SF and CPI/escalation assumption cells
5. Populate the Reviewer Flags section:
   - Write each entry from `reviewer_flags` into the flag rows (starting at row 63)
   - Column B = flag_type (bold), Column C = description (wrap text)
   - If more than 5 flags, insert additional rows per mapping.md row management rules
   - If zero flags, leave the section empty (header still visible)
6. Run `recalc.py` on the output file to calculate formula values
7. Verify zero formula errors
8. Present the completed file to the user

## Rules
- Before executing each extraction pass (Pass 1, 2, or 3), read `preferences.md` in full and prepend its contents to the prompt. Do not rely on earlier context — re-read the file every time. All formatting, citation, terminology, and length rules live in preferences.md.
- NEVER modify the template file in `templates/`. Always copy first.
- NEVER write to label cells (B and D columns in the paired-field rows). Only write to value cells (C and E).
- NEVER skip the human review checkpoint. Always present extraction results and wait for approval.
- For the rent schedule: use Excel formulas for calculated values, never hardcode computed numbers.
- NEVER save intermediate files (JSON, text) into the Abstraction Process folder or the lease folder. All working data stays in the session working directory.
- The only file written to the lease folder is the final populated Excel abstract.
