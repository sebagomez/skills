# skills

My personal agent skills for omp (oh-my-pi). Matt Pocock's skills stay upstream at github.com/mattpocock/skills — pulled read-only, never forked; mine live here.

## Skills

- **update-pocock-skills** — pulls the Pocock clone at /tmp/pocock-skills, shows what changed upstream, guards locally edited skills with a checksum manifest, and copies the picks into ~/.omp/agent/skills/.

## Install

Copy the skill folder into ~/.omp/agent/skills/ and restart omp:

`cp -R update-pocock-skills ~/.omp/agent/skills/`

Verify: `ls ~/.omp/agent/skills` lists the folder.
