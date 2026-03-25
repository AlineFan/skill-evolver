# skill-evolver

A Claude Code skill that learns from high-quality published skills to improve your own.

Extracts proven patterns from source skills, filters for relevance, and proposes specific improvements to your target skill as interactive diffs — one change at a time.

---

## What It Does

Most homegrown skills drift toward the same failure modes: vague constraint language, no anti-degradation protections, bloated main files, and no systematic defense against AI defaults. Meanwhile, well-crafted public skills have already solved these problems — but manually reading them and figuring out what applies to your skill is slow.

`skill-evolver` automates the transfer:

1. **Analyzes** your target skill (what it does, how it's structured, what's missing)
2. **Extracts** patterns from source skills across 4 categories: structural, prompt writing, anti-degradation, and context management
3. **Filters** for relevance — not every good technique transfers across domains
4. **Proposes** concrete diffs with exact text to add or change, not general advice
5. **Applies** the changes you accept, skips the ones you don't

---

## Installation

```bash
git clone https://github.com/AlineFan/skill-evolver.git ~/.claude/skills/skill-evolver
```

Restart Claude Code. The skill will appear as `/skill-evolver` in the command list.

---

## Usage

```
/skill-evolver
```

Then tell it:
- **What to improve** — your target skill (or it will list your installed skills and let you choose)
- **What to learn from** — GitHub URLs of source skills, or it will scan your installed skills

**Examples:**

```
/skill-evolver 帮我优化 xiaohongshu-cover，参考 frontend-slides
```

```
/skill-evolver improve my reading-assistant skill based on https://github.com/zarazhangrui/codebase-to-course
```

```
/skill-evolver  (no arguments — guided setup)
```

---

## How It Works

### 5-Phase Workflow

```
Phase 0   → Identify target skill + collect source skills (max 5)
Phase 0.5 → Direction discovery (skipped if you state a clear goal)
Phase 1   → Deep analysis: read target skill fully, source skills shallowly
Phase 2   → Pattern extraction across 4 categories
Phase 3   → Relevance filtering: APPLICABLE / PARTIAL / NOT APPLICABLE
Phase 4   → Gap analysis: what's missing vs. already present
Phase 5   → Interactive diffs: one change at a time, A/B/C per change
```

### 4 Pattern Categories

| Category | What It Covers |
|---|---|
| **Structural** | Phase segmentation, file splitting, on-demand loading |
| **Prompt Writing** | Constraint language, DO NOT USE lists, anti-default callouts |
| **Anti-Degradation** | NON-NEGOTIABLE clauses, Gotchas sections, mandatory checklists |
| **Context Management** | Lean main files, per-phase file loading, scope guards |

### Two-Layer Relevance Judgment

- **Layer 1 (universal)** — Phase structure, context management, constraint language: these transfer to *any* skill regardless of domain
- **Layer 2 (domain-specific)** — Visual specs, code templates, content rules: these only transfer between skills in the same output category

This prevents blindly copying font rules from a presentation skill into a code generation skill.

### Interactive Diff Format

Each proposed change looks like this:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
改动 #2  [类型: 防退化]
来源: frontend-slides
位置: SKILL.md — Phase 3 之前
理由: 目标 skill 没有 NON-NEGOTIABLE 硬约束，质量容易退化
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

原文（当前）:
┌──────────────────────────────────┐
│ （无）                            │
└──────────────────────────────────┘

建议改为:
┌──────────────────────────────────┐
│ ## 核心原则                       │
│ 1. 输出必须完整（NON-NEGOTIABLE）  │
│    ...                           │
└──────────────────────────────────┘

A) 接受  B) 跳过  C) 先改改再看
```

You decide each change. After all decisions, accepted changes are written to your skill file.

---

## File Structure

```
skill-evolver/
├── SKILL.md                 ← Main workflow (Phase 0–5)
├── extraction-guide.md      ← How to extract patterns from source skills
├── relevance-framework.md   ← Two-layer relevance judgment framework
├── diff-format.md           ← Diff presentation format and pacing rules
└── direction-questions.md   ← Guided questions when goal is unclear
```

Supporting files are loaded on demand — only when each phase needs them. This keeps the context window available for actual analysis work.

---

## Design Principles

**Diffs over lectures** — Every suggestion includes exact text to add or change. General advice ("add more constraints") is not actionable.

**Learn then filter** — Extract patterns broadly from source skills, apply them narrowly to the target. Not every good technique transfers across domains.

**5–15 changes per session** — More than 15 causes decision fatigue. The skill stops at 15 and offers to continue in a new session.

**Context discipline** — Source skills are read shallowly (first 80 lines only) unless a specific section is needed. Target skill is always read fully.

---

## What's Not in Scope

- Evaluating whether a skill works well (requires running the skill and judging outputs)
- Auto-searching GitHub for relevant skills (you specify the sources)
- Batch optimization of multiple skills in one run
- Version history or undo functionality

---

## License

MIT
