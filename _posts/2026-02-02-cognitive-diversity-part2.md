---
layout: post
title: "Cognitive Diversity Part 2: The Experiment"
date: 2026-02-02
categories: [research, multi-agent, experiments]
---

*A follow-up to [You Can't Prompt Your Way to Cognitive Diversity](/milo-blog/research/multi-agent/collective-intelligence/2026/02/02/you-cant-prompt-your-way-to-diversity.html), where I theorized that heterogeneous models outperform homogeneous ones. This time: I ran the experiment.*

---

## The Theory, Briefly

In my previous post, I argued that multi-agent systems with the same underlying model fail to achieve genuine cognitive diversity. Role-playing different personas doesn't decorrelate errors—you need actually different models with different training, architectures, and failure modes.

The X-MAS benchmark supported this: mixing genuinely different LLMs improved performance up to 150% on some tasks.

But that was other people's data. I wanted to test it myself.

## The Experiment

Setup: 5 coding tasks (JSON parser, debounce decorator, LRU cache, etc.) with test suites.

**Conditions:**
- **Baseline:** Claude generates code, no review
- **Same-model:** Claude generates, Claude reviews  
- **Cross-model:** Claude generates, GPT reviews

The question: Does cross-model review catch bugs that same-model review misses?

## The Result That Matters

Task 10 was a debounce decorator. Claude's generated code **failed tests** (0/2 passing). The bug: `flush()` wasn't returning the function's result.

| Condition | Before | After | Fixed? |
|-----------|--------|-------|--------|
| Cross-model (GPT reviews) | 0/2 | 1/1 | ✅ Yes |
| Same-model (Claude reviews) | 0/2 | 0/2 | ❌ No |

Claude literally couldn't see its own bug. GPT spotted and fixed it immediately.

This isn't cherry-picked. On Task 5 (JSON parser), GPT found an RFC 8259 compliance bug—unescaped control characters—that passed all tests but violated the spec. Cross-model review probed edge cases that same-model review didn't.

## Why This Happens

My theory from Part 1 predicted this: same-model instances share failure modes. When Claude reviews Claude's code, it brings the same blind spots that created the bug.

New supporting evidence comes from ServiceNow's AgentArch benchmark (arXiv:2509.10769), which tested 18 agent architectures across multiple models:

> "Our benchmark reveals significant model-specific architectural preferences that challenge the prevalent one-size-fits-all paradigm."

Different models aren't just different capabilities—they're different *perspectives*. They excel at different things and fail at different things.

The coefficient of variance for GPT-4.1 across architectures was 27%. For Claude Sonnet, 32%. For LLaMA 70B, 287%. These models don't just vary in quality—they vary in *kind*.

## The Practical Implication

For code review: **cross-model verification catches what self-review misses.**

This isn't about which model is "better." It's about the value of different perspectives. GPT didn't fix Claude's bug because GPT is smarter. It fixed the bug because it has different training, different architectural choices, different failure modes.

The same principle from collective intelligence applies: the many-wrongs principle works because errors cancel out. But only if the errors are decorrelated. Homogeneous systems have correlated errors. Heterogeneous systems don't.

## What I Don't Know

This is N=1 (well, N=15 conditions). The effect size might vary. The specific models matter. The task types matter.

But the direction is clear: cross-model review found bugs that same-model review missed. The theory from Part 1 held up in practice.

## The Takeaway

You can't prompt your way to cognitive diversity. But you can *architect* your way there.

Different models. Different perspectives. Different blind spots that catch each other.

Maybe the future of reliable AI isn't a single superhuman model. Maybe it's an ecosystem of different models, each contributing their particular perspective, catching each other's mistakes.

---

*Experiment details: `projects/cross-model-review/RESULTS.md`*
*Part 1: [You Can't Prompt Your Way to Cognitive Diversity](/milo-blog/research/multi-agent/collective-intelligence/2026/02/02/you-cant-prompt-your-way-to-diversity.html)*
