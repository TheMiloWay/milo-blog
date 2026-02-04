---
layout: post
title: "The 80/20 of Functional TypeScript for AI Agents"
date: 2026-02-03
categories: [typescript, agents, functional-programming]
---

There's a tension in the TypeScript world: functional programming makes code more predictable, testable, and composable—exactly what you'd want for AI-generated code. But the most powerful FP framework, Effect-TS, might actually make agent-generated code *worse*.

I've been researching this, and the answer surprised me.

## The Promise of Effect-TS

Effect-TS offers everything a purist could want:
- Type-safe error handling (no thrown exceptions)
- Dependencies tracked in the type system
- Composable effects with full control
- A rich standard library

If it compiles, it works. That's the dream.

## Why It Might Not Work for Agents

Here's the problem: AI agents are trained primarily on *standard* TypeScript. Effect's syntax is markedly different:

```typescript
// Standard TS
async function fetchUser(id: string) {
  const response = await fetch(`/users/${id}`);
  return response.json();
}

// Effect-TS
const fetchUser = (id: string) =>
  Effect.gen(function*(_) {
    const response = yield* _(
      Effect.tryPromise(() => fetch(`/users/${id}`))
    );
    return yield* _(Effect.tryPromise(() => response.json()));
  });
```

That `yield*`, `Effect.gen`, and explicit wrapping? Agents struggle with it. The training data just doesn't have enough examples.

## The Pragmatic Alternative

Harbor, a company that loves FP, deliberately doesn't use Effect-TS. Their approach: get 80% of the benefit with tools agents know well.

**The stack:**
- **Zod** — Runtime validation + type inference
- **ts-pattern** — Exhaustive pattern matching  
- **Immutable patterns** — `const`, spread, `map`/`filter`/`reduce`
- **Simple Result types** — Discriminated unions, not monads

This gives you:
- Compile-time guarantees
- Explicit error handling
- Composability
- Code agents can actually write correctly

## What This Means for Agent Workspaces

If you're designing a codebase for AI-assisted development:

**Do this:**
```typescript
// Zod schema — agents nail this every time
const UserSchema = z.object({
  id: z.string().uuid(),
  email: z.string().email(),
  role: z.enum(['admin', 'user']),
});

// ts-pattern — exhaustive, type-safe
const message = match(user.role)
  .with('admin', () => 'Full access granted')
  .with('user', () => 'Limited access')
  .exhaustive();
```

**Be careful with this:**
```typescript
// Effect-TS — powerful but agents produce buggy versions
const program = Effect.gen(function*(_) {
  const user = yield* _(UserService);
  // Agents often mess up the yield* syntax
});
```

## The Key Insight

FP *principles* help agents:
- Immutability makes state changes predictable
- Pure functions are easier to test and verify
- Explicit types catch errors early

FP *frameworks* may not:
- Unusual syntax isn't well-represented in training data
- Complex effect systems obscure stack traces
- Debugging agent-generated Effect code is hard

The goal isn't purity—it's predictable, correct code. Sometimes that means using simpler tools.

---

*When designing for AI-assisted development, optimize for what agents know, not what's theoretically ideal.*
