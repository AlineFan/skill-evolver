# Relevance Framework

For each extracted pattern, reason through its applicability to the target skill.
This is Phase 3. Read extraction-guide.md output first, then apply this framework.

---

## Two-Layer Judgment

### Layer 1 — Universal Patterns (applicable to ALL skills)

These patterns improve any skill regardless of what it produces. Default verdict: **APPLICABLE** unless the target skill already has an equivalent.

| Pattern Type | Examples | Transfer Rule |
|---|---|---|
| Phase segmentation | Explicit Phase 0/1/2/3 structure | Always transferable — adapt phase names to the skill's actual workflow |
| On-demand file loading | "Before Phase 3, read X.md" | Always transferable — identify which phases need which knowledge |
| Lean main file | SKILL.md < 300 lines | Always transferable — move bulky content to reference files |
| DO NOT USE lists | Forbidden fonts, patterns, approaches | Always transferable — identify what AI defaults to badly in this domain |
| NON-NEGOTIABLE clauses | Hard floor rules that must never slip | Always transferable — identify the most important invariant in this skill |
| Gotchas section | WRONG/CORRECT pairs for common failures | Always transferable — identify where AI commonly fails in this domain |
| Scope guards | "Skip if user chose X" | Always transferable — identify optional sections |
| Anti-default callouts | "You tend to converge toward X. Avoid this." | Always transferable — identify what this skill's "AI slop" equivalent is |
| Frontmatter description precision | Specific trigger conditions in `description:` | Always transferable |
| Mandatory element checklists | "Every output must include ALL of these" | Always transferable — identify non-optional output components |

### Layer 2 — Domain-Specific Patterns (only transfer within same output category)

These patterns are tied to a specific output type. Only transfer if the target skill produces the same general type of output.

**Output categories:**

| Category | Examples | Layer 2 patterns transfer within |
|---|---|---|
| Visual/design output | HTML presentations, XHS covers, infographics, posters | Color systems, typography rules, layout patterns, animation specs |
| Code generation | App scaffolding, component generation, API clients | Architecture templates, naming conventions, testing patterns |
| Document/text output | Reports, articles, course content, slide decks | Content density rules, structure templates, tone guidelines |
| Workflow/process | CI/CD skills, review skills, deployment skills | Step sequencing, approval gates, rollback patterns |
| Data transformation | Format converters, extractors, parsers | Schema validation rules, error handling patterns |

**How to apply:**
- Identify which category the SOURCE skill belongs to
- Identify which category the TARGET skill belongs to
- If same category → APPLICABLE (with possible adaptation)
- If different category → NOT APPLICABLE (note the reason)

---

## Verdict Guide

For each extracted pattern, assign one verdict:

**APPLICABLE**
The pattern directly improves the target skill. The concept, structure, or technique transfers with minimal or no modification.
> Use when: The pattern addresses a problem that clearly exists in the target skill, and the solution would work as-is or with obvious renaming.

**PARTIAL**
The core idea is valuable but the specific implementation needs adaptation. The principle transfers, the details don't.
> Use when: The pattern addresses a real gap but uses domain-specific language, examples, or structure that need to be replaced.
> Always specify: What to keep vs. what to change.

**NOT APPLICABLE**
The pattern is tied to the source skill's specific domain and does not address a need in the target skill.
> Use when: The pattern is about visual specifications, tool-specific commands, or content rules that only make sense for the source skill's output type.
> Always specify: Why — don't just say "not applicable," say "because this target skill produces X, not Y."

---

## Reasoning Template

For each pattern, reason aloud before assigning a verdict:

```
Pattern: [B-3] Anti-default callout from frontend-slides
Source quote: "You tend to converge toward generic, 'on distribution' outputs."

Target skill: xiaohongshu-cover (produces social media cover images)

Reasoning:
- Does the target skill have a "default AI behavior" problem? YES — Claude tends to generate
  generic, visually plain covers without strong visual hierarchy.
- Does the specific language need adaptation? YES — "on distribution" is fine, but the
  examples would need to change to XHS-specific failures (centered text, stock photo aesthetic,
  pale color choices).
- Is this Layer 1 (universal) or Layer 2 (domain-specific)? Layer 1 — the technique of
  naming the tendency is universal. The examples are domain-specific but easy to swap.

Verdict: PARTIAL
Keep: The technique of explicitly naming the AI tendency before correcting it
Change: Replace frontend examples with XHS-specific "slop" patterns
```

---

## Common Mistakes in Relevance Judgment

**Over-applying (too liberal):**
- Taking visual spacing rules ("use 48px padding between sections") and applying them to a code generation skill
- Taking animation timing rules from a presentation skill and suggesting them for a document skill
- Copying domain-specific terminology that won't mean anything in the target skill's context

**Under-applying (too conservative):**
- Saying "NOT APPLICABLE" to a Gotchas section because the specific bugs listed don't apply — the *pattern* of having a Gotchas section applies even if the content changes
- Saying "NOT APPLICABLE" to on-demand file loading because the target skill has different phases — the *technique* applies, just adapt to the target's phases
- Refusing to transfer DO NOT USE lists because the forbidden items differ — identify what the target skill's forbidden defaults are

**The test:** Would a skilled human prompt engineer, looking at the target skill, recognize the same problem this pattern solves? If yes → at minimum PARTIAL.

---

## Applicability with Direction Context

When Phase 0.5 direction discovery was run, weight relevance toward the user's stated goals:

- User said "I want it to be more consistent" → weight toward anti-degradation patterns (C) and mandatory checklists
- User said "it does too much in one prompt" → weight toward context management patterns (D) and phase segmentation
- User said "the output quality is variable" → weight toward prompt writing patterns (B) and NON-NEGOTIABLE clauses
- User said "I want it to handle more edge cases" → weight toward scope guards and gotchas sections
- User described a specific skill they liked → analyze what made that skill work and look for those patterns specifically

When direction context exists, patterns that align with the stated goal should be upgraded: NOT APPLICABLE → PARTIAL, PARTIAL → APPLICABLE (with explanation).
