---
name: closing-ritual-bb
description: bb variant of closing-ritual. Run before declaring any coding task done in bb with omp. Verifies the work with real test output, reviews the diff with the code-review skill (not /review, which bb does not pass through to omp), and reports with evidence instead of claims. Use at the end of every feature, bug fix, or refactor session, before the final summary.
---

# Closing ritual (bb variant) — finish with proof

Never declare a task done on your own say-so. Before your final report, complete these steps in order.

This variant exists because bb owns the composer and its `/` menu inserts bb skills — typing `/review` in bb suggests skills, it does not invoke omp's `/review` command. Everything else in the ritual is unchanged.

## 1. Prove it runs

- Run the verification the project itself defines: the tests covering the code you touched, plus typecheck and lint when the repo has them. Find the commands in the repo steering file (AGENTS.md / CLAUDE.md), package.json scripts, Makefile, or CI config.
- Report the actual result: the command you ran, pass/fail counts, and any failure output that matters.
- If something fails, fix it and rerun. If a failure is pre-existing and unrelated to your change, prove it (for example, show it also fails on the base branch) and say so explicitly.
- If you cannot run the verification (missing service, credentials, or environment), state exactly what is missing. Never claim success you did not observe.

## 2. Review your own diff — via the code-review skill, not /review

- Do NOT type `/review` in bb: it will not reach omp's review command.
- Instead, invoke the Matt Pocock `code-review` skill on the diff (the skill already installed for his work agents). It runs two parallel sub-agent reviews and reports them separately:
  - **Standards** — does the code follow this repo's documented coding standards (plus the Fowler smell baseline)?
  - **Spec** — does the code faithfully implement the originating issue/spec?
- Give the skill the fixed point (branch, commit, or merge-base) and the originating issue/spec. The Spec axis is what fixes the diff-only blind spot in omp's /review: a deliberate behaviour change is judged against the intent, not flagged as a suspected regression on the diff alone.
- Treat the skill's findings as flags for the author agent to adjudicate, not verdicts — same rule as omp's /review (P0–P3 style severity does not apply here; use the skill's own Standards/Spec reports).
- Fix blockers and majors before reporting. Note every fix this step produces; the final report lists what the review caught.
- Do NOT settle for typing "review your changes" as a plain prompt in the same thread: that is the main agent grading its own homework. The skill's parallel sub-agents are the separation that makes step 2 a real second pair of eyes.

Note: what bb does NOT break — omp's model roles still fire internally. When the agent spawns a subagent, omp still routes it to the `task` role (Sol 6.1, medium, in his work roster); the roles were never a cmux feature and are not a bb UI feature either. This variant only replaces the typed command.

## 3. Check the scope

- The diff contains only what the task needed. No drive-by refactors, no formatting churn in untouched code, no unrelated files.

## 4. Report with evidence

End with a short report in this shape:

- **What changed** — one or two lines per file or area.
- **Verification** — commands run and their results, with numbers, not adjectives.
- **Review** — what the code-review skill found (Standards and Spec separately) and what you fixed. "Reviewed, nothing found" is acceptable only if you actually reviewed.
- **Not verified** — anything you could not check, and why.

If a step cannot be completed, the task is not "done" — it is "done except X", stated plainly in the report.
