# Diagnosis-First Editor (Claude Plugin)

A rigorous, four-pass diagnostic editor designed for Claude Code, Codex, and agentic IDEs. It audits drafts for factual sourcing, synthetic surface habits, overly tidy structural skeletons, and author voice alignment before making targeted line edits.

## Features
- **Pass 1 (Truth & Sourcing):** Quarantines unverified or scope-inflated claims inside `[VERIFY SOURCE: ...]` tags.
- **Pass 2 (Surface Habits):** Flags negative parallelisms, inflated copulas, participial (`-ing`) tails, and triad puffery.
- **Pass 3 (Structural Shape):** Catches double-tap explanations, frictionless causality, and bookend conclusions, enforcing a **Structural Bailout Threshold (>70% failure rate)**.
- **Pass 4 (Author Voice):** Derives 3 mechanical rules from real writing samples (cadence, contractions, asides) and applies them exclusively to verified prose.

## Installation & Usage
Place this folder into your Claude plugins directory or marketplace repository, or load `SKILL.md` directly into your custom instructions / agent workspace.
