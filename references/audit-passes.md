# Diagnostic Audit Passes & Calibration Reference (`references/audit-passes.md`)

This reference defines the analytical rules, failure modes, and few-shot calibration examples for Passes 1–4 of the `diagnosis-first-editor` skill (v1.1.0).

**Core Calibration Rules:**
1. Do not equate a "successful edit" with smoother prose. In Pass 1 and Pass 3, a successful edit frequently results in deleted paragraphs, exposed gaps, or bracketed stops that force human intervention.
2. **The Unverified Claim Quarantine:** Never apply Pass 4 voice-polishing to an `Unsupported` or `Contradictory` factual claim. Making an unverified claim sound like the author's best writing increases the risk they publish a falsehood.

---

## PASS 1: Truth and Sourcing

### Analytical Mandate
Prevent a fluent rewrite from laundering an unverified, exaggerated, or distorted claim. Evaluate claims against the provided `sources` strictly on scope, precision, and attribution.

### Classification Taxonomy
- **`Verified`**: The provided source directly supports both the fact and the exact scope/intensity of the wording.
- **`Unsupported`**: The draft makes an empirical, historical, or statistical claim with no backing in `sources`.
- **`Contradictory`**: The draft's claim conflicts with the data or caveats in `sources`, or stretches a narrow finding into a universal claim ("scope inflation").
- **`Unclear`**: The claim is too vague to verify (e.g., "studies show significant gains") or the citation link/note is broken or ambiguous.

### Calibration Examples (Pass 1)

#### Example 1.1: Scope Inflation (`Contradictory`)
- **Provided Source Note:** *"2025 internal benchmark of 40 Java microservices showed virtual threads reduced tail latency (p99) by 18% under high-concurrency I/O loads, but showed zero improvement on CPU-bound batch jobs."*
- **Original Draft:** *"Moving to virtual threads cuts service latency by nearly 20% across backend workloads, making traditional thread pools obsolete."*
- **Diagnostic Reason:** Pass 1 (`Contradictory` — Scope Inflation). The source specifies an 18% reduction strictly at `p99` for I/O-bound microservices and explicitly notes zero gain for CPU-bound jobs. The draft inflates this into a blanket 20% latency cut across all workloads and invents a conclusion ("making traditional thread pools obsolete").
- **Calibrated Revision:** *"In our 2025 benchmark of 40 Java microservices, virtual threads cut p99 latency by 18% under high-concurrency I/O loads, though CPU-bound batch jobs saw no gain."*

#### Example 1.2: Unbacked Empirical Claim & Missing Experience (`Unsupported` + Quarantine)
- **Provided Source Note:** *[None provided for this section]*
- **Original Draft:** *"Most enterprise data migrations fail in the third month because engineering teams underestimate schema drift across legacy staging tables."*
- **Diagnostic Reason:** Pass 1 (`Unsupported`). "Most enterprise data migrations fail in the third month" is presented as an industry statistic without evidence. Quarantine the unverified claim rather than rewriting it into a punchy assertion, and prompt for the author's specific lived experience.
- **Calibrated Revision:** *"**[VERIFY SOURCE: 'Most enterprise data migrations fail in the third month']** In the migrations I have worked on, the trouble started when **[ADD YOUR REAL EXAMPLE: Q1 — Which specific migration hit a wall due to schema drift in staging tables, and what broke?]**."*

---

## PASS 2: Surface Habits

### Analytical Mandate
Eliminate synthetic, bureaucratic, and throat-clearing prose habits without acting like a naive word-filter. Never flag a single word in isolation; flag the *rhetorical habit* and explain how it weakens specificity or authority.

### Target Pattern Catalog
1. **Superficial Participial (`-ing`) Tails:** Tacking `", highlighting..."`, `", underscoring..."`, or `", ensuring..."` onto the end of a clause to fake analytical depth.
2. **Mechanical Negative Parallelisms:** Formulaic `"It's not just X; it's Y"` or `"This isn't about X—it's about Y"` constructions used as fake dramatic pivots.
3. **Inflated Copulas (Verb Puffery):** Replacing simple `is`, `has`, or `does` with `"serves as a testament to"`, `"boasts"`, `"stands as"`, or `"plays a pivotal role in"`.
4. **Weasel Attribution & Vague Importance:** `"Critics argue"`, `"Industry leaders agree"`, `"It is increasingly crucial to recognize that"`.
5. **Compulsive Triads (Rules of Three):** Grouping three near-synonyms (`"robust, scalable, and resilient"`) where one concrete metric or mechanism is needed.
6. **Chatbot Residue & Meta-Signposting:** `"Let's dive in"`, `"In conclusion"`, `"At its core"`, `"When it comes to"`.

### Calibration Examples (Pass 2)

#### Example 2.1: Negative Parallelism + Participial Tail Cluster
- **Original Draft:** *"Modern API governance isn't just about enforcing rate limits; it's about fostering a vibrant developer ecosystem, ultimately unlocking seamless integration and driving long-term platform adoption."*
- **Diagnostic Reason:** Pass 2 (Cluster: Negative parallelism + puffery + double `-ing` tail). The `"isn't just X; it's Y"` structure sets up a false dichotomy, and the trailing participial phrase (`"ultimately unlocking... and driving..."`) asserts a vague positive outcome without explaining the mechanism.
- **Calibrated Revision:** *"Rate limits keep an API from falling over, but **[ADD YOUR REAL EXAMPLE: Q2 — What specific governance rule beyond rate limits actually got external developers to adopt your API?]**."*

#### Example 2.2: Inflated Copula + Compulsive Triad
- **Original Draft:** *"The ingestion layer boasts a modular, decoupled, and fault-tolerant architecture that serves as the backbone of our analytics pipeline."*
- **Diagnostic Reason:** Pass 2 (Inflated verbs + triad). `"Boasts"` and `"serves as the backbone"` are promotional filler. `"Modular, decoupled, and fault-tolerant"` stacks three abstract adjectives without telling the reader how failures are isolated.
- **Calibrated Revision:** *"The ingestion layer isolates stage failures so **[ADD YOUR REAL EXAMPLE: Q3 — How does a failed batch get quarantined without halting downstream analytics?]**."*

---

## PASS 3: Structural Shape

### Analytical Mandate
Map the functional skeleton of the piece (`Claim` -> `Evidence` -> `Example` -> `Qualification` -> `Decision` -> `Conclusion`) and disrupt artificial neatness. Real human reasoning contains friction, asymmetric paragraphs, dead ends, and qualifications that do not wrap up in a bow.

### Structural Failure Modes & Prescriptions
- **The Double-Tap Explanation:** A concrete example lands the point, and the next two sentences explain what the example just proved. -> **Move: `Cut` the explanation with an explicit inline `[CUT: ...]` tag; end the paragraph on the concrete image.**
- **Frictionless Causality:** Proposing `"We did X, which solved Y"` without naming the trade-off, maintenance burden, or edge case X introduced. -> **Move: `Add Qualification` from `sources`, or `Request Real Example` of the trade-off (do not invent external caveats silently).**
- **Polished-Out Awkwardness:** Summarizing a messy real-world compromise in sterile executive language. -> **Move: `Restore Friction` via a human-only question.**
- **Bookend Summary:** Final paragraph restates the opening thesis in slightly warmer words. -> **Move: `Cut` the final paragraph entirely using `[CUT: ...]`, or `Leave Unresolved` on a hard open constraint.**
- **Unsalvageable Paragraph (>70% Failure Rate):** Over 70% of the paragraph is synthetic throat-clearing or restated summary with no verified facts. -> **Move: Issue `[REWRITE FROM SCRATCH: <prompt>]` rather than ghostwriting.**

### Calibration Examples (Pass 3)

#### Example 3.1: The Double-Tap Explanation + Tidy Resolution
- **Original Draft (Paragraph 4):** *"During the Q3 traffic spike, our connection pool saturated because retry storms from the mobile client multiplied failed requests by 4x. We implemented exponential backoff with jitter on the client side and capped retries at two. This demonstrates how coordinating client and server resilience patterns is essential for maintaining system stability under unpredictable load, ensuring that temporary hiccups do not cascade into full outages."*
- **Diagnostic Reason:** Pass 3 (`Explanation After Point Landed`). Sentences 1 and 2 deliver the concrete failure and the exact fix. Sentence 3 (`"This demonstrates how..."`) steps back onto a soapbox to explain the moral of the story, draining the tension out of the paragraph.
- **Calibrated Revision:** *"During the Q3 traffic spike, our connection pool saturated because retry storms from the mobile client multiplied failed requests by 4x. We added exponential backoff with jitter on the client and capped retries at two. **[CUT: Removed trailing moral-of-the-story summary sentence ('This demonstrates how coordinating...')]** **[ADD YOUR REAL EXAMPLE: Q4 — Did capping retries at two cause any new user-facing complaints or dropped edge-case transactions that you had to live with?]**"*

#### Example 3.2: Bookend Conclusion (>70% Unsalvageable)
- **Original Draft (Final Paragraph):** *"In summary, choosing between a monolith and microservices requires balancing team velocity against operational overhead. By carefully evaluating your organization's maturity and scaling needs, you can select the architecture that positions your engineering team for sustainable success."*
- **Diagnostic Reason:** Pass 3 (`Conclusion Restates Opening` — 100% synthetic summary). This paragraph contains zero new information and resolves a sharp architectural trade-off with a platitude (`"carefully evaluating... sustainable success"`).
- **Calibrated Revision:** *"**[CUT: Deleted bookend summary paragraph restating the monolith vs. microservices premise]** **[LEAVE UNRESOLVED: End the piece on the preceding trade-off, or answer Q5 — What is the one operational cost of your chosen architecture that still wakes people up at 2 AM?]**"*

---

## PASS 4: Author Voice

### Analytical Mandate
Derive **three explicit, testable mechanical rules** from the author's provided `writing_samples` and enforce them on verified and structural lines. Never invent a "witty," "punchy," or "folksy" persona if the samples do not exhibit those mechanics, and never apply voice-polishing to unverified factual claims.

### How to Derive Voice Rules (Good vs. Bad Rule Formulation)
- **Bad Voice Rule (Vague/Adjectival):** *"The author writes in an engaging, pragmatic, and slightly conversational tone."* (Unusable for line editing).
- **Good Voice Rule (Mechanical/Observable):**
  1. **Cadence:** Alternates 25–35 word technical explanations with abrupt 4–7 word declarative verdicts (e.g., *"That trade-off rarely pays."*).
  2. **Epistemic Posture & Asides:** Uses parentheticals `(like this)` to admit edge cases or personal annoyance mid-sentence rather than dedicating separate paragraphs to caveats.
  3. **Lexical Entry:** Opens paragraphs with concrete tools, file names, or failure states—never with abstract category nouns ("Software engineering," "Modern teams").

### Calibration Example (Pass 4)

- **Derived Voice Rules from Samples:**
  1. Uses contractions (`doesn't`, `you'll`, `we've`) consistently, even in dense technical passages.
  2. Grounds opinions in first-person singular (`I prefer`, `I've broken this by...`) rather than impersonal passive (`It is recommended`).
  3. Cuts conjunctive adverbs (`However,`, `Furthermore,`, `Consequently,`) at the start of sentences, preferring `But`, `And`, or a direct contrast with no transition word.
- **Original Draft (Verified claim, post-Pass 1–3 cleanup):** *"Furthermore, one must avoid running schema migrations inside application startup hooks. Consequently, if three replicas boot simultaneously, lock contention will stall the deployment pipeline."*
- **Diagnostic Reason:** Pass 4 (Violates all three derived voice rules: uses `"Furthermore"`/`"Consequently"`, impersonal `"one must"`, and zero contractions).
- **Calibrated Revision:** *"Don't run schema migrations inside application startup hooks. If three replicas boot at the same time, they'll fight over the migration lock and stall the deploy."*
