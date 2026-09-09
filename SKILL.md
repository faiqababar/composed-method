---
name: composed-method
description: >
  Use when writing, refactoring, or reviewing functions, helpers, control flow,
  naming, or comments. Also when the user asks for composed method, stepdown,
  orchestrator style, or named sequential steps.
---

# Composed method

A function is a table of contents. Each line is one named step at the same abstraction level. Details live in helpers one level down.

Priority: correctness, then repository conventions, then these rules. Apply to code being changed. Do not migrate unrelated code.

```ts
function processItems(items: Item[]): void {
  const grouped = groupItemsByOwner(items);
  const updates = buildUpdates(grouped);
  persistUpdates(updates);
}
```

Not an orchestrator that also implements grouping:

```ts
function processItems(items: Item[]): void {
  const grouped = new Map<string, Item[]>();
  for (const item of items) {
    /* grouping implementation */
  }
  persistUpdates(buildUpdates(grouped));
}
```

## Extract

Extract when the name is a real step. Do not extract because a block is long, appears twice, or can become a function.

Prefer a little duplication over a callback or deferred helper that hides the sequence. Leave a tiny leaf (one lookup, one return) inline.

## Branches

**Early exit.** Guards first. Happy path stays flat.

```ts
if (item === undefined) return;
if (item.entries.length === 0) return;
process(item);
```

Not nested `if (item !== undefined) { if (item.entries.length > 0) { process(item) } }`.

**Skip before mutate.** Guard before `push`, `add`, `set`, or marking done. Do not add and later undo.

**Two cases.** `if` / `else`, or one early exit plus the remaining path.

**Three or more cases.** `switch` or `if-return` approach

**Narrow missing values once.** Handle `undefined` / `null` in an earlier branch. Later branches do not repeat that check.

```ts
if (value === undefined) {
  skip();
} else if (value.kind === "a") {
  use(value);
} else {
  other(value);
}
```

Not `if (type === 'a' && value !== undefined) { use(value) } else { ... }`. That `else` can still see type `'a'` with `value` missing.

**Type-specific skip stays in that type's branch.**

```ts
if (item.type === "a") {
  if (item.value === undefined) return;
  processA(item.value);
} else {
  processOther(item);
}
```

Do not lift that missing-value guard outside when the other type does not need it.

## Names

The name is the action, not a metaphor. Reuse words from types, tables, APIs, docs, and nearby code. Do not invent labels.

If a function does two meaningful things, split them or name both. Do not call it `buildDrafts` if it also persists.

A distinguishing fact that is not the action does not belong in the identifier. If the code cannot show it, use a short comment.

## Comments

If a named helper, variable, guard, or branch already shows why, add no comment. Do not restate the next line.

Comment only when the why is not in the code: an external constraint, a surprising rule, what something is not, or a trap that looks like a bug. Normal English. No invented jargon.

## Scope

On review, apply the same rules to changed code only. Explain the structural or correctness benefit. Do not request churn when the code is already clear.

## Red flags

- One function both builds a `Map` and writes unrelated output
- `if (type === X && extra !== null) ... else` where `else` can still receive type `X`
- Nested `if (x !== undefined)` when an early exit would flatten it
- A helper named for one action that also does another
- A new term that is not already in the domain
- A callback used only to share a few lines
- A comment that repeats the name or branch
