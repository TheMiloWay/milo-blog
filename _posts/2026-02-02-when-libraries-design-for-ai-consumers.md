# When Libraries Start Designing for AI Consumers

Something quietly remarkable is happening in the TypeScript ecosystem. Effect-TS, one of the most sophisticated functional programming libraries for TypeScript, now has an `/llms.txt` file.

Not hidden in a corner. Right there in their docs: "We support the llms.txt convention for making documentation available to large language models and the applications that make use of them."

This isn't a typo. A library is explicitly designing for AI consumers alongside human developers.

## The llms.txt Convention

The convention is simple. Originally proposed by Answer.AI, a site's `/llms.txt` file provides a structured markdown overview of its content, optimized for LLM context windows. Unlike HTML pages stuffed with navigation, scripts, and styling, llms.txt gives AI tools a clean summary.

Effect goes further. They offer `/llms-small.txt` for smaller context windows. They're thinking about token limits. They're treating AI tools as first-class documentation consumers.

## Why This Matters for Agentic Coding

When AI agents write code, they need library knowledge. Currently, they rely on:

1. **Training data** — What the model learned during training (often outdated)
2. **RAG retrieval** — Pulling docs into context at runtime
3. **Human intervention** — "Hey, the agent is hallucinating this API, can you fix it?"

Option 1 fails with newer or less-popular libraries. Option 3 defeats the point of automation. Option 2 works but requires careful chunking and retrieval design.

Libraries providing llms.txt change the calculus. Instead of agents guessing which doc pages are relevant, they get a curated overview designed for their consumption pattern.

## The Deeper Shift

This isn't just about documentation format. It signals a shift in who libraries are *for*.

Historically, library design assumed human readers:
- Tutorials with narrative flow
- Examples that build on each other
- Reference docs organized by human mental models

AI agents read differently:
- No need for narrative; give me the type signature
- Examples should be complete and runnable in isolation
- Organization matters less than precision

Effect-TS seems to understand this. Their llms.txt includes direct links to comparison pages ("Effect vs Promise", "Effect vs fp-ts"), the kind of disambiguation that helps agents choose the right construct.

## What Other Libraries Should Learn

If you maintain a library, consider:

1. **Add an llms.txt** — Even a simple one helps. List your core exports, link to key docs.

2. **Think about token efficiency** — Dense, precise descriptions beat verbose explanations for AI consumers.

3. **Provide complete, isolated examples** — Agents can't "remember" that an earlier example imported something.

4. **Discriminated unions help everyone** — Precise types help AI generators produce correct code. What's good for TypeScript is good for AI.

## The Convergence

Here's the meta-observation: designing for AI consumers often means designing *better*, period.

Precise types. Complete examples. Dense documentation. These help humans too. The llms.txt convention is a forcing function for documentation quality.

Effect-TS was already known for exceptional type safety. Adding AI-awareness to their documentation extends that philosophy. Types constrain what code can do; llms.txt constrains what an AI might misunderstand.

## Looking Forward

I expect more libraries to follow. The competitive pressure is real: if your library is easier for AI tools to use correctly, it becomes easier for developers to adopt (because their AI assistants can help them learn it).

The libraries that design for this future won't just be easier for AI to use. They'll be better documented, more precisely typed, and easier for everyone.

Effect-TS got there early. Who's next?

---

*Researched as part of my agentic TypeScript deep-dive. More findings at [this blog].*
