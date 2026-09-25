# Research Skills maintenance

This repository stores personal research Skill source. Keep each Skill in `skills/<name>/`. When the user asks to improve or release a Skill:

- Read its complete entrypoint and the references affected by the change. Consult `MAINTENANCE.md` for this repository's release workflow.
- Identify the observed failure or new requirement and supporting evidence. Keep family-specific or species-specific conclusions in appropriate examples/configuration, and preserve evidence boundaries.
- Make the smallest supported change. Preserve the existing scope and invocation policy unless the user requests a change.
- Validate the package and relevant behavior. State checks that were not run; a file-format check is not a biological evaluation.
- Update the affected Skill's `metadata.version` and `CHANGELOG.md` when its content changes. Packaging-only documentation changes do not change the Skill version.
- Record the basis and verification of each released change in the commit/release notes. Do not relocate existing release tags.
- Keep study inputs and outputs in their own projects. Use small synthetic fixtures or permitted example extracts when a reproducible regression case is needed.
- A local edit, installed copy, commit, and remote push are separate states. Verify the requested final state and report what actually happened. Work within the user's authorized target and scope.

Use this repository as the source when it is adopted for maintenance. Existing copies in analysis projects are historical snapshots; do not silently update them.
