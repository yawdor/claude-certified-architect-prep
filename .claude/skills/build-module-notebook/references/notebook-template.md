# Notebook Template & Style Guide

This is the reusable pattern extracted from Module 1.1
(`01-agentic-architecture-orchestration/src/1_1_agentic_loops.ipynb`). Adapt
the *content* to whatever the fetched module actually teaches — tools,
concepts, and anti-patterns will differ — but keep this *shape*, tone, and
validation discipline.

## Notebook structure (cell-by-cell)

1. **Title cell** (markdown) — module number/name, domain + weight, task
   number, estimated time, source URL. A short, warm welcome paragraph. A
   "🎯 What you'll build" and "✅ What you'll walk away knowing" pair. A
   `> 💳` callout warning that the notebook makes real, billed API calls
   (a few cents for a short exercise), with an honest note that reading first
   and running later is a fine way to use it too.

2. **Setup section** (markdown + code) — `pip install anthropic`, environment
   variable instructions for both PowerShell and bash, then the setup code
   cell (below). Reuse this verbatim; it doesn't change module to module.

3. **Key concept section(s)** (markdown) — faithfully cover what the module
   explains: definitions, the exact quoted callouts, tables (e.g. state/enum
   value references), any "critical point" the module emphasizes.

4. **Build exercise, task by task** (markdown intro + code per task) — one
   task per cell (or a tight markdown/code pair per task), each explaining
   *why* the task matters, not just what to type. Build up to one combined,
   complete function/class only after the pieces are introduced individually
   — mirrors how the module itself breaks down the exercise.

5. **Anti-patterns section** — see "Anti-pattern cells" below.

6. **Reference material** (markdown) — exam traps table, practice scenario
   with answer choices and the correct answer, key takeaways list. Carry
   these over faithfully; they're revision gold and cost nothing to include.

7. **Quick-Fire Recap Q&A** (markdown) — see "The Q&A" below.

8. **Closing note** — one or two lines of genuine congratulations, plus a
   pointer to the next module if the source page names one ("Related lesson").

## Reusable setup cell

```python
import os
from anthropic import Anthropic

# The SDK reads ANTHROPIC_API_KEY from your environment automatically --
# nothing else to configure here as long as it's set (see the Setup section above).
if not os.environ.get("ANTHROPIC_API_KEY"):
    raise RuntimeError(
        "ANTHROPIC_API_KEY is not set. Set it in your shell, then restart the "
        "kernel and run this cell again -- see the Setup section above."
    )

client = Anthropic()
MODEL = "claude-sonnet-5"

print("Connected. Using model:", MODEL)
```

Don't deviate from this — no mocked client, no hardcoded key, fails loudly
and helpfully if misconfigured.

## Anti-pattern cells

For each anti-pattern the module names, write a code cell shaped like this
(real code, then disabled):

```python
# ============================================================
# ❌ ANTI-PATTERN N -- <one-line name of the mistake>
# ============================================================
# Commented out on purpose: built here so you can see the shape of the
# mistake, not just read a description of it.
#
# def some_wrong_function(...):
#     ...actual, syntactically-plausible wrong code...
#
# Why it fails: <the module's stated reason, in your own words if it helps
# clarity, but don't lose the specific failure mode it names>.
```

**Exception:** if the module has a specific *case-study bug* (a named,
concrete scenario — not just an abstract anti-pattern description), keep the
buggy check function **active** (not commented out) in its own cell, paired
with the correct fix, and run both against a **real captured response** from
earlier in the notebook (e.g. `transcript[0]`, or whatever your build
exercise's loop captured). Print what each check concludes, so the reader
watches two lines of code disagree on the same real data — that's the moment
that makes the lesson stick. Example shape:

```python
def is_done_wrong(response) -> bool:
    """DO NOT USE -- kept active only to demonstrate the bug below."""
    ...the buggy check...

def is_done_correctly(response) -> bool:
    """The fix."""
    ...the correct check...

first_real_response = transcript[0]
print("anti-pattern says done:", is_done_wrong(first_real_response))
print("correct check says done:", is_done_correctly(first_real_response))
```

Guard this kind of demo with an `if`/`else` on what the real response
actually contains, and print a graceful explanatory message in the `else`
branch — real model output isn't scripted, so don't let the cell read as
broken if this particular run didn't happen to reproduce the exact condition.

## The Q&A ("Quick-Fire Recap")

Style, not substance, is what makes this land:

- Frame it as a fun self-check, not a quiz: "try answering from memory before
  scrolling", "cover the answer with your hand if you're the honor-system type".
- 6-10 questions. Each: a short, conversational question in **bold**, then a
  `> 💡` blockquote answer that's punchy — one or two sentences, not a
  restated essay.
- Cover: the core definition, the "thing people forget" step, at least one
  anti-pattern with *why* (not just *what*), the safety-net-vs-primary-control
  distinction if the module has an analogous idea, and — if the notebook
  captured something run-specific (a transcript, a result value) — one
  question that references *the user's own output*, so it feels personal
  rather than generic.
- Close with a short, genuine congratulations and a pointer to the next
  module by name, if the source page names one.

Avoid: dry restatement of the key-takeaways list, questions with no clear
single answer, more than ~10 questions (diminishing returns), overdone emoji
(a couple per section is warmth; one per line is noise).

## Validation harness (fake client, conversation-state-based)

Use this shape to dry-run the generated notebook without spending real API
credits. **Do not** use a global call counter — it breaks whenever the
notebook has more than one independent conversation (e.g. a cheap
single-call skeleton demo cell followed later by the full loop), which looks
like a notebook bug but is actually a flaw in the harness.

```python
import os, types
os.environ["ANTHROPIC_API_KEY"] = "dummy-key-for-validation-only"

import anthropic

def mk_block(type_, **kw):
    return types.SimpleNamespace(type=type_, **kw)

class FakeMessages:
    """Decides its response from CONVERSATION STATE, not a call count."""
    def create(self, model, max_tokens, system, messages, tools=None, **kw):
        tool_result_count = sum(
            1 for m in messages
            if m["role"] == "user" and isinstance(m["content"], list)
            and any(b.get("type") == "tool_result" for b in m["content"])
        )
        # Script one plausible response per state the exercise needs to hit,
        # ending in stop_reason="end_turn". Adapt the number of states and
        # the tool names/inputs to whatever this module's exercise actually
        # calls -- this is illustrative, not literal.
        if tool_result_count == 0:
            return types.SimpleNamespace(
                content=[
                    mk_block("text", text="..."),
                    mk_block("tool_use", id="t1", name="<tool_a>", input={...}),
                ],
                stop_reason="tool_use",
            )
        # ...additional states as needed...
        return types.SimpleNamespace(
            content=[mk_block("text", text="...")],
            stop_reason="end_turn",
        )

class FakeClient:
    def __init__(self, *a, **kw):
        self.messages = FakeMessages()

anthropic.Anthropic = FakeClient

# Then exec() the concatenated, extracted code cells in a fresh namespace and
# confirm it runs start to finish with no exception, before showing the user
# anything.
```

Also run, before this harness:

```python
import json, ast, nbformat

nb = json.load(open(path, encoding="utf-8"))          # 1. valid JSON
nbformat.validate(nbformat.read(path, as_version=4))    # 2. spec-compliant
for cell in nb["cells"]:                                # 3. per-cell syntax
    if cell["cell_type"] == "code":
        ast.parse("".join(cell["source"]))
```

If any step fails, fix the generator and regenerate — don't hand-patch the
`.ipynb` JSON directly; keep the generating script as the source of truth so
the next module can reuse it.

## Generating the .ipynb file

Build the notebook via a small Python generator script (see the pattern
below) rather than hand-writing raw notebook JSON — it's far less
error-prone, especially around triple-quote escaping when code cells contain
their own docstrings.

```python
import json, os, uuid

def _id():
    return uuid.uuid4().hex[:8]

def md(text):
    return {"cell_type": "markdown", "id": _id(), "metadata": {}, "source": text.splitlines(keepends=True)}

def code(text):
    return {
        "cell_type": "code", "id": _id(), "execution_count": None,
        "metadata": {}, "outputs": [], "source": text.splitlines(keepends=True),
    }

# Use ''' as the OUTER delimiter for every md()/code() call, since generated
# code cells will contain their own """docstrings""" -- this avoids needing
# to escape anything.

cells = [md('''...'''), code('''...'''), ...]

notebook = {
    "cells": cells,
    "metadata": {
        "kernelspec": {"display_name": "Python 3", "language": "python", "name": "python3"},
        "language_info": {"name": "python", "version": "3.x"},
    },
    "nbformat": 4,
    "nbformat_minor": 5,
}

with open(out_path, "w", encoding="utf-8") as f:
    json.dump(notebook, f, indent=1)
```

Write the generator script to a scratch location (not into the repo) and run
it to produce the `.ipynb` at the resolved target path.
