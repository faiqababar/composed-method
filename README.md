# Composed method

An agent skill for writing, refactoring, and reviewing code in composed-method style to generate clean and well structured code.

A function is a table of contents: each line is one named step at the same abstraction level, and details live in helpers one level down.

## What it helps accomplish

- Keep high-level functions readable as a sequence of named steps instead of mixed orchestration and implementation
- Keep one level of abstraction in a method. Abstraction is how a person understands something. Jumping from one level of abstraction to another takes mental effort. If you have to keep jumping between levels to understand some code, it will be far more difficult to understand. So mixed “what” and “how” is harder to read.
- Tackle complex problems top-down: that is how people think, and the structure in your head gets translated into the code
- Flatten nested guards with early exits so the happy path stays obvious
- Name steps after real actions and existing domain words, not invented labels
- Extract only when the name is a real step, not because a block is long or appears twice
- Leave comments only when the code cannot show why

The full rules are in [SKILL.md](./SKILL.md).

## Inspiration

This skill is inspired by Kent Beck’s _Smalltalk Best Practice Patterns_. In that book, Beck says:

> Compose methods out of calls to other methods, each of which is at roughly the same level of abstraction.

[Jorge Manrubia’s write-up of the pattern](https://www.jorgemanrubia.com/2009/06/28/the-composed-method-implementation-pattern/) is the explanation this skill follows. Three properties should rule method design:

- **Cohesion.** A method does one thing, from the caller’s point of view.
- **Clear interfaces.** Steve Maguire’s _Writing Solid Code_ says the signature — name, parameters, return, and their types — should be enough to understand the method. That needs cohesion. C’s `realloc()` is the anti-pattern: allocate, grow, shrink, and free behind one name.

  ```c
  void *realloc(void *ptr, size_t size);
  // ptr == NULL  -> allocate
  // size == 0    -> free
  // otherwise    -> grow or shrink, maybe move
  ```

- **Symmetry.** The steps a method is divided into sit at the same level of abstraction.
