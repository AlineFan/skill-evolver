# Extraction Guide

When analyzing source skills in Phase 2, extract patterns across these four categories.
For each pattern, record: source skill name, category, verbatim quote, and why it works.

---

## Category A: Structural Patterns

These govern how the skill is organized — phases, files, and information architecture.

**What to look for:**

- **Phase segmentation** — Does the skill break work into explicit phases (Phase 0, 1, 2...)? Note how many phases, what triggers each, and whether phases have clear entry/exit conditions.
  > Example worth extracting: "Phase 0: Detect mode. Phase 1: Gather content. Phase 2: Style discovery." — Clear separation means Claude doesn't blend steps.

- **File splitting strategy** — Is content split across multiple files? Note what goes in the main SKILL.md vs. reference files, and the naming convention.
  > Example worth extracting: A main SKILL.md that references `style-presets.md`, `animation-patterns.md` loaded only when needed — this is progressive disclosure.

- **On-demand loading** — Does the skill explicitly tell Claude when to read each supporting file? ("Before Phase 3, read html-template.md")
  > This is high value. Many skills load everything upfront and waste context.

- **First-run welcome** — Does the skill have a graceful introduction when the user hasn't provided input yet?

- **Frontmatter description** — Is the `description:` field specific enough that Claude knows exactly when to trigger this skill vs. others?

**Extraction template:**
```
PATTERN-A-[N]
Source: <skill name>
Type: Structural
Quote: "<exact text>"
Why it works: <one sentence>
```

---

## Category B: Prompt Writing Quality

These are linguistic patterns that make instructions stick — that Claude reliably follows rather than drifts away from.

**What to look for:**

- **Constraint intensity language** — Escalating vocabulary that signals non-negotiability:
  - Weak: "try to", "prefer", "consider"
  - Medium: "always", "must", "required"
  - Strong: "NON-NEGOTIABLE", "NEVER", "this is CRITICAL", "without exception"
  > Extract examples of strong constraint language. Note: overuse dilutes impact — only the most important rules should use the strongest language.

- **Anti-default instructions** — Explicit callouts that Claude tends toward a certain bad behavior, followed by a correction:
  > Example worth extracting: "You tend to converge toward generic, 'on distribution' outputs. Avoid this." — Naming the tendency is more effective than just saying "be creative."

- **Concrete over abstract** — Instructions that specify exact outputs rather than vague qualities:
  > Weak: "Write good descriptions." Strong: "Max 2-3 sentences per text block. If you're writing a fourth sentence, stop and convert it into a visual instead."

- **DO NOT USE lists** — Explicit enumeration of forbidden choices:
  > Example: "DO NOT USE: Inter, Roboto, Arial / #6366f1 purple gradients / everything centered"

- **Positive + negative pairing** — Rules that state both what to do AND what not to do in the same breath.

**Extraction template:**
```
PATTERN-B-[N]
Source: <skill name>
Type: Prompt Writing
Quote: "<exact text>"
Why it works: <one sentence>
```

---

## Category C: Anti-Degradation Techniques

These prevent Claude from taking shortcuts, regressing to defaults, or losing consistency across a long session.

**What to look for:**

- **NON-NEGOTIABLE clauses** — Rules stated as absolute, not preferences. These are the "floor" that must never go below.
  > Example: "Viewport Fitting (NON-NEGOTIABLE) — Every slide MUST fit exactly within 100vh."

- **Gotchas section** — A dedicated section listing common failure modes with specific fixes:
  > Example: "WRONG: `right: -clamp(28px, 3.5vw, 44px)` — Browser ignores this. CORRECT: `right: calc(-1 * clamp(...))` — Works."
  > The format "WRONG / CORRECT" is especially effective — it pre-loads the fix so Claude doesn't have to rediscover it.

- **Mandatory element checklists** — Explicit lists of things that must appear in every output:
  > Example: "Mandatory interactive elements (every course must include ALL of these): Group Chat Animation, Message Flow Animation, Code↔English Translation Blocks, Quizzes, Glossary Tooltips."

- **Failure mode documentation** — Descriptions of what happens when a rule is broken, making the consequence concrete:
  > Example: "Using scroll-snap-type: y mandatory traps users inside long modules."

- **Repetition for emphasis** — Critical rules stated in multiple places in the skill (main file + gotchas section):
  > This is intentional redundancy — not DRY — because the rule is important enough to catch in multiple contexts.

- **"Already required above, but reiterating"** — Explicit acknowledgment that a rule is being repeated because it's non-negotiable.

**Extraction template:**
```
PATTERN-C-[N]
Source: <skill name>
Type: Anti-Degradation
Quote: "<exact text>"
Why it works: <one sentence>
```

---

## Category D: Context Management Strategies

These control how the skill uses the context window — preventing overload while ensuring Claude has what it needs when it needs it.

**What to look for:**

- **Lean main file** — Is the SKILL.md under ~300 lines? Does it avoid dumping all reference material inline?
  > Note the ratio: lines in SKILL.md vs. total lines across all files. A healthy ratio is roughly 1:3 to 1:5.

- **Explicit per-phase file loading** — Instructions like "Before Phase 3, read X. In Phase 4, read Y."
  > This is the single most important context management pattern. Loading a file costs context; loading it only when needed preserves context for the actual task.

- **Reference file naming convention** — Are supporting files named to reflect their load-time? (e.g., loaded during generation vs. loaded during setup)

- **Content that belongs in reference files (not SKILL.md):**
  - Style specifications (colors, fonts, spacing values)
  - Animation catalogs
  - Code templates / boilerplate
  - Error/gotcha catalogs
  - Long examples

- **Content that belongs in SKILL.md (not reference files):**
  - Phase structure and triggers
  - Core principles (5-7 max)
  - Hard constraints
  - Output format requirements
  - File reading instructions

- **Scope guards** — Instructions that prevent Claude from doing unnecessary work:
  > Example: "If user chose 'No images' in Phase 1, skip this section entirely."
  > Example: "If user chose 'No' for inline editing, do NOT generate any edit-related HTML, CSS, or JS."

**Extraction template:**
```
PATTERN-D-[N]
Source: <skill name>
Type: Context Management
Quote: "<exact text>"
Why it works: <one sentence>
```

---

## What NOT to Extract

Skip these — they are domain-specific and won't transfer to other skills:

- Visual design specifications (specific colors, font names, animation timing values)
- Output format details tied to a specific medium (HTML structure, slide layout rules)
- Domain-specific terminology and concepts
- Tool-specific commands (Python scripts, specific CLI tools)
- Content quality rules tied to a specific audience or output type

**When in doubt:** Ask "would this pattern apply to a skill that produces a completely different type of output?" If yes → extract. If only works for this specific output type → skip.

---

## Extraction Output Format

After analyzing all source skills, produce a numbered list grouped by category:

```
## Extracted Patterns

### A. Structural (N patterns)
[A-1] Source: frontend-slides | Phase segmentation with explicit trigger conditions
  Quote: "Phase 0: Detect mode (new presentation / PPT conversion / enhancement)"
  Why: Clear entry conditions prevent Claude from guessing which phase applies

[A-2] Source: codebase-to-course | On-demand file loading per phase
  Quote: "Read references/design-system.md before writing any CSS."
  Why: Loads specialized knowledge exactly when needed, not upfront

### B. Prompt Writing (N patterns)
[B-1] ...

### C. Anti-Degradation (N patterns)
[C-1] ...

### D. Context Management (N patterns)
[D-1] ...

---
Total extracted: N patterns across 4 categories
```
