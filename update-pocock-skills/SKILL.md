---
name: update-pocock-skills
description: Update the Matt Pocock (AI Hero) skills installed in ~/.omp/agent/skills from the upstream repo. Use when the user says "update my skills", "update the Pocock skills", or asks to refresh or sync the AI Hero skills.
---

# Update Pocock skills

Facts:

- Source clone: `~/seb/pocock-skills` (github.com/mattpocock/skills). Treat it as a pristine mirror — never edit files inside it.
- Installed skills live in `~/.omp/agent/skills/`.
- A manifest at `~/.omp/agent/skills/.pocock-manifest` records the last successful sync: first line is the upstream commit hash, the remaining lines are `<sha256>  <path relative to ~/.omp/agent/skills>` for every installed pick file. It is how local edits are detected.
- Only the picks below are managed by this skill. Every other folder in `~/.omp/agent/skills/` (the user's own skills, including this one) is left untouched.

The picks (repo-relative paths):

- productivity/grilling
- productivity/handoff
- engineering/grill-with-docs
- engineering/domain-modeling
- engineering/codebase-design
- engineering/code-review
- engineering/diagnosing-bugs
- engineering/tdd
- engineering/improve-codebase-architecture
- engineering/setup-matt-pocock-skills

## Steps

1. **Get the latest upstream.** If `~/seb/pocock-skills` does not exist, run: `git clone --depth 1 https://github.com/mattpocock/skills.git ~/seb/pocock-skills`. Otherwise run: `git -C ~/seb/pocock-skills pull`.

2. **Read the last sync state.** Read `~/.omp/agent/skills/.pocock-manifest`. If it does not exist, this is a first install: skip steps 3 and 4, copy every pick in step 5, then do steps 6 and 7.

3. **Show what changed upstream.** Run `git -C ~/seb/pocock-skills log --oneline <last-synced-commit>..HEAD -- <all ten pick paths, each prefixed with skills/>`. If the log is empty and step 4 finds no differences, report "Pocock skills already up to date" and stop.

4. **Check for local edits before overwriting anything.** For each file listed in the manifest, compare its current checksum (`shasum -a 256 <file>`) with the manifest value. A mismatch means the user edited that skill locally. Never overwrite a locally edited skill silently: name the affected skills and ask the user — overwrite, skip that skill, or stop. Proceed only as directed.

5. **Copy the picks over.** For each pick not skipped: `rm -rf ~/.omp/agent/skills/<skill-name>` then `cp -R ~/seb/pocock-skills/skills/<category>/<skill-name> ~/.omp/agent/skills/`.

6. **Write the new manifest.** First line: the output of `git -C ~/seb/pocock-skills rev-parse HEAD`. Then one `<sha256>  <relative path>` line per file under each copied pick (use `find` + `shasum -a 256`). For any skill skipped in step 4, keep its previous manifest lines unchanged so the local edit is still detected next time.

7. **Verify and report.** Run `ls ~/.omp/agent/skills` — the ten pick folders are listed. Report to the user: the upstream commits pulled (subjects from step 3), which skills changed, which were skipped and why, and the new commit hash.
