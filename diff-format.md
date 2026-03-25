# Diff Format

Use this format in Phase 5 when presenting proposed changes to the user.

---

## Single Change Block

Present each proposed change as a self-contained block:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
改动 #N  [类型: 结构 / Prompt写法 / 防退化 / Context管理]
来源: <source skill 名称>
位置: <目标 skill 的哪个部分，如"Phase 2 之前" / "Core Principles 节" / "新增节">
理由: <一句话说明为什么这对你的 skill 有价值>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

原文（当前）:
┌─────────────────────────────────────┐
│ <现有内容，如果是新增则写"（无）">     │
└─────────────────────────────────────┘

建议改为:
┌─────────────────────────────────────┐
│ <具体新内容>                          │
└─────────────────────────────────────┘

A) 接受  B) 跳过  C) 先改改再看
```

---

## Scale and Pacing

- Present **one change at a time**. Wait for the user's A/B/C before showing the next.
- **Total changes per session: 5-15.** Stop at 15 even if more gaps exist. Too many changes at once = decision fatigue.
- **Order by impact**: lead with the highest-value changes (usually anti-degradation and structural changes), end with lower-priority refinements.
- If the user chooses C ("先改改再看"), incorporate their edit into the proposed text and re-present.

---

## Change Type Labels

| Label | Use when |
|---|---|
| `结构` | Adding/restructuring phases, splitting into files, adding a new section |
| `Prompt写法` | Improving constraint language, adding DO NOT USE list, strengthening rule phrasing |
| `防退化` | Adding NON-NEGOTIABLE clauses, Gotchas section, mandatory checklists |
| `Context管理` | Moving content to reference files, adding per-phase file loading instructions, adding scope guards |

---

## Writing the "建议改为" Content

**Be specific and complete.** Don't write "add a DO NOT USE section here" — write the actual DO NOT USE section.

**Adapt, don't copy.** The source skill's content uses its own domain vocabulary. Replace domain-specific terms with the target skill's equivalents.

**Match the target skill's voice.** If the target skill is in Chinese, write in Chinese. If it uses a casual tone, match that. If it's formal, match that.

**For structural additions**, write the complete new section including heading, content, and any sub-sections.

**For in-place edits**, show only the changed portion — not the entire file.

---

## After All Changes Are Confirmed

Once the user has accepted/skipped all proposed changes:

1. Apply all accepted changes to the target SKILL.md (and any supporting files if new files were proposed)
2. Show a summary:

```
✓ 已应用 N 条改动 / 跳过 M 条

改动摘要:
  #1 ✓ 结构：新增 Phase 0.5 方向探索节
  #2 ✓ 防退化：添加 Gotchas 章节（3 条）
  #3 — 跳过：Prompt写法改进（用户保留原文）
  ...

目标文件已更新: <path/to/SKILL.md>
```

3. Offer a next step: "要继续从其他 source skill 学习，还是先试试更新后的 skill？"

---

## Special Case: New Supporting File

When a change involves creating a new reference file (not just editing SKILL.md):

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
改动 #N  [类型: Context管理 — 新增参考文件]
来源: <source skill 名称>
文件: <new-file-name.md>（与 SKILL.md 同目录）
理由: <把哪些内容从主文件移出来，为什么>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

新文件内容:
┌─────────────────────────────────────┐
│ <完整文件内容>                        │
└─────────────────────────────────────┘

同时，在 SKILL.md 中添加加载指令:
┌─────────────────────────────────────┐
│ Before Phase X, read new-file.md.   │
└─────────────────────────────────────┘

A) 接受（创建文件 + 更新 SKILL.md）  B) 跳过  C) 先改改再看
```
