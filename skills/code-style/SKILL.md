---
name: code-style
description: Language-agnostic code style rules condensed from the Google C++ Style Guide. Use when writing, reviewing, or refactoring code in any language, and when the user asks about naming, comments, function design, scope, ownership, error handling, or formatting conventions.
---

# Code Style

These rules make code easy to read and maintain in any language. They come from the Google C++ Style Guide, with the C++ syntax removed and the principles kept.

## How to apply

When you write or refactor code, follow these rules without comment. When you review code, cite the section name for each finding. Do not reformat or rename code that the task does not touch.

## Priorities

If two rules conflict, the earlier rule wins.

1. Match the existing code. Local consistency beats every rule below.
2. Write for the reader, not the writer. People read code far more often than they write it.
3. Avoid surprise. Use the obvious construct, not the clever one.
4. Follow the language's community conventions (PEP 8, Effective Go, the Rust API Guidelines) when the codebase has none.
5. On formatting, the language's standard formatter (gofmt, rustfmt, Black, Prettier) wins over this skill.

## Scope and state

- Declare each variable in the narrowest scope, close to its first use, and initialize it in the declaration.
- Avoid mutable global state. Global constants are fine. Globals with complex setup or teardown cause order-of-initialization bugs.
- Keep file-internal helpers private to the file or module.
- Do not add names to shared or standard-library namespaces, and do not monkey-patch them.
- Do not use wildcard imports such as `import *` or `using namespace`.
- Import every module you use directly. Do not rely on a transitive import.
- Group imports: standard library, third-party, then project. Put a blank line between the groups.
- Prefer module-level functions to classes that hold only static methods.
- Make values immutable (`const`, `final`, `readonly`, `val`) when they do not change.

## Types and classes

- Prefer composition to inheritance. Use inheritance only for a true "is-a" relationship.
- Avoid multiple inheritance of implementation. Inheriting several interfaces is fine.
- Mark overrides explicitly when the language allows it (`override`, `@Override`).
- Keep data members private unless they are constants.
- Use a plain record (struct, dataclass, record) only for passive data without invariants. An invariant is a condition the type must always keep true. Use a class when invariants exist.
- Prefer a record with named fields to a tuple or pair when the elements have meaning.
- A constructor must not call overridable methods or do work that can fail silently. Use a factory function when construction can fail.
- Avoid implicit conversions between types. Make conversions explicit.
- Make copy, clone, and equality behavior explicit for each type.
- Overload an operator only when its meaning matches the built-in meaning.
- Prefer polymorphism to branching on a value's runtime type.
- Order declarations public, then protected, then private. Group similar members.

## Functions

- Keep functions short and focused. At about 40 lines, consider a split. This is a soft limit.
- Return values instead of using output parameters. Put inputs before outputs.
- Overload a function only when the reader can understand a call without knowing which overload runs.
- Default arguments must be fixed values. Do not put them on overridable methods.
- Replace boolean flags and magic numbers in calls with named constants, enums, keyword arguments, or option objects.

## Ownership and resources

- Give every resource (memory, file, connection, lock) one clear owner.
- Transfer ownership explicitly. Shared ownership needs a strong reason.
- Release resources deterministically with the language's mechanism (RAII, `with`, `defer`, `using`, try-with-resources).

## Types of values

- Use explicit, checked conversions. Avoid unchecked casts.
- Use a type with explicit width when the size of a number matters.
- Do not use an unsigned type only to say "never negative". Make sure that the value is valid with a check instead.
- Use the language's null value (`nullptr`, `None`, `nil`), not `0`.

## Errors and control flow

- Use the language's idiomatic error mechanism and the codebase's convention. Never hide an error.
- If a branch "cannot happen", fail loudly in it.
- A switch or match must handle every case or have a default case. Mark intentional fall-through.

## Restraint

- Avoid clever metaprogramming, reflection, macros, and code-generation tricks. If you need one, keep it in a few low-level places behind a simple, documented interface.
- The error messages that such a tool shows to its users are part of its interface.
- Use type inference only when the type is obvious or unimportant to the reader. Write the type out otherwise.
- If a closure can outlive its scope, capture variables explicitly.
- A public type alias is part of the API. Document it, and do not add one only for convenience.
- Avoid nonstandard language extensions.
- Prefer the standard library and the dependencies already in use. Add a new dependency only when it pays for its license and maintenance cost.

## Naming

- A name must explain its purpose to a reader from another team.
- Make name length grow with scope: `i` in a three-line loop, `pending_upload_count` at module scope.
- Do not use unfamiliar abbreviations, and do not drop letters (`cnt`, `usr`). Well-known abbreviations such as `id` and `url` are fine.
- Treat acronyms as words: `StartRpc`, `HttpServer`, not `StartRPC`.
- Use the language's casing convention, and use one convention for each kind of name (type, function, variable, constant).
- Make constants look different from variables.
- Give files lowercase, specific names: `http_server_logs.py`, not `logs.py`.
- Do not encode the type in the name (Hungarian notation).
- When a new entity copies an established API, it can follow that API's naming.
- Use inclusive language. Use neutral terms such as `allowlist`, `denylist`, `primary`, and `replica`. Use "they" for a person and "it" for software.

The guide's C++ naming conventions, such as `kConstantName` and a trailing underscore on members, apply only in C++ code or in a codebase that already uses them.

## Comments

- Comment why the code does something. The code already shows what it does.
- Prefer clearer code to a comment: rename, extract a named variable, or use an enum.
- File: the license header if the project requires one, and a summary if the file holds several abstractions.
- Type or class: its purpose, how to use it, and its thread-safety.
- Function documentation: what it does, its inputs, outputs, errors, ownership, null handling, and any cost that is not obvious. Do not repeat the signature.
- Function body: explain each tricky step.
- Field: document invariants and sentinel values, for example "-1 means unknown".
- Global: say what it is and why it is global.
- TODO: `TODO(owner or issue): what to do, and when (a date or an event)`.
- Deprecation: use the language's deprecation mechanism and name the replacement.
- Write complete sentences with correct grammar and punctuation.

## Formatting

If the language has a standard formatter, use it and skip this section. Otherwise:

- Keep lines at 80 characters or the codebase's limit. URLs, long string literals, and import lines are exceptions.
- Indent with spaces, not tabs, unless the language requires tabs (Go, Makefiles). Google uses two spaces.
- Remove trailing whitespace. It causes noise in diffs.
- Use blank lines sparingly, to separate logical chunks.
- Wrap a long call after the open parenthesis or with a four-space continuation indent. Use as few lines as you can.
- Put braces around every control-flow body that is longer than one line, and around both branches of an if-else.
- Make an empty loop body explicit (`{}`, `pass`, `continue`), never a bare `;`.
- When you wrap a boolean expression, put the operator at the end of the line, and do it the same way each time.
- Do not put parentheses around a returned value.
- Write floating-point literals with digits on both sides of the point: `1.0`, `0.5`.
- Encode files as UTF-8. Use non-ASCII characters sparingly.
- Put two spaces before an end-of-line comment.

## Exceptions

- If existing code breaks these rules, match the local style in that file. If you are not sure, ask the maintainer.
- Do not mix style fixes into an unrelated change.

## Source

Condensed and adapted from the [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html), licensed under [CC BY 3.0](https://creativecommons.org/licenses/by/3.0/). Changes: rules rewritten in a language-agnostic form, C++-only rules removed, and the text shortened.
