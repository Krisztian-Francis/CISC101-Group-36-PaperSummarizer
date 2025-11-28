# Module 02 – Section Loop

## Change Log
- 2025-11-28: Added `summary_level` variable ("short" vs "detailed") and conditional behavior for each section.
- 2025-11-28: Clarified word-limit rules and re-stated no-hallucination and evidence constraints for all section summaries.

---

## Purpose

Module 02 controls the **main loop over paper sections**.  
For each section it:
- decides how detailed the summary should be,
- generates the appropriate summary,
- enforces the PS2 constraints (word limits, no added facts, clear paraphrasing),
- and hands structured results to later modules.

This module is where the summarizer actually “does the work” of summarizing each section.

---

## Inputs

From Module 01 (Intake & Setup) this module receives:

- `sections_normalized`:  
  A list of section objects, each with:
  - `section_id`
  - `section_title`
  - `section_text`
  - `status` flag (e.g., `"ok"`, `"missing"`, `"too_short"`)

- `target_audience`: `"expert"`, `"mixed"`, or `"lay"`.

- `summary_level`:  
  A required variable that controls how much detail to produce for **each** section.  
  Allowed values:
  - `"short"`
  - `"detailed"`

- `evidence_mode`: passed through to support guardrails (e.g., `"normal"` or `"strict"`).

---

## Outputs

For each valid section, this module produces a **section summary record** containing:

- `section_id`
- `section_title`
- `summary_level_used`
- `section_summary_text`
- (if `summary_level = "detailed"`) `key_points` = bullet list of 3–5 items
- any flags (e.g., `"too_short_warning"`, `"evidence_warning"`)

These records are collected into a list, which is passed to:

- Module 03 (Guardrails) for additional checks, and
- Module 04 (Rendering & Refinement) for building tables and final outputs.

---

## Workflow

1. **Receive normalized section list and configuration.**

2. **Loop over each section** in `sections_normalized`:
   - If `status = "missing"`:  
     - Do not generate a summary.  
     - Attach a note for Module 03 / Module 04 that the section is missing.
   - If `status = "too_short"` (e.g., < 50 words):  
     - Generate a summary anyway (to the best of the model’s ability).  
     - Mark this section so that Module 03 can attach a warning.

3. **Apply `summary_level` logic**:
   - If `summary_level = "short"`:
     - Generate a **1–2 sentence summary** for the section.
     - Focus only on the most important idea (what the section is mainly doing).
     - Keep the total length comfortably within the 300-word per-section limit.
   - If `summary_level = "detailed"`:
     - Generate a **short paragraph summary** (still ≤ 300 words for the entire section).
       - Explain the role of the section in the paper.
       - Preserve key methods, results, and claims without adding new facts.
     - Then generate a **bullet list of 3–5 key points** summarizing:
       - main contributions or ideas,
       - important methods or equations,
       - important results or conclusions.

4. **Apply audience adaptation** (lightweight):
   - For `target_audience = "expert"`:
     - Allow technical terminology.
   - For `target_audience = "mixed"`:
     - Balance technical accuracy with occasional clarification.
   - For `target_audience = "lay"`:
     - Prefer plain language and short sentences, but do **not** invent new explanations that are not in the text.

5. **Enforce constraints for each section**:
   - Do not exceed **300 words** total for the section summary text (paragraph + bullets combined).
   - Do not add new facts, equations, or results beyond what appears in `section_text`.
   - If evidence seems weak or incomplete, set a flag so Module 03 can add a warning.

6. **Emit structured record** for each section:
   - Attach `section_id`, `section_title`, `summary_level_used`, `section_summary_text`, `key_points` (if any), and flags.
   - Append each record to a `section_summaries` list.

7. **Return `section_summaries`** to be processed by Module 03 and Module 04.

---

## Rules & Constraints

- Must always respect the PS2 specification:
  - Clear, simple paraphrasing.
  - Preservation of core methods and findings.
  - No hallucinated or speculative claims.
  - Per-section limit: **≤ 300 words**.
- Must obey `summary_level` exactly:
  - `"short"` → **only** 1–2 sentences (no bullet list).
  - `"detailed"` → short paragraph **plus** 3–5 bullet points.
- If `section_text` is empty or missing, no summary should be generated; instead, rely on Module 03 to produce standardized warnings.
- All summaries should be written so that Module 04 can reuse them in the final paper summary, tables, and expert/lay variants.
