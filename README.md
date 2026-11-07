# skills

My personal agent skills for omp (oh-my-pi). Matt Pocock's skills stay upstream at github.com/mattpocock/skills — pulled read-only, never forked; mine live here.

## Skills

- **update-pocock-skills** — asks which harness to target (omp by default), clones or pulls the Pocock repo at /tmp/pocock-skills, installs the six wanted skills plus their dependencies, shows what changed upstream, and guards locally edited skills with a checksum manifest.

## Install

Copy the skill folder into ~/.omp/agent/skills/ and restart omp:

`cp -R update-pocock-skills ~/.omp/agent/skills/`

Verify: `ls ~/.omp/agent/skills` lists the folder.
