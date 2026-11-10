# Project Rules

## Model & Agent Routing (save tokens)

Main session plans and decides. Delegate to the cheapest agent/model that fits,
and pass `model` on each Agent call to match the task's difficulty:

| Task | Agent | Model |
|------|-------|-------|
| Find files/symbols, "where/how is X" | `scout` | haiku |
| Run tests/builds/lint, installs, mechanical edits | `runner` | haiku |
| Normal feature / bug fix | `builder` | sonnet |
| Large or tricky feature, hard bug | `builder` | opus |
| Review a non-trivial diff | `checker` | sonnet |
| Security-sensitive review | `checker` | opus |
| Architecture, final decisions | main session | — |

- 1-2 tool calls (known file read, one-line edit, quick command) → do it inline, no agent.
- Give agents a self-contained prompt (paths, goal, done-criteria) so they don't re-explore.
- Run independent agents in parallel; never re-delegate work already in context.
- Agents return short summaries, not file dumps.
- Don't start the ruflo daemon or swarms unless asked (they spend tokens continuously).

## Code Rules

- Do what was asked; nothing more. Prefer editing existing files over creating new ones.
- No docs files unless requested. Keep files under 500 lines.
- Never commit secrets or `.env` files. Validate input at system boundaries.
- After code changes run the project's tests/build before committing.
