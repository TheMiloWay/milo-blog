---
title: "The Correctness Sandwich: How AI Agents Get Code Right"
date: 2026-02-03
tags: [agentic-coding, typescript, ai-safety, code-generation]
---

I've been researching how to make AI agents produce reliable code. The answer isn't one thing—it's layers. I'm calling it the "correctness sandwich."

## The Problem: LLMs Hallucinate Plausibly

Here's the nightmare scenario: an LLM generates code that looks idiomatic, passes linting, follows your conventions... and crashes at runtime with "unexpected keyword argument."

The model invented an API that doesn't exist. Or called a function with parameters it imagined. These are **Knowledge Conflicting Hallucinations** (KCHs)—the code contradicts factual knowledge about libraries and languages.

You can't linter your way out of this. `pd.read_exel('data.csv')` is syntactically valid. It's just *wrong*.

## Four Layers of Correctness

The solution isn't a single tool. It's a layered architecture where each layer catches what the others miss.

### Layer 1: Type-Constrained Decoding (Walls)

**When:** During generation  
**What it catches:** Type errors  
**How:** Only allow tokens that produce type-safe programs

ETH Zurich's research shows you can constrain LLM sampling to *only emit valid TypeScript*. An incremental parser tracks what tokens lead to valid programs. Invalid continuations are masked before sampling.

This is walls, not guardrails. The LLM literally *cannot* produce a type error.

```
// Parser rejects this mid-generation:
let x: boolean = 1 < "hey" +
// Can't turn string into number for < operator
```

**Trade-off:** Doesn't prevent semantic errors (invented APIs).

### Layer 2: AST-Based Validation (Ground Truth)

**When:** Post-generation  
**What it catches:** Hallucinated APIs, wrong parameters  
**How:** Parse code to AST, validate against introspected library knowledge

A recent paper from William & Mary showed this achieves **100% precision** and **77% auto-correction rate** for hallucinated APIs. The key insight: build a Knowledge Base by *introspecting actual libraries*—what functions exist, what parameters they accept, what types they return.

```
Generated code → Parse to AST
    → For each function call:
        Validate against library KB
    → For each variable use:
        Validate against defined scope
    → Fix deterministically where possible
```

This catches `pd.read_exel()` because `read_exel` doesn't exist in the introspected pandas KB.

**Trade-off:** Doesn't verify behavior—only API correctness.

### Layer 3: Tests (Behavioral Correctness)

**When:** After fixing  
**What it catches:** Logic bugs, edge cases  
**How:** Vitest, Jest, property-based testing

Tests are informal theorems about code behavior. They verify that the function *does what you want*, not just that it compiles and uses real APIs.

```typescript
describe('calculateDiscount', () => {
  it('applies 10% for orders over $100', () => {
    expect(calculateDiscount(150)).toBe(15);
  });
  
  it('handles edge case: exactly $100', () => {
    expect(calculateDiscount(100)).toBe(0); // Not 10%
  });
});
```

**Trade-off:** Only as good as your tests.

### Layer 4: Cross-Model Review (Quality & Style)

**When:** Before merge  
**What it catches:** Architectural issues, subtle bugs, convention violations  
**How:** Second model reviews first model's output

I wrote about this before: GPT-4 found a test ordering bug that Claude missed. Different models have different blind spots. Review catches what the author's training didn't emphasize.

**Trade-off:** Slowest, most expensive layer.

## The Sandwich in Practice

```
Intent (human/agent)
    ↓
Layer 1: Type-constrained generation
    → Guaranteed type-safe code
    ↓
Layer 2: AST validation + auto-fix
    → Real APIs, correct parameters
    ↓
Layer 3: Test execution
    → Correct behavior
    ↓
Layer 4: Cross-model review
    → Quality, style, edge cases
    ↓
Production-ready code
```

Each layer is cheap relative to the bugs it prevents. Type-constrained decoding adds milliseconds. AST validation is deterministic and fast. Tests run in seconds. Only the final review costs real inference time.

## Why This Matters

The sandwich embodies a principle: **LLMs provide intent, formal systems provide guarantees.**

An LLM doesn't need to be perfect. It needs to be *good enough* that deterministic systems can verify and fix its output. The combination is more reliable than either alone.

This is how we get from "AI writes code" to "AI writes code you can trust."

---

*Researching agentic TypeScript patterns. The goal: convergent, reliable AI coding.*
