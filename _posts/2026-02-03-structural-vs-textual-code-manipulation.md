---
layout: post
title: "Structural vs. Textual: Why AI Agents Need AST Tools"
date: 2026-02-03
categories: [agentic-coding, typescript]
tags: [ts-morph, AST, refactoring, tooling]
excerpt: "LLMs manipulate code as text. But code has structure. ts-morph bridges the gap—and a new Peking University survey confirms why this matters."
---

A recent survey from Peking University ([arXiv:2508.00083](https://arxiv.org/abs/2508.00083)) on LLM-based code generation agents identified a crucial shift: from algorithmic innovation to **engineering practice**. The focus is no longer just making models generate correct code—it's making the *systems* reliable.

One concrete example: the difference between text-level and structure-level code manipulation.

## The Problem with Text-Based Code Changes

When an AI agent modifies code, it typically works with text:

```
Find: "function oldName("
Replace: "function newName("
```

This works until it doesn't. What about:
- `oldName` in comments?
- `oldName` as a variable assigned from the function?
- Other files importing `oldName`?
- Type annotations referencing `oldName`?

Text manipulation breaks referential integrity. The agent doesn't *understand* the code—it's pattern-matching strings.

## AST: Code as Structure

The Abstract Syntax Tree (AST) represents code as the compiler sees it—a tree of nodes with semantic relationships.

```typescript
// This text...
const greeting = (name: string) => `Hello, ${name}`;

// ...becomes a tree:
// VariableDeclaration
//   └─ ArrowFunction
//        ├─ Parameter (name: string)
//        └─ TemplateExpression
//             └─ Identifier (name)
```

When you rename via AST, you're renaming the *concept*, not the string. Every reference, every import, every type annotation updates atomically.

The Peking survey mentions **cAST**—a chunking mechanism for RAG pipelines that uses AST structure to partition code. The insight: chunking code by syntax preserves completeness better than arbitrary line splits. The same principle applies to modification.

## ts-morph: Making AST Accessible

The TypeScript Compiler API is powerful but verbose. [ts-morph](https://ts-morph.com/) wraps it into something usable:

```typescript
import { Project, SyntaxKind } from 'ts-morph'

const project = new Project({})
project.addSourceFilesAtPaths('src/**/*.ts')

for (const file of project.getSourceFiles()) {
  // Find nodes by type, not by string matching
  const awaits = file.getDescendantsOfKind(SyntaxKind.AwaitExpression)
  
  for (const awaitExpr of awaits) {
    if (shouldRefactor(awaitExpr)) {
      const param = awaitExpr.getFirstDescendantByKind(
        SyntaxKind.ObjectLiteralExpression
      )
      param?.replaceWithText(`{ where: ${param.getText()} }`)
    }
  }
  
  await file.save()
}
```

Surgical. Structural. No grep required.

**Pro tip:** Use [ts-ast-viewer.com](https://ts-ast-viewer.com/) to explore AST structure before writing transforms.

## The Hybrid Workflow

The insight: **use AST for structural changes, LLM for semantic changes.**

| Change Type | Tool | Example |
|-------------|------|---------|
| Rename symbol | ts-morph | Cross-file rename with reference resolution |
| Extract function | ts-morph | Move code, create function, update call sites |
| Add feature | LLM | "Add validation to this handler" |
| Fix logic bug | LLM | "This should reject negative values" |

This is what the survey means by engineering focus: not just generating code, but integrating with the *right tools* for each task.

## Validation: The Forgotten Step

Even with AST tools, validation matters. I wrote about the [correctness sandwich](/2026/02/03/the-correctness-sandwich/) architecture:

1. **Type-constrained decoding** (walls during generation)
2. **AST validation** (ts-morph, checking against real types)
3. **Tests** (behavioral correctness)
4. **Cross-model review** (different blind spots)

ts-morph enables layer 2. It gives you access to the same type information the compiler uses—ground truth, not guesses.

## The Survey's Three Distinctions

The Peking survey distinguishes code generation agents from traditional approaches:

| Aspect | Traditional | Agents |
|--------|-------------|--------|
| Mode | Passive (completion) | Active (workflow management) |
| Scope | Clear boundaries | Full SDLC |
| Focus | Algorithmic accuracy | **Engineering reliability** |

That last row is why AST tools matter. If you're building systems, not just generating snippets, you need structured manipulation—not string replacement.

## Practical Takeaways

1. **Text manipulation is fine for small, isolated changes.** Don't overcomplicate.

2. **Cross-file refactors need AST.** If you're touching imports or types, use the right tool.

3. **Pre-commit validation matters.** Even with AST tools, run `tsc` before committing.

4. **Agents should declare their capabilities.** "I can rename symbols safely" is different from "I'll try to find-replace."

The shift from "generate text that looks like code" to "manipulate code structure directly" is a maturity marker for agentic coding. The tools exist. The patterns are emerging. The question is whether we'll use them.

---

*Part of my research into agentic coding with TypeScript. See also: [The Correctness Sandwich](/2026/02/03/the-correctness-sandwich/)*
