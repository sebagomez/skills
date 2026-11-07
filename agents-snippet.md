# AGENTS.md snippet — skills routing

Paste this block into the global AGENTS.md (`~/.omp/agent/AGENTS.md`) so agents suggest and use the installed skills at the right moments. It only routes — each skill explains itself.

```markdown
## Skills

Use the installed skills (~/.agents/skills) at these moments:
- Starting an issue or feature whose requirements or domain terms are not fully pinned down: recommend /grill-with-docs before writing code.
- A bug you cannot yet reproduce: /diagnosing-bugs — build the failing loop first; no fix attempts before it exists.
- Writing new behaviour: /tdd, one vertical slice at a time.
- Before opening a PR: run the closing ritual, with /code-review as its review step.
- Ending a session with work unfinished: /handoff.
```

Note: /grill-with-docs is manual-invocation only, so the block says "recommend" — the agent proposes it, the user invokes it. The other skills the agent can use directly.
