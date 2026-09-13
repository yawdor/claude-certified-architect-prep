---
name: build-module-notebook
description: Use when the user gives a claudecertificationguide.com/learn module URL and asks to build, create, or generate a notebook, exercise, or script for it (e.g. "create a notebook for module 1.2", "do the next module", "build the exercise for this URL"). Fetches the module content and generates a validated, real-Anthropic-API Jupyter notebook in the matching domain folder of the claude-certified-architect-prep repo.
argument-hint: <module-url>
allowed-tools: [WebFetch, Read, Write, Edit, Bash, Glob, Grep]
---

# Build Module Notebook

Generates a hands-on Jupyter notebook for one module of the Claude Certified
Architect (CCAR-F) exam prep, given a `claudecertificationguide.com/learn`
module URL, following the conventions already established in this repo (see
Module 1.1, `01-agentic-architecture-orchestration/src/1_1_agentic_loops.ipynb`,
as the reference example this skill was extracted from).

## When to use

- The user gives a URL like `https://claudecertificationguide.com/learn/<domain-slug>/<module-slug>`
  and asks to build/create/generate a notebook, exercise, or script for it.
- The user says things like "do the next module", "build 1.2", "create the
  notebook for X module".

## Repo context

Working repo: `claude-certified-architect-prep` (GitHub: `yawdor/claude-certified-architect-prep`),
already structured with one folder per exam domain, each containing a `src/`
subfolder for notebooks. See `references/domain-folder-map.md` for the exact
domain → folder mapping. Don't recreate this structure — it already exists.

## Steps

### 1. Resolve the target file path

- Parse the module URL's path: `/learn/<domain-slug>/<module-slug>`.
- Map the domain-slug to a repo folder by its **leading number** (e.g. a
  domain-slug starting `1-...` → `01-agentic-architecture-orchestration`),
  per `references/domain-folder-map.md`. This is deliberately robust to the
  exact wording of the rest of the slug — only the leading digit is load-bearing.
- If the leading number doesn't match any of the 5 known domains, don't
  guess: fetch `https://claudecertificationguide.com/learn` to check whether
  the site's domain structure changed, and confirm with the user before
  inventing a new folder.
- **Never guess a module's URL slug from its title.** They frequently
  differ — e.g. module "1.2 Multi-Agent Orchestration"'s real slug is
  `1-2-orchestration-patterns`, not `1-2-multi-agent-orchestration`. If you
  weren't given the exact URL (e.g. you're inferring "the next module" from a
  "related lesson" name, or a user just says "do 1.2"), fetch the domain
  index page (`https://claudecertificationguide.com/learn/<domain-slug>`)
  first and read the real link for that module number, or fetch your guessed
  URL and treat a 404 as the signal to go do that instead of retrying variants.
- Convert the module-slug to a filename: replace `-` with `_`, append
  `.ipynb` (e.g. `1-2-multi-agent-orchestration` → `1_2_multi_agent_orchestration.ipynb`).
- Target path: `<repo-root>/<domain-folder>/src/<module-file>.ipynb`.
- **If a file already exists at that path, stop and ask the user** before
  overwriting — don't silently clobber prior work.

### 2. Fetch the module content

Use WebFetch on the module URL. Ask for everything, verbatim where possible —
the notebook's quality depends on faithfully covering what the module
actually teaches, not a thin paraphrase:

- Module identity (domain, task number, estimated time)
- All learning objectives
- Every key concept, with its explanation, tables, and any quoted callouts
- Any named case study / practical scenario (root cause, fix, exact code shown)
- Every anti-pattern or common mistake called out, with *why it fails* and
  *the fix*
- Any "exam traps" / distractor table
- Any practice scenario with answer options and the correct answer
- The full build-exercise task breakdown, task by task, including the stated
  "why" for each task and any success criteria
- Key takeaways
- Related/next lesson (for the closing pointer)

If the first fetch comes back thin (e.g. "not listed in this excerpt" for
some section), retry with a more targeted prompt naming the missing section
before giving up on it.

### 3. Generate the notebook

Follow `references/notebook-template.md` for the full structural pattern,
reusable setup code, and style guide. Highlights:

- **Real `anthropic` SDK throughout — no mocked API client in the shipped
  notebook.** Read `ANTHROPIC_API_KEY` from the environment; never hardcode a
  key; fail with a friendly, actionable error if it's missing. Use
  `MODEL = "claude-sonnet-5"` unless the module specifies otherwise.
- Build the exercise **incrementally, task by task**, exactly as the module
  lays it out, each with clear comments explaining *why*, not just *what*.
  Keep tasks as separate cells so the notebook teaches the build-up, not just
  the finished result.
- **For every anti-pattern the module calls out:** write it as real, working
  Python, then **comment it out**, with an inline explanation directly below
  the disabled code of why it fails. If the module describes a specific
  case-study bug, keep **one** demonstration of it active (not commented out)
  so it runs against a real captured response from earlier in the notebook
  and visibly reproduces the bug — show it happening, don't just describe it.
- Close with a friendly, professional **"Quick-Fire Recap" Q&A** — 6-10
  questions, conversational tone, short punchy answers, something people
  enjoy reading rather than a dry test bank. When the notebook captures a
  transcript or some other run-specific artifact, tailor at least one
  question to what the *user's own run* actually produced.
- Professional look and feel: clear headers with restrained, purposeful
  emoji (not decoration for its own sake), tables for structured comparisons,
  blockquotes for callouts, and an upfront setup/cost disclaimer since real
  API calls are billed (typically cents, but say so).
- Every code cell needs a unique `id` field (nbformat ≥ 4.5 requirement) —
  the template's generator pattern handles this automatically.

### 4. Validate before showing the user

Never hand over an unvalidated notebook. In order:

1. `json.load` the file — confirm valid JSON.
2. `nbformat.validate()` it (`pip install nbformat` if missing) — fix every
   warning, including missing cell `id`s.
3. `ast.parse` every code cell's source individually — catch syntax errors
   cell-by-cell, not just as one concatenated blob (which can hide which
   cell is actually broken).
4. **Full end-to-end dry run**: extract all code cells in order, concatenate,
   and `exec` them in a fresh namespace with `anthropic.Anthropic`
   monkeypatched to a fake client — **without ever making a real, billed API
   call**. The fake client MUST decide its response from **conversation
   state** (e.g. how many `tool_result` blocks are already in the incoming
   `messages` list), never from a global call counter — a counter-based fake
   breaks as soon as the notebook makes more than one independent
   conversation (e.g. a throwaway single-call demo cell followed by the full
   loop later), which *looks* like a notebook bug but is actually a test
   harness flaw. See `references/notebook-template.md` for a working,
   adaptable fake-client pattern.
5. Fix anything validation surfaces, then re-validate. Don't stop at the
   first clean run if you had to patch something — confirm the fix actually
   holds.

### 5. Report and stop — do not commit

Tell the user the file path, a short summary of what's in it (structure,
tools/concepts covered, roughly how many real API calls a full run makes),
and that it's ready for review. Only `git add` / `commit` / `push` when the
user explicitly asks, matching this repo's established review-then-commit
workflow. When they do:

- Write a commit message describing what module/content was added and the
  validation performed (mirror the style of prior commits in this repo's
  `git log`).
- End it with `Co-Authored-By: Claude <model-name> <noreply@anthropic.com>`
  per the session's standing attribution convention (check the current
  attribution instructions in context for the exact model name to use).
- Push to `origin main` after committing.

## Notes

- If the user's request implies a different repo, or a plain `.py` script
  instead of a notebook, ask before assuming — this skill's default output
  is a `.ipynb` inside `claude-certified-architect-prep`, matching every
  module built so far.
- Install `nbformat` (and `anthropic`, for validating against real SDK types)
  if missing — they're needed to *validate* the notebook, not to run it for
  real.
- Never spend the user's API budget during generation or validation. The
  live run — with their key, at their discretion — is theirs to make.
