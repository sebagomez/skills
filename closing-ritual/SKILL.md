---
name: closing-ritual
description: Run before declaring any coding task done. Verifies the work with real test output, self-reviews the diff, and reports with evidence instead of claims. Use at the end of every feature, bug fix, or refactor session, before the final summary.
---

# Closing ritual — finish with proof

Never declare a task done on your own say-so. Before your final report, complete these steps in order.

## 1. Prove it runs

- Run the verification the project itself defines: the tests covering the code you touched, plus typecheck and lint when the repo has them. Find the commands in the repo steering file (AGENTS.md / CLAUDE.md), package.json scripts, Makefile, or CI config.
- Report the actual result: the command you ran, pass/fail counts, and any failure output that matters.
- If something fails, fix it and rerun. If a failure is pre-existing and unrelated to your change, prove it (for example, show it also fails on the base branch) and say so explicitly.
- If you cannot run the verification (missing service, credentials, or environment), state exactly what is missing. Never claim success you did not observe.

## 2. Review your own diff

- Review the complete diff of your changes. Use /review if it is available; otherwise re-read every hunk yourself, as if reviewing the PR of a stranger.
- Look for: correctness bugs, missed edge cases, unrelated changes, leftover debug output, TODOs, commented-out code — anything you would flag in someone else's PR.
- Fix blockers and majors before reporting. Note every fix this step produces; the final report lists what the self-review caught.

## 3. Check the scope

- The diff contains only what the task needed. No drive-by refactors, no formatting churn in untouched code, no unrelated files.

## 4. Report with evidence

End with a short report in this shape:

- **What changed** — one or two lines per file or area.
- **Verification** — commands run and their results, with numbers, not adjectives.
- **Self-review** — what it found and what you fixed. "Reviewed, nothing found" is acceptable only if you actually reviewed.
- **Not verified** — anything you could not check, and why.

If a step cannot be completed, the task is not "done" — it is "done except X", stated plainly in the report.
