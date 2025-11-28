- Purpose
Prevent hallucinations and enforce evidence rules.

- Inputs
Section summaries.

Evidence mode.

- Outputs
Verified summaries.

Standardized warnings.

- Workflow
Verify evidence against source text.

If evidence_mode = "strict":

Only include claims present in text.

If insufficient evidence → output: “The source text does not provide enough detail to summarize this section in strict evidence mode.”

Apply standardized warnings:

Missing section → “Section skipped: no usable text provided.”

Short section → “Section very short: summary may be incomplete.”

- Rules & Constraints
No hallucinations.

No invented citations.

Always enforce PS2 constraints.