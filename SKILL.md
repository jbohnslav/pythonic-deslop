---
name: pythonic-deslop
description: Python style guide for AI coding agents. Use when writing or modifying Python code to produce clean, idiomatic code.
---

# Write Python Like a Human

Write the simplest code that solves the problem. Every line should justify its
existence. If a pattern doesn't make the code clearer, shorter, or more correct,
leave it out.

This is a style guide for AI coding agents. LLM-generated Python has a recognizable
"house style" that experienced developers find grating. This document exists to
counteract those habits and produce code that reads like it was written by a
senior Python developer who values simplicity, clarity, and the actual idioms
of the language.

## Anti-Slop Rules

**Underscore spam.** A leading underscore means "non-public API" per PEP 8 — it's
a legitimate convention, but agents massively overuse it. Only use `_` when a name
is truly non-public API and that distinction matters. Don't prefix every helper.

**Defensive try/except.** Only catch exceptions when you can handle them meaningfully,
add context, or convert them at a boundary. Never catch-log-reraise — that's what
tracebacks are for. Never use bare `except:`.

```python
# BAD — defensive programming theater
def load_config(path):
    try:
        with open(path) as f:
            try:
                config = json.load(f)
            except json.JSONDecodeError as e:
                logger.error(f"Failed to parse config: {e}")
                raise
    except FileNotFoundError as e:
        logger.error(f"Config file not found: {e}")
        raise
    except Exception as e:
        logger.error(f"Unexpected error loading config: {e}")
        raise
    return config

# GOOD — let it crash, the caller will know what happened
def load_config(path):
    with open(path) as f:
        return json.load(f)
```

**Over-classing.** Prefer module-level functions over classes unless you have state
that mutates over time. A bag of pure functions sharing a config dict is not a class.

**Premature abstraction.** No `BaseProcessor` with one subclass. No registry for
three items. No plugin system that will never have plugins. Write the concrete
thing; extract a pattern only when you see it repeat.

**Annotation noise.** Type hints on function signatures: yes. Type hints on every
local variable: no.

**Docstring parroting.** If the function name and signature tell the whole story,
skip the docstring. Docstrings should explain *why*, not restate *what*.

**Log spam.** Log at boundaries and when something interesting happens. Don't log
entry/exit of every function. Trust the traceback.

**Redundant guards.** Don't add guard clauses for conditions that would already
raise an informative error. `user.name` on `None` already gives `AttributeError`.

**Obvious comments.** Don't narrate what the code does. No `# return the result`
above a return, no `# increment counter` above `i += 1`. Comments should explain
non-obvious *intent*, not restate the code in English.

## Pythonic Preferences

- Use pathlib over os.path.
- Use dataclasses for simple structured data.
- Use comprehensions/generators when they improve clarity; don't golf.
- Use context managers for files/resources.
- Return early to avoid deep nesting.
- Favor stdlib for simple things, but consider well-known PyPI packages (pytest, httpx, numpy, scikit-learn, etc.) over complex stdlib workarounds. Check if a mainstream library already solves the problem before hand-rolling it.
- Use the real idioms: truthiness checks, `enumerate`, unpacking, comprehensions.
- Prefer concrete names (`invoice`, `retry_delay`) over generic ones (`data`, `item`, `value`) when the type/role is known.
- Use modern type syntax: `list[str]` not `List[str]`, `X | Y` not `Union[X, Y]`. Avoid `Any` unless truly unavoidable.
- Don't hand-format for line length or style. Write for readability; let `ruff` handle the rest.

## When Modifying Existing Code

- Preserve local style unless it is clearly harmful.
- Reduce complexity and line count where possible.
- Do not introduce frameworks or patterns unless requested.

## General Vibes

- Flat is better than nested.
- Explicit is better than implicit — but explicit doesn't mean verbose.
- Practicality beats purity.
- YAGNI. Build for now, not hypothetical futures.
- A 50-line script beats a 200-line "properly architected" one.

## Bug Fixing

When something that should work has a bug:
1. Write a test that reproduces the bug.
2. Run it. Confirm it actually fails.
3. Fix the bug.
4. Run the test again. Confirm it passes.

Don't skip steps. Don't "fix first, test later." The failing test is proof you
understand the bug before you touch the code.

## Testing

- Minimize mocks. Prefer real (or anonymized real) data as test fixtures.
- Test non-trivial logic; don't write tests for trivial wiring.
- Keep tests fast and readable — a test is documentation.

## Before Writing Code

1. Solve the problem in the most direct way.
2. Avoid introducing abstractions until they are clearly needed.
3. Prefer one clean pass over speculative extensibility.
4. If adding complexity, justify it in one sentence.

## Self-Check Before Finalizing

1. Did I add unnecessary underscores?
2. Did I add unnecessary try/except blocks?
3. Did I create abstractions that don't pay for themselves?
4. Would a strong Python reviewer call this clean and idiomatic?
