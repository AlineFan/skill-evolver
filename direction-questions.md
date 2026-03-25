# Direction Questions

Use this in Phase 0.5 when the user has not stated a clear optimization direction.
Ask 3-4 of these questions (choose the most relevant ones based on what you already
know about the target skill). Do NOT ask all of them — pick the ones that will reveal
the most about what "better" means for this specific skill.

---

## The Questions

**Q1. 当前不满**
> 你现在运行这个 skill 的时候，最让你不满意的是什么？
> 比如：输出质量不稳定？经常忘记某个重要步骤？需要反复纠正同一类错误？

*Purpose: Identifies the most painful failure mode. Maps to anti-degradation patterns (C) and Gotchas sections.*

---

**Q2. 缺失功能**
> 有没有什么你希望这个 skill 能做到、但现在做不到的？
> 比如：希望它能处理某种输入、生成某种格式、在某个阶段问你一个问题？

*Purpose: Identifies structural gaps. Maps to phase segmentation (A) and scope expansion.*

---

**Q3. 参照 skill**
> 你有没有用过某个 skill 感觉特别顺手？不一定要功能相同，可以是工作流程、互动方式、或者输出格式上让你满意的。

*Purpose: The user's "gold standard" — analyze what made that skill work and look for those patterns in the source skills.*

---

**Q4. 一致性问题**
> 同样的输入，你运行这个 skill 两次会得到明显不同的结果吗？如果是，在哪些方面？

*Purpose: Flags consistency issues. Maps to NON-NEGOTIABLE clauses (C) and mandatory element checklists (C).*

---

**Q5. 最后一次手动纠正**
> 你上次手动修改这个 skill 输出的时候，改了什么？

*Purpose: The most concrete signal of what's failing. Whatever the user had to fix manually is the highest-priority Gotcha to document.*

---

## How to Use the Answers

After asking 3-4 questions, synthesize the direction into a one-line optimization goal:

```
Optimization direction: [specific, testable statement]

Examples:
- "Reduce output variability — ensure every run produces the same core structure"
- "Add a user checkpoint at Phase 2 before generating the final output"
- "Prevent regression to generic defaults in visual output"
- "Handle the 'no existing file' case gracefully"
```

This direction statement then feeds into Phase 3 (relevance filtering) — patterns that
align with this direction get upgraded from PARTIAL → APPLICABLE.

---

## When to Skip Phase 0.5

Skip this phase entirely if the user has clearly stated ANY of these:
- A specific section of the skill they want to improve
- A specific behavior they want to add or fix
- A specific skill they want to learn from (often signals they already know what they want)
- "Make it more like X" (X = a clear reference point)

When in doubt, ask one quick question: "Is there a specific aspect of this skill you
want to focus on, or should I decide based on what I find?" — if the user gives any
direction, that's enough to skip Phase 0.5.
