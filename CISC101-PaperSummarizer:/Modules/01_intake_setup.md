- Purpose
Prepare and validate input data for summarization.

- Inputs
Full paper text or section-by-section text.

Target audience.

Summary level.

Evidence mode.

- Outputs
Normalized section titles.

Flags for missing sections.

Flags for sections < 50 words.

Chunked text for long papers.

- Workflow
Normalize section titles (e.g., Introduction, Methods, Results).

Detect missing sections → mark as “section missing.”

Detect sections < 50 words → flag with warning.

Apply context-window chunking for long papers.

Enforce PS2 constraints before passing to Section Loop.

- Rules & Constraints
No hallucinations.

No invented content.

Always enforce PS2 specification.