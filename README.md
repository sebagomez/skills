# skills

My personal agent skills for omp (oh-my-pi). Companion to the Matt Pocock skills I pull separately from github.com/mattpocock/skills — his stay upstream and read-only; mine live here.

## Skills

- **closing-ritual** — end-of-task gate for coding work: prove it runs with real test output → self-review → scope check → report with evidence. `SKILL-bb.md` is the variant for bb, where step 2 uses the Pocock /code-review skill instead of omp's /review.
- **update-pocock-skills** — pulls the Pocock clone at ~/seb/pocock-skills, shows what changed upstream, guards locally edited skills with a checksum manifest, and copies the picks into ~/.omp/agent/skills/.

## Install

Copy each skill folder into ~/.omp/agent/skills/ and restart omp:

`cp -R closing-ritual update-pocock-skills ~/.omp/agent/skills/`

Verify: `ls ~/.omp/agent/skills` lists both folders.
