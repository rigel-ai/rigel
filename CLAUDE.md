# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

This is the **implementation repository** for the Rigel programming language — a strongly-typed,
immutable-by-default, Scheme-derived programming language with first-class constraint-based
generics. The language specification lives in
[rigel-ai/rigel-spec](https://github.com/rigel-ai/rigel-spec).

Rigel is designed around a thesis about AI-assisted coding:

1. Refactoring is much easier with AI
2. Frameworks matter far less
3. Languages matter less
4. Code should be optimized for writing by AI and reading by humans
5. Rigor, structure, and tooling are far more important than syntax sugar

## Source Files

- `src/rigel/` — Python implementation of the toolchain (lexer, parser, type checker, interpreter)
- `tests/` — Test suite (pytest)
- `docs/plans/` — Implementation plans (phased roadmap)
- `pyproject.toml` — Package configuration; entry point is `rigel.driver:main`
- Source files use the `.rgl` extension

## Build and Test

Requires Python 3.12+.

```bash
pip install -e .           # Install in development mode
pytest                     # Run all tests
rigel run file.rgl         # Interpret a program
rigel check file.rgl       # Type-check without running
```

## Key Design Decisions (Settled)

Read the spec repo for full details. Brief summary of non-obvious choices:

- **Declaration separate from definition** — `let` binds names to values. Reassignment uses `set`.
- **Unified function/closure model** — functions and lambdas are the same construct (`lambda`).
- **`int`, `float`, `number` are constraints, not types** — concrete types are `int32`, `float64`.
- **Algebraic effects** — declared via `(:with (fail io))`. No `:with` means pure.
- **Types are opaque by default** — `.field` access only inside the type's own methods.
- **RAII via `:release`** — runs at scope exit including effect-raise paths.
- **Compilation target: C** — monomorphization, tail recursion → loops.
- **One language, two forms** — compiled and interpreted share identical syntax and semantics.

## Collaboration Notes

- The spec repo is the source of truth for language semantics
- Implementation should follow the spec; if the spec is unclear, clarify there first
- The lead designer drives architectural decisions; fill in implementation details
- Reference PL theory when relevant (lambda calculus, catamorphisms/anamorphisms, type classes)
- Connect design choices to C++, Scheme, Clojure, and Rust

## Possible Next Steps

- Implement the C compiler backend
- Flesh out the dictionary-based parameter model and named arguments
- Refine higher-level concurrency patterns (select/alt, cancellation, back-pressure)
