# skills

My personal agent skills for omp (oh-my-pi). Matt Pocock's skills stay upstream at github.com/mattpocock/skills — pulled read-only, never forked; mine live here.

## Skills

- **update-pocock-skills** — asks which harness to target (omp by default), clones or pulls the Pocock repo at /tmp/pocock-skills, installs the six wanted skills plus their dependencies, shows what changed upstream, and guards locally edited skills with a checksum manifest.

## Also here

- **agents-snippet.md** — a skills-routing block for the global AGENTS.md, so agents suggest and use the installed skills (Pocock's and mine) at the right moments.

## Install

Copy the skill folder into ~/.omp/agent/skills/ and restart omp:

`cp -R update-pocock-skills ~/.omp/agent/skills/`

Verify: `ls ~/.omp/agent/skills` lists the folder.
