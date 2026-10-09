# MIA pathway gene-family identification skills

Source, version history, and migration instructions for personal research skills. The user-designated repository is [yz3394/MIA_pathway_genefamily_identification](https://github.com/yz3394/MIA_pathway_genefamily_identification). It currently contains `genome-family-identification` 1.2.1.

## Contents and status

```text
MIA_pathway_genefamily_identification/
├── README.md
├── AGENTS.md
├── CHANGELOG.md
├── MAINTENANCE.md
├── .gitignore
└── skills/
    └── genome-family-identification/
        ├── SKILL.md
        ├── agents/openai.yaml
        └── references/                 # 6 supporting reference files
```

This repository stores the portable skill source. The skill currently contains 8 files. Version 1.1.0 added evidence-tiered coffee GH1 → SGD screening; 1.2.0 added MDR → CAD/ADH subgroup identification verified against manual records and clarified CDD/KO evidence boundaries. Version 1.2.1 provides English documentation and UI text for sharing with colleagues. See [CHANGELOG.md](CHANGELOG.md) for changes and the extent of validation.

## Source storage and version control

1. Sign in to GitHub locally, clone this repository, and maintain the source under `skills/`.
2. Synchronize with the remote repository and compare existing content before editing; commit changes after relevant validation. Uploading through the website also saves files, but local copies must then be synchronized with those web-created commits.
3. Use skill-prefixed tags for stable versions, such as `genome-family-identification-v1.0.0`. Do not move or overwrite existing tags.
4. Consider a remote backup complete only after verifying the files, commit, and tag on the remote. Each update requires a commit and push; configuring a remote URL does not synchronize files automatically.

Keeping the source directory allows line-by-line review of changes. A version tag identifies a specific commit; a Release can include release notes and a source download for that version. [GitHub documentation](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases)

The repository can be maintained with `main` and short-lived working branches. Once a version is stable, merge it into `main`, tag it, and install it. Do not move or overwrite tags already used in analyses. Version 1.0.0 remains the initial release baseline.

## Restore on a new computer

In the new environment, sign in to a GitHub account with access to the target repository, then ask Codex:

> Use $skill-installer to install skills/genome-family-identification from yz3394/MIA_pathway_genefamily_identification at tag genome-family-identification-v1.2.1. After installation, check package integrity and skill visibility.

Alternatively, clone the repository locally and copy the complete `skills/genome-family-identification/` directory into `~/.agents/skills/`. This is the user-level discovery location described in the linked official documentation; symbolic links are also supported. If the skill does not appear, restart Codex and check again. [OpenAI documentation](https://learn.chatgpt.com/docs/build-skills)

On the original computer, the actual files are in `~/.codex/skills/genome-family-identification/` and are discovered through the `~/.agents/skills/genome-family-identification` link. Preserve and restore the actual files during migration; an absolute-path link from the original computer cannot be reused directly on a new computer.

If the destination already contains a skill with the same name, compare versions and local changes before backing it up and replacing it. The installer does not merge an existing directory automatically. Maintain one active installation source to avoid discovering two different copies with the same name.

## Update and roll back

- The repository's `skills/` directory is the maintained source; the local installation directory is the runtime copy. The first backup matched the installed 1.0.0 contents; later installation updates are separate actions.
- Edit and validate the source, then update the runtime copy after release. Editing GitHub does not automatically refresh an installed copy, and editing an installed copy does not automatically commit changes to GitHub.
- Before updating, preserve the runtime copy and record its current tag/commit. After updating, compare the complete file set and hashes, then check an actual invocation.
- If a new version has problems, restore the complete skill from the previous validated tag and run the relevant cases. Preserve the new version's analysis results and a record of differences.
- For important analyses, record the skill name, version, Git commit, and hashes or a snapshot of the files actually used. Clearly mark any uncommitted temporary changes.

See [MAINTENANCE.md](MAINTENANCE.md) for the update workflow and review cases.

## Analysis environment and data

This repository contains workflows and decision rules. Configure BLAST+, HMMER, MAFFT, tree-building tools, and Pfam/CDD/KO databases for each task; the skill does not replace those tools or databases. Preserve the environment export, software/database versions, download sources, parameters, and input hashes for each analysis. Once an environment configuration has been tested, include its portable configuration in the relevant skill and remove machine-specific paths.

Fully reproducing the historical coffee analyses also requires their original inputs, outputs, commands, and validation records, stored separately. This repository retains gene IDs, counts, and conclusion summaries from the coffee examples but does not include the original analysis data. The user authorized public storage of these methodological examples; check the permitted disclosure scope when adding future examples.

Other self-authored skills can be added under `skills/` in the same way. For preinstalled or third-party skills, record their source and version and preserve their licenses; do not upload an entire personal configuration directory as skill source. Keep an additional release ZIP outside GitHub if useful, and use a repository mirror backup to preserve the complete Git history. [GitHub backup documentation](https://docs.github.com/en/repositories/archiving-a-github-repository/backing-up-a-repository)
