---
name: update-pocock-skills
description: Install or update the Matt Pocock (AI Hero) skills the user wants, from the upstream repo. Use when the user says "update my skills", "update the Pocock skills", or asks to refresh, sync or install the AI Hero skills.
---

# Update Pocock skills

Facts:

- Source clone: `/tmp/pocock-skills` (github.com/mattpocock/skills). Treat it as a pristine mirror — never edit files inside it.
- A manifest at `<skills dir>/.pocock-manifest` records the last successful sync for that harness: first line is the upstream commit hash, the remaining lines are `<sha256>  <path relative to the skills dir>` for every installed file. It is how local edits are detected.
- Only the wanted skills below (plus their dependencies) are managed by this skill. Every other folder in the skills dir (the user's own skills, including this one) is left untouched.

## Wanted skills and their dependencies

Wanted:

- grill-with-docs (engineering)
- code-review (engineering)
- diagnosing-bugs (engineering)
- tdd (engineering)
- handoff (productivity)
- improve-codebase-architecture (engineering)

Dependencies (install these too — the wanted skill calls or consults them):

- grill-with-docs → grilling (productivity), domain-modeling (engineering)
- tdd → codebase-design (engineering)
- improve-codebase-architecture → codebase-design (engineering), grilling (productivity), domain-modeling (engineering)
- code-review, diagnosing-bugs, handoff → none

Always install as well: setup-matt-pocock-skills (engineering) — run once per repo, its answers are read by the other engineering skills.

The install set is the wanted skills + their dependencies + setup-matt-pocock-skills: 10 folders.

## Steps

1. **Ask which harness.** Ask the user which harness the skills are for, with omp as the default. The answer sets the skills dir:
   - omp → `~/.omp/agent/skills/`
   - pi → `~/.pi/agent/skills/`
   - Claude Code → `~/.claude/skills/`
   - anything else → ask the user for the skills folder path.
   
   If the user names the harness in the request itself, use it without asking. Note: a host app is not the harness — inside bb, an omp agent still uses the omp skills dir.

2. **Get the latest upstream.** If `/tmp/pocock-skills` does not exist, run: `git clone --depth 1 https://github.com/mattpocock/skills.git /tmp/pocock-skills`. Otherwise run: `git -C /tmp/pocock-skills pull`.

3. **Read the last sync state.** Read `<skills dir>/.pocock-manifest`. If it does not exist, this is a first install for this harness: skip steps 4 and 5, copy the whole install set in step 6, then do steps 7 and 8.

4. **Show what changed upstream.** Run `git -C /tmp/pocock-skills log --oneline <last-synced-commit>..HEAD -- <the install set's repo paths, each prefixed with skills/>`. If the manifest's commit is not present in the clone (the clone was re-created, e.g. after /tmp was cleaned), skip the changelog and treat the run as a refresh. If the log is empty and step 5 finds no differences, report "Pocock skills already up to date" and stop.

5. **Check for local edits before overwriting anything.** For each file listed in the manifest, compare its current checksum (`shasum -a 256 <file>`) with the manifest value. A mismatch means the user edited that skill locally. Never overwrite a locally edited skill silently: name the affected skills and ask the user — overwrite, skip that skill, or stop. Proceed only as directed.

6. **Copy the install set over.** For each folder not skipped: `rm -rf <skills dir>/<skill-name>` then `cp -R /tmp/pocock-skills/skills/<category>/<skill-name> <skills dir>/`.

7. **Write the new manifest.** First line: the output of `git -C /tmp/pocock-skills rev-parse HEAD`. Then one `<sha256>  <relative path>` line per file under each copied folder (use `find` + `shasum -a 256`). For any skill skipped in step 5, keep its previous manifest lines unchanged so the local edit is still detected next time.

8. **Verify and report.** Run `ls <skills dir>` — the 10 managed folders are listed. Report to the user: the harness targeted, the upstream commits pulled (subjects from step 4), which skills changed, which were skipped and why, and the new commit hash.
