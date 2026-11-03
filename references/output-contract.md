# Output Contract & Formatting Schema (`references/output-contract.md`)

This document enforces the exact sequence, structural schema, placeholder syntax, and edge-case behavior for the five-part output of the `diagnosis-first-editor` skill (v1.1.0).

**Global Output Rules:**
1. **Strict Ordering:** Output Sections 1 through 5 in exact numerical order. Never merge sections, skip a section, or add introductory/closing conversational filler outside the schema.
2. **Cross-Section Synchronization:** Every `Proposed Revision` in Section 3 MUST appear verbatim in Section 4 (`Revised Draft`). Every bracketed placeholder in Section 4 MUST correspond to a question in Section 2 (`Human-Only Questions`) or an item in Section 5 (`Integrity Check`).
3. **No Silent Rewrites or Silent Cuts:** If a sentence in Section 4 differs from the user's original draft by more than punctuation, or if any sentence/paragraph is removed, that change MUST have a corresponding diagnostic entry in Section 3 and a visible inline tag in Section 4.

---

## Pre-Header: Context & Voice Status Block
Before Section 1, output a compact status block confirming intake constraints:

```markdown
> **Target Reader & Outcome:** [Stated or explicitly inferred in 1 sentence]
> **Sourcing Baseline:** [Sources provided (N items) | No sources provided — all empirical claims treated as Unsupported and quarantined]
> **Voice Baseline:** [3 rules derived from samples | Limited Mode — no samples provided; defaulting to plain, direct prose]
```
*(If Voice Baseline is in full mode, list the 3 derived mechanical voice rules as concise sub-bullets inside this block before proceeding to Section 1.)*

---

## Section 1: Five Highest-Impact Issues (Ranked)

### Schema
Rank the most damaging flaws across Passes 1–4 by how severely they undermine reader trust or the desired outcome.

```markdown
## 1. Five Highest-Impact Issues (Ranked)

1. **[Pass X: Issue Category] — [Location: Paragraph # / Section Name]**
   - **Failure:** [1–2 sentence concrete diagnosis citing the exact words/claims at fault.]
   - **Impact:** [Why this breaks credibility, clarity, or structural momentum for the target reader.]
```

### Edge-Case Rules
- **Fewer than 5 issues exist:** Do NOT pad the list with nitpicks or invented flaws. List only the real issues found (e.g., 1–3) and append the explicit declaration: `"I have looked for additional weaknesses and I cannot find five distinct high-impact issues."`
- **Cluster Consolidation:** If the same surface habit (e.g., superficial `-ing` tails) appears multiple times across the draft, rank the *systemic pattern* as a single high-impact issue rather than burning multiple slots on the same habit.

---

## Section 2: Human-Only Questions

### Schema
Group every question that requires lived experience, proprietary metrics, unstated caveats, or editorial intent that an AI is forbidden to invent.

```markdown
## 2. Human-Only Questions

1. **[Q1 — Tied to Para # / Pass X]:** [Direct, specific question asking for a concrete fact, failure mode, metric, or decision?]
2. **[Q2 — Tied to Para # / Pass X]:** [Direct, specific question...]
```

### Edge-Case Rules
- **1-to-1 Placeholder Mapping:** Every `[ADD YOUR REAL EXAMPLE: Q# — ...]` tag injected into Section 4 must map directly to a numbered question in Section 2.
- **Zero Questions Needed:** If the draft is fully sourced and structurally complete, output: `"None. All factual claims and experiential examples in the draft are grounded in the provided inputs."`

---

## Section 3: Targeted Diagnostic Ledger & Edits

### Schema
List every failing passage chronologically by paragraph. Omit sentences and paragraphs that pass all four audits cleanly.

```markdown
## 3. Targeted Diagnostic Ledger & Edits

### [Paragraph # or Heading Name]
- **Pass & Flag:** `Pass [1|2|3|4]` — `[Specific Taxonomy Label, e.g., Scope Inflation / Negative Parallelism / Double-Tap Explanation / Voice Cadence]`
- **Original:** "[Exact verbatim quote from the draft]"
- **Reason:** [Specific analytical reason why the passage fails, referencing the source discrepancy, structural redundancy, or derived voice rule.]
- **Revision:** "[Exact replacement text, including any mandatory inline tags]"
```

### Edge-Case Rules
- **Long-Draft Token Guard (>800 words):** If a single paragraph contains 3+ distinct failures, combine them into one ledger entry per paragraph (`Pass & Flag: Multiple [Pass X + Pass Y]`), quote the full paragraph once, itemize the reasons briefly, and provide the unified paragraph revision.
- **Pass 1 Quarantine Priority:** When a sentence fails both Pass 1 (Truth) and Pass 2/4 (Surface/Voice), enforce the **Unverified Claim Quarantine** first—never polish the style or voice of an unverified claim without quarantining it inside **`[VERIFY SOURCE: ...]`**.

---

## Section 4: Revised Draft

### Schema
Output the complete draft with all Section 3 revisions and structural moves applied, leaving working original lines untouched.

```markdown
## 4. Revised Draft

[Full text of the revised draft with mandatory inline bold tags]
```

### Strict Placeholder & Structural Tag Syntax
Only five inline intervention markers are permitted inside Section 4. Render them in **bold** so the author can locate them immediately via `Ctrl+F`:
1. **`[VERIFY SOURCE: <exact claim requiring proof>]`** — Mandatory when an empirical, statistical, or historical statement has no backing in `sources` or exceeds the cited source's scope.
2. **`[ADD YOUR REAL EXAMPLE: Q# — <direct question for the author>]`** — Mandatory when a claim needs a lived anecdote, trade-off, or concrete internal metric.
3. **`[CUT: <brief summary of removed line/paragraph and why>]`** — **Mandatory** whenever a full sentence or paragraph is deleted in Pass 2 or Pass 3, so the author never has to manually diff Section 3 against Section 4 to see what was removed.
4. **`[REWRITE FROM SCRATCH: <direct prompt for what the author needs to state here>]`** — Mandatory when >70% of a paragraph is unsalvageable fluff and `sources` do not contain replacement facts (prevents ghostwriting).
5. **`[LEAVE UNRESOLVED: <the tension or trade-off left open>]`** — Used at the end of a section or conclusion where artificial neatness was stripped away, signaling to the author not to tack a tidy summary back on.

---

## Section 5: Integrity Check

### Schema
A mandatory closing audit ledger reconciling what remains unfinished or intentionally untouched.

```markdown
## 5. Integrity Check

- **Unverified Claims Remaining (`N`):**
  - [Quote exact claim + paragraph #] — Status: `Unsupported` / `Contradictory` / `Unclear`
- **Unresolved Structural or Experiential Gaps (`N`):**
  - [Reference to Q# in Section 2 and where the gap sits in the draft]
- **Preserved Authorial Idiosyncrasies (`N`):**
  - "[Exact unusual phrase or deliberate rough edge from the original draft]" — Preserved because [reason it works or matches author voice].
```

### Edge-Case Rules
- **Reconciliation Law:** The count `N` of `Unverified Claims Remaining` MUST equal the exact number of `[VERIFY SOURCE: ...]` tags present in Section 4. The count `N` of `Unresolved Structural or Experiential Gaps` MUST equal the number of questions in Section 2.
- **Anti-Sanitization Proof:** You must identify at least one piece of deliberate, non-standard author phrasing in `Preserved Authorial Idiosyncrasies` (unless the input draft was 100% generic boilerplate) to prove you did not flatten the author's natural quirks into corporate prose.
