---
layout: post
title: "Structural vs. Textual: Why AI Agents Need AST Tools"
date: 2026-02-02
categories: [agentic-coding, typescript]
tags: [ts-morph, AST, refactoring, tooling]
excerpt: "LLMs manipulate code as text. But code has structure. ts-morph bridges the gap."
---

My previous post on [agentic coding principles](/2026/02/02/beyond-vibe-coding/) argued that agents need structured feedback loops. Here's a concrete example: the difference between text-level and structure-level code manipulation.

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

## ts-morph: Making AST Accessible

The TypeScript Compiler API is powerful but verbose. [ts-morph](https://ts-morph.com/) wraps it into something usable:

```typescript
import { Project } from 'ts-morph'

const project = new Project({})
project.addSourceFilesAtPaths('src/**/*.ts')

// Find all functions named 'oldName'
for (const file of project.getSourceFiles()) {
  const funcs = file.getFunctions()
    .filter(f => f.getName() === 'oldName')
  
  // Rename with full cross-reference resolution
  for (const func of funcs) {
    func.rename('newName')
  }
}

// Save all affected files
await project.save()
```

Every import, every call site, every type reference—updated correctly. No grep. No broken builds.

## The Hybrid Workflow

The insight: **use AST for structural changes, LLM for semantic changes.**

| Change Type | Tool | Example |
|-------------|------|---------|
| Rename symbol | ts-morph | `rename('oldName', 'newName')` |
| Extract function | ts-morph | Move code block, create function, update references |
| Add feature | LLM | "Add validation to this handler" |
| Fix logic bug | LLM | "This should reject negative values" |

An AI agent can use ts-morph as a precision tool while using natural language generation for creative work. Separation of concerns.

## MCP Integration

The [mcp-ts-morph](https://github.com/nicobailey/mcp-ts-morph) server exposes ts-morph operations to any Model Context Protocol client. Claude, Cursor, and other agentic tools can perform structural refactoring without generating possibly-broken text diffs.

This is the direction: agents with access to *structured* tools, not just text generation. The compiler understands code better than any LLM. Let the agent leverage that understanding.

## Practical Takeaways

1. **Text manipulation is fine for small, isolated changes.** Don't overcomplicate.

2. **Cross-file refactors need AST.** If you're touching imports or types, use the right tool.

3. **Pre-commit validation matters.** Even with AST tools, run `tsc` before committing. Belt and suspenders.

4. **Agents should declare their capabilities.** "I can rename symbols safely" is different from "I'll try to find-replace."

The shift from "generate text that looks like code" to "manipulate code structure directly" is a maturity marker for agentic coding. The tools exist. The patterns are emerging. The question is whether we'll use them.

---

*Part of my ongoing research into agentic coding with TypeScript. Previous: [Beyond Vibe Coding](/2026/02/02/beyond-vibe-coding/)*
