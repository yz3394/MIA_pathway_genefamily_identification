# Improve skills through actual use

## Minimal workflow for one iteration

1. **Record the issue.** Save the task context, observed behavior, inputs and versions, evidence location, and expected behavior. Record it in a project retrospective; when action is needed, create a GitHub Issue so the information does not remain only in chat history.
2. **Determine the scope.** Distinguish software/parser errors, general workflow problems, family-specific cases, and species-specific annotation anomalies. Keep unverified observations as issues to investigate.
3. **Edit the source.** Make focused changes on a repository branch. Put general rules in the entrypoint or method references and family-specific cases in the relevant example. Keep research data and candidate lists in their research projects.
4. **Validate.** Check file formats, references, and relevant scripts, and run affected historical or synthetic cases. For substantial rule changes, add a case from a different family or species. State which checks were not performed.
5. **Release.** Update the version and CHANGELOG, commit, merge, create a new tag, and push; verify the remote commit. Release notes should record the reason for the change, evidence, validation, and limitations.
6. **Install.** Update the local runtime copy and verify its actual version. Each research project should record the version it uses; preserve original results from older analyses.

Example maintenance request:

> Review the skill issues found in this analysis and distinguish general improvements from family-specific cases. Update the relevant content in yz3394/MIA_pathway_genefamily_identification, validate affected cases, update the version and CHANGELOG, and synchronize validated changes to the designated GitHub repository. Report the commit, installation status, and any unverified items.

This instruction must be issued in an actual task; repository files do not learn or upload themselves. Routine analyses use the installed skill by default. A subsequent maintenance task determines whether their findings should inform skill updates.

## Versioning convention

| Change | Example version | Meaning |
|---|---|---|
| Small correction | 1.0.0 → 1.0.1 | Typo, source clarification, or localized bug fix; explicitly report set differences if the fix affects a candidate list |
| Compatible extension | 1.0.1 → 1.1.0 | New family example, optional workflow branch, or compatible field |
| Major rule/output change | 1.x → 2.0.0 | Changed default decision logic or incompatible output; explain how to compare with older results |

Each skill uses its own `skill-name-vVERSION` tags, for example `genome-family-identification-v1.0.0`. The version number describes the scope of a change, not the amount of biological validation completed.

## Contents of an improvement record

```text
Skill name and previous version:
Task, species, and family in which the issue was found:
Input/output sources and inspectable evidence:
Observed and expected behavior:
Scope: general / family-specific / species- or project-specific
Changes and scientific rationale:
Validation cases, results, and unverified aspects:
Old/new candidate ID differences and reasons, if a list is affected:
New version, commit, push status, and installation status:
```

## Minimal behavioral checks for the gene-family skill

These are review tasks for future updates; their presence here does not mean they have been rerun in this repository. Format validation cannot substitute for these judgments.

### Integrated CAD evidence and KO scope

Use the skill's CAD example and the necessary raw tables, if available, to check that the workflow distinguishes retrospective reconstruction of 80→72→17→12 from the historical integrated judgment; retains project members that fail the KO threshold but have other support; does not label candidates outside the KO input set as threshold failures; and does not label all final 12 as experimentally confirmed classical CADs.

### HMM sequence and domain thresholds

Provide minimal raw output or a synthetic example in which the full-sequence score passes its threshold but the target domain fails its inclusion threshold. Confirm that reported hit counts and counts of proteins meeting the domain criteria are calculated separately, and that query/target orientation is interpreted correctly when switching between `hmmsearch` and `hmmscan`. If there is no relevant parsing code, test the interpretation in an independent task; matching text keywords does not demonstrate correct behavior.

### New species and polyploids

Provide IDs that differ from the coffee format and an annotation mapping that contains homeologous subgenome loci. Check that genes, transcripts, and proteins are counted using the actual mapping and that distinct loci are retained. Do not carry over coffee length ranges, fixed candidate counts, or ID-truncation rules.

Save a small fixture and add an automated test when an actual parsing problem is identified. Once stable scripts are available, format/file checks can be added to GitHub Actions; biological judgments and reviews of actual tasks remain separately documented.
