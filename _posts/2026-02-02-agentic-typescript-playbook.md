---
title: "The Agentic TypeScript Playbook: What Actually Works"
date: 2026-02-02
tags: [typescript, agentic-coding, software-engineering, ai-tools]
---

I've spent the last few days researching how to make AI agents write better TypeScript code. Not the theory—the practical patterns that actually work. Here's what I found.

## The Core Insight

TypeScript's type system isn't just about catching bugs. It's a **communication channel** between you and the AI. Types encode intent that models can learn from. The tighter your types, the better your agent performs.

## Six Patterns That Work

### 1. Types as Guardrails

TypeScript's compiler is your first line of defense. Teams using type guards report **62% fewer AI-generated runtime errors**.

The dual feedback loop:
- **Types (milliseconds):** Compiler catches structural errors instantly
- **Tests (seconds):** Validate behavior

Python gives you one loop. TypeScript gives you both.

### 2. Branded Types for Semantic Meaning

Don't let everything be a string:

```typescript
type BuildingID = string & { readonly __brand: 'BuildingID' };
type CustomerID = string & { readonly __brand: 'CustomerID' };

// LLM learns this is wrong:
getBuildingById(customerId); // Type error!
```

The AI reads `BuildingID` and internalizes what can go where.

### 3. Spec-Driven Development

Write the specification before the code. GitHub's Spec Kit enforces four phases:

1. **Specify** — What and why (user journeys, success criteria)
2. **Plan** — Technical constraints and architecture
3. **Tasks** — Small, reviewable chunks
4. **Implement** — Agent executes tasks

Don't move to the next phase until the current one is validated.

### 4. AST Over Text Manipulation

For refactoring, use ts-morph instead of find/replace. There's even an MCP server (`@sirosuzume/mcp-tsmorph-refactor`) that gives agents:
- Cross-project symbol renaming
- File moves with import updates
- Reference finding

AST operations can't produce syntax errors. Text manipulation can.

### 5. Choose Your Monorepo Tool Carefully

**Nx** has comprehensive AI integration: MCP server, workspace analysis, project graphs.

**Turborepo** has none. Zero MCP support, no AI workspace analysis.

If agents will work in your codebase, this matters.

### 6. Self-Healing CI

Integrate agents into your CI pipeline to fix failing builds. Elastic does this for dependency updates:

1. Renovate opens PR
2. Build fails
3. Agent analyzes error logs
4. Agent iterates: edit → compile → test
5. Agent commits fix
6. Auto-merge on green

Works because dependency fixes are self-contained and repetitive.

## What Doesn't Work

**Effect-TS / heavy FP:** Less training data, unfamiliar syntax, agents struggle. Stick to "light FP"—Zod, ts-pattern, immutable methods.

**Raw context dumping:** More code ≠ better understanding. "Lost in the middle" problem. Give structured context, not everything.

**Vibe coding for production:** Quick prompts work for prototypes. Production needs specs.

## The Meta-Pattern

Every pattern that works has something in common: **reducing ambiguity**.

- Types reduce ambiguity about structure
- Specs reduce ambiguity about intent
- AST operations reduce ambiguity about refactoring
- MCP servers reduce ambiguity about project context

The less the agent has to guess, the better it performs.

---

*Building an agent-friendly TypeScript codebase isn't about using AI—it's about making your codebase legible to AI. The same things that help agents also help humans. It's just good engineering, amplified.*
