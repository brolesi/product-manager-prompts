# CLAUDE.md

Instructions for Claude Code and Claude agent sessions in this repository.

## What this repo is

A curated library of AI prompts for product management and marketing
professionals. Every asset serves two purposes: practical PM usefulness
and pedagogic value. If there is a tradeoff, prefer pedagogy and clarity
over cleverness.

Read `AGENTS.md` for the full authoring contract, directory map, and
quality bar. Read `prompting-style-guide.md` for the complete
methodology. This file is the fast-path orientation.

Pattern documents: `generative-guidance-pattern.md` (v2 facilitation),
`interaction-modes.md` (facilitation / co-construction / autonomous
investigation), `jinja2-prompt-structures.md` (loops, switches, and
guards for /loop, /goal, and agent-safe prompts).

Tooling: after adding or editing prompts, run
`python3 scripts/validate-prompts.py` (structural checks; errors
block, warnings are the migrate-on-touch worklist) and
`python3 scripts/generate-catalog.py` (regenerates `catalog/`).

---

## Interaction Modes and the Generative Guidance Pattern

Every prompt follows one of three interaction modes, defined in
`interaction-modes.md`: **facilitation** (Generative Guidance — the
human holds the context), **checkpointed co-construction** (an artifact
drives section-by-section building with a human gate per section), and
**autonomous investigation** (the world holds the context; evidence
contract, citations, defaults that let the prompt run unattended).

Most prompts in `/prompt-generators/` and a portion of the generators
in `/storytelling/` use the **Generative Guidance** pattern, now at
**v2**. Read `generative-guidance-pattern.md` before creating or
editing any file in those directories.

The short version of v2: the AI asks a budgeted 3–5 questions one at a
time, offering 3 context-aware recommendations plus "Other" per
question. Two standing bypasses are available at every turn: "take your
best guess" (AI answers, names the assumption) and "bulk drop" (user
pastes notes; AI extracts answers, accounts for found / inferred /
missing, asks only about gaps). The user can skip, go back, or stop
early. The AI searches before offering options that would otherwise be
generic. The final output is withheld until the loop closes with a
confirm-before-build summary. If the user arrives with enough context,
questions are reduced or skipped.

**When editing a prompt that uses this pattern:**
- Choices 1–3 must be generated from accumulated context, not hardcoded.
- The standing bypasses (best guess, bulk drop) are non-negotiable
  fixtures — do not remove them.
- The context-detection collapse rule must be present in the prompt.
- Each question must visibly narrow based on prior answers.
- Existing v1 prompts (5-choice menus) are grandfathered; migrate to v2
  when the file is next edited, not in mass rewrites.

---

## Coupling discipline

**Forward pointers are free. Backward prerequisites are debt.**

Every asset must be describable, and runnable, without naming another
file above its Final Step block. A prompt that requires reading a
second file before it produces anything is tightly coupled, and tight
coupling destroys the pedagogy for exactly the novice-to-nascent user
this library exists for.

- Forward ("this could feed a battle card next") -> Final Step only,
  always optional. Write more of these.
- Backward ("run X first", "the sibling of Y", "assumes context is
  already present in session") -> do not write these.
- **Duplication across tiers is intentional.** A generator, a
  workshop, a `prompts/` template, and a loop for the same framework
  teach four different things. Do not consolidate them, and never
  reduce one to a pointer at another.
- `prompts/` is the novice floor: one file in, one finished artifact
  out. Declare `Standalone: yes` and mean it.
- A required input never justifies a stall: ask once, offer the
  best-guess bypass, proceed with a labeled worked example.

Full rules in `AGENTS.md` (Coupling Discipline); enforcement lives in
`scripts/validate-prompts.py`.

## Key constraints

- **Pedagogy first.** Treat every prompt as a teaching artifact. Preserve
  the hidden curriculum in metadata comments.
- **Template stability.** Do not silently mutate canonical output
  templates. Version explicitly (`v1`, `v2`) if structure must change.
- **Workload inversion.** Never ask users to pre-design the artifact the
  AI should help create. The AI proposes; the human reacts.
- **Persona language first.** Decision options are phrased from the
  user's world before adding business translation.
- **Humans as decision owners.** AI assists; it does not replace judgment.

---

## Required metadata block

Every prompt file needs a comment block with:
`Description`, `Standalone`, `Usage Note`, `Instructions`,
`Attribution`, `Licensing`, `Date`.

`Standalone` is one of: `yes` (required throughout `prompts/`),
`better with [artifact], works without`, or `requires [artifact]`
(`loops/` and `vibes/` only).

**`Attribution` cites public sources only.** Books, articles,
published talks and webinars, named frameworks, public URLs,
Productside course and playbook material. Never decks, private notes,
or client work -- and never an unnamed engagement ("grounded in field
experience across multiple X engagements"), which names no customer
but still asserts the content came out of confidential work. Write the
substance instead: if the rule is legal, then financial, then
operational, say that. Full rule in `AGENTS.md`.

---

## Directory intent

| Directory | What lives here |
|---|---|
| `prompts/` | Core PM frameworks and execution prompts |
| `prompt-generators/` | Meta-prompts that emit reusable prompts; Generative Guidance |
| `workshops/` | Guided sessions that produce the artifact itself (battle card, PRD, canvas) |
| `storytelling/` | Narrative and visual prompts; some use Generative Guidance |
| `market-intelligence/` | Autonomous research prompts; evidence contracts, schedulable |
| `loops/` | Seasoned /goal, /loop, /batch, /routine recipes; three levels (plain, loop lingo, Just Enough Jinja2) |
| `skeletons/` | Prompt architecture analysis and reverse-engineering tools |
| `skills/` | Agent Skills: one `kebab-case` folder each, holding `SKILL.md` (+ optional `template.md`, `examples/`) |
| `vibes/` | Experimental and agentic workflow prompts |
| `flows/` | Flow exports and automation artifacts (e.g. LangFlow JSON) |
| `resumes-resignations-reactions/` | Satirical and creative prompts |

---

## Before you finish

1. Practical + pedagogic value both present.
2. Metadata block complete.
3. Naming and placement follow directory intent.
4. Generative Guidance fixtures intact if the prompt uses that pattern.
5. No burden-shifting questions; options are persona-first and context-aware.
6. Attribution cites only sources a reader could go find.
7. `python3 scripts/validate-prompts.py` passes.
