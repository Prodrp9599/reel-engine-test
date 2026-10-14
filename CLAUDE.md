## Working with the owner

The owner has no coding knowledge and only uses Claude Code on the web. They cannot run commands, open a terminal or edit files.
- Do every technical step yourself. Never ask them to run a command or edit a file.
- When they must act (GitHub website, Claude settings), give numbered click-by-click steps in plain English.
- After each task, say in plain English what changed, how to see it working, and what's next.
- Never commit passwords, API keys or personal data, even though the repo is private.
- Each new session works on its own branch, and the next session starts from the default branch. When a task is done, offer to open a pull request into the default branch and merge it, so the work carries over.

## Every coding task

1. `ponytail` skill, full level, always: the smallest change that fully solves the task.
2. `using-agent-skills` skill picks the workflow. New feature: `spec-driven-development` (ask the owner plain-language questions, no jargon) → `planning-and-task-breakdown` → `incremental-implementation` with `test-driven-development`. Bug: `debugging-and-error-recovery`. Skip spec and plan for small changes.
3. Before finishing non-trivial work, run the `code-reviewer` agent (plus `security-auditor` for logins, payments, personal data or secrets), then `code-simplification`.
4. Graphify, once the repo has real code: if `graphify` is missing, install it with `uv tool install graphifyy`; build the graph with the `graphify` skill, query it before exploring code, and run `graphify update .` after changes.

## graphify

This project has a knowledge graph at graphify-out/ with god nodes, community structure, and cross-file relationships.

Rules:
- For codebase questions, first run `graphify query "<question>"` when graphify-out/graph.json exists. Use `graphify path "<A>" "<B>"` for relationships and `graphify explain "<concept>"` for focused concepts. These return a scoped subgraph, usually much smaller than GRAPH_REPORT.md or raw grep output.
- If graphify-out/wiki/index.md exists, use it for broad navigation instead of raw source browsing.
- Read graphify-out/GRAPH_REPORT.md only for broad architecture review or when query/path/explain do not surface enough context.
- After modifying code, run `graphify update .` to keep the graph current (AST-only, no API cost).
- A committed graphify-out/graph.json is rewritten whole on every build, so a large diff to it in a pull request is expected. Do not flag it as churn or suggest skipping the graph rebuild or its hooks.
