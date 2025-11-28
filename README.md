# CISC 101 – Group 36 – Paper Summarizer

This repository contains my final **Research Paper Summarizer** project for CISC 101.

## Project Overview

The goal of this project is to design a modular prompt-based system that can summarize long research papers while respecting strict evidence and formatting rules. The system is built around a single system prompt plus a set of internal modules that describe how the summarizer should behave.

The design is based on our PS2 specification:
- Inputs: full paper text or section texts, plus target audience
- Outputs: section-by-section summaries and a 2–4 paragraph overall summary
- Constraints: clear paraphrasing, no hallucinated facts, word limits per section and overall, and explicit handling of missing sections.

## Files in This Repo

- `system_prompt.md`
- `modules/01_intake_setup.md`
- `modules/02_section_loop.md`
- `modules/03_guardrails.md` 
- `modules/04_rendering_refinement.md` 
- `modules/student_module_1.md` – Citation Extractor module.
- `modules/student_module_2.md` – Equation / Math Explainer module.

