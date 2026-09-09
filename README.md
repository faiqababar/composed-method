# Composed method
An agent skill for writing, refactoring, and reviewing code using the **Composed Method** style to produce clean, readable, and well-structured code.

The core idea is simple:

> **A function should read like a table of contents.** Each line represents one meaningful step at roughly the same level of abstraction, while the implementation details live in helpers one level down.

## What it helps accomplish

- **Keep high-level functions easy to read.** They should describe a sequence of named steps instead of mixing orchestration with implementation details.

- **Keep one level of abstraction per method.** Abstraction is about how we understand something. Moving repeatedly between high-level intent and low-level implementation creates unnecessary mental effort. Keeping the “what” separate from the “how” makes code easier to follow.

- **Solve complex problems top-down.** Start with the overall sequence of steps, then move into the details. This mirrors how we naturally reason about problems and lets that mental structure translate directly into the code.

- **Keep control flow flat.** Use guards and early exits instead of unnecessary nesting so the happy path remains obvious.

- **Use meaningful names.** Name steps after real actions and existing domain concepts rather than inventing new labels or abstractions.

- **Extract for meaning, not size.** Create a helper when its name represents a genuine step or concept—not simply because a block is long or duplicated.

- **Prefer expressive code over comments.** Add comments only when the code itself cannot explain *why* something exists or behaves a certain way.

The full set of rules is in [SKILL.md](https://github.com/faiqababar/composed-method/blob/main/SKILL.md).

## Inspiration

This skill is inspired by Kent Beck’s *Smalltalk Best Practice Patterns*. Beck describes the Composed Method pattern as:

> Compose methods out of calls to other methods, each of which is at roughly the same level of abstraction.

[Jorge Manrubia’s explanation of the pattern](https://www.jorgemanrubia.com/2009/06/28/the-composed-method-implementation-pattern/) captures the approach this skill follows particularly well.

Three properties should guide method design:

- **Cohesion.** A method should do one thing from the caller’s point of view.

- **Clear interfaces.** A method’s signature—its name, parameters, return value, and types—should communicate enough to understand what the method does. This depends on cohesion.

- **Symmetry.** The steps within a method should sit at roughly the same level of abstraction. A method should not jump between high-level decisions and low-level implementation details.
