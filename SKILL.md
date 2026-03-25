---
name: skill-evolver
description: "Learn from high-quality published skills to evolve and improve your own skill files. Use when the user wants to improve an existing SKILL.md, build a new skill based on proven patterns, or extract best practices from other skills. Triggers on: 'improve my skill', 'evolve this skill', 'learn from other skills', 'optimize my SKILL.md', 'make my skill better', 'skill-evolver'. Analyzes source skills for structural patterns, prompt quality, anti-degradation techniques, and context management strategies, then proposes specific diff-by-diff improvements."
---

# Skill Evolver

Learn from high-quality published skills to improve your own. Extracts proven patterns,
filters for relevance, and proposes specific improvements as interactive diffs.

## Core Principles

1. **Learn then filter** — Extract patterns broadly, apply them narrowly. Not every good
   technique transfers across domains. Always reason about why before applying.
2. **Context discipline** — Source skills are read shallowly (first 80 lines only) unless
   a specific section is needed. Target skill is always read fully.
3. **Diffs over lectures** — Output is concrete proposed changes, not advice. Every
   suggestion includes the exact text to add/change.
4. **Direction drives relevance** — When the user has a clear goal, weight everything
   toward that goal. When they don't, ask first (Phase 0.5).
5. **5-15 changes per session** — More than 15 creates decision fatigue. Stop and offer
   to continue in a new session.

---

## Phase 0: Identify Target + Collect Sources

### Identify the target skill (what to improve)

Check in this order:
1. Is there a SKILL.md in the current directory? → Use it, confirm with user
2. Did the user provide a file path? → Read it
3. List `~/.claude/skills/` and ask the user to choose

If user wants to build from scratch (no existing SKILL.md): note this — Phase 4 becomes
"template extraction" and Phase 5 outputs a full SKILL.md draft instead of diffs.

### Collect source skills (what to learn from)

**From GitHub URL** (if user provided one or more URLs):
```bash
git clone <url> /tmp/<repo-name> 2>/dev/null
```
Read `/tmp/<repo-name>/SKILL.md` (first 80 lines only at this stage).

**From local skills** (always scan unless user said "only GitHub"):
```bash
ls ~/.claude/skills/
```
For each installed skill, read its SKILL.md (first 80 lines only).

**Hard limit: maximum 5 source skills total.** If user provides GitHub URL(s), those
take priority. Fill remaining slots from local skills that appear most relevant to the
target skill's domain (judge by the `description:` frontmatter field).

### Check for direction

Does the user have a clear optimization goal? Signs of clear direction:
- Mentions a specific section or behavior to improve
- Describes a problem ("it keeps forgetting X")
- References a skill they want to emulate
- Says "make it more like X"

If NO clear direction → proceed to Phase 0.5.
If YES clear direction → record it and skip to Phase 1.

---

## Phase 0.5: Direction Discovery (skip if user has clear direction)

Read `direction-questions.md`. Ask 3-4 of the most relevant questions based on what
you already know about the target skill. Do NOT ask all 5 — pick the ones that will
reveal the most.

After the user answers, synthesize into a one-line optimization direction:
> "Optimization direction: [specific, testable statement]"

This direction feeds into Phase 3 (relevance filtering).

---

## Phase 1: Deep Analysis

**Target skill:** Read ALL files in the target skill's directory. Understand:
- What the skill does and what it produces
- Its current structure (how many phases, any supporting files)
- What output type it produces (visual, code, document, workflow, data)
- Obvious gaps or weak spots (note these for Phase 4)

**Source skills:** For each source skill, read only the first 80 lines of SKILL.md.
Extract: purpose, output type, phase structure, any standout techniques visible in the
opening section. Do NOT read supporting files at this stage.

Produce a mental model of each source skill — enough to know what patterns are worth
extracting in Phase 2.

---

## Phase 2: Pattern Extraction

Read `extraction-guide.md` now.

For each source skill, extract patterns across four categories:
- **A. Structural** — Phase design, file organization, progressive disclosure
- **B. Prompt writing** — Constraint language, DO NOT USE lists, anti-default callouts
- **C. Anti-degradation** — NON-NEGOTIABLE clauses, Gotchas sections, mandatory checklists
- **D. Context management** — Lean main file, per-phase loading, scope guards

If a source skill's opening section hints at strong patterns in a specific area (e.g.,
an extensive Gotchas section), read that section fully. Read only what you need.

Output a numbered extraction list grouped by category. See extraction-guide.md for format.

---

## Phase 3: Relevance Filtering

Read `relevance-framework.md` now.

For each extracted pattern, reason through its applicability to the target skill.
Assign: **APPLICABLE**, **PARTIAL**, or **NOT APPLICABLE**.

Key judgment: Layer 1 patterns (structural, context management, prompt techniques) are
universally transferable. Layer 2 patterns (visual specs, domain-specific rules) only
transfer within the same output category.

If Phase 0.5 direction was established, upgrade patterns that align with that direction:
NOT APPLICABLE → PARTIAL, PARTIAL → APPLICABLE (with explanation).

Produce a filtered list: only APPLICABLE and PARTIAL patterns, with verdicts and reasoning.

---

## Phase 4: Gap Analysis

Compare the filtered patterns against the target skill's current state.

For each APPLICABLE/PARTIAL pattern:
- **Already present** in target skill → skip (no change needed)
- **Completely absent** → propose as new addition
- **Present but weaker** → propose as improvement

Rank gaps by priority:
1. Missing anti-degradation protections (highest — these prevent quality regression)
2. Missing context management (high — prevents context overload in long sessions)
3. Weak constraint language (medium — improves reliability)
4. Missing structural improvements (medium — improves usability)
5. Prompt writing refinements (lower — incremental improvements)

Cap at 15 changes. If more than 15 gaps exist, keep the top 15 by priority.

---

## Phase 5: Propose Diffs + Interactive Confirmation

Read `diff-format.md` now.

Present each proposed change one at a time. Wait for A/B/C before the next.

After all changes are confirmed, apply accepted changes to the target SKILL.md
(and create any new supporting files that were proposed).

Show a summary of what was applied. Offer: "Want to continue with more source skills,
or try the updated skill now?"

---

## Gotchas — Common Failure Modes

**Over-reading source skills:** Reading all supporting files of all source skills
in Phase 1 will consume most of the context window before the real work starts.
Read source skills shallowly. Only dive deep into specific sections when needed.

**Copying instead of adapting:** A DO NOT USE list from a visual skill lists font names.
A code generation skill doesn't use fonts. Don't copy — adapt. Identify what the
equivalent "AI default bad behavior" is in the target skill's domain.

**Ignoring direction context:** If Phase 0.5 established that the user wants better
consistency, but you're mostly proposing structural changes — you're off track.
Direction context should visibly influence which patterns you prioritize.

**Too many changes:** More than 15 proposed changes causes the user to start skipping
everything. Stop at 15. If there are more improvements to make, offer a follow-up session.

**Proposing changes that already exist:** Always check whether the target skill already
has an equivalent before proposing. A skill that already has a Gotchas section doesn't
need "add a Gotchas section" — it might need the section improved or extended.

---

## NOT in Scope

- Evaluating whether a skill works well (requires running the skill and judging outputs)
- Searching GitHub for relevant skills automatically (user specifies sources)
- Batch optimization of multiple target skills
- Version history or undo functionality
