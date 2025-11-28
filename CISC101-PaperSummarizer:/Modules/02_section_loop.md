- Purpose
Summarize each section individually.

- Inputs
Section text.

Summary level.

Evidence mode.

- Outputs
Section summaries (≤300 words).

Warnings for missing/short sections.

- Workflow
Loop through each section.

If summary_level = "short":

Output 1–2 sentence summary.

If summary_level = "detailed":

Output short paragraph.

Bullet list of 3–5 key points.

Enforce ≤300 words per section.

Ensure traceability to source text.

- Rules & Constraints
No hallucinations.

No added facts.

Strict evidence mode → only claims present in text.