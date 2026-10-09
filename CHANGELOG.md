# Changelog

## genome-family-identification 1.2.1 — 2026-10-09

- Translated repository guidance, release history, the skill description, and UI metadata into English for sharing with colleagues.
- Retained existing English methods, scientific thresholds, counts, identifiers, evidence boundaries, commands, and implicit-invocation policy. Preserved original Chinese source-path names and explained their provenance role in English.
- Validated the skill format, YAML metadata, relative documentation links, and preservation of scientific entrypoint content. This documentation-only patch does not rerun biological analyses or change historical results.

## genome-family-identification 1.2.0 — 2026-09-25

- Added an MDR → CAD/ADH subgroup-identification example based on a user-supplied manual workbook and original CDD/KofamScan records, clarifying the roles of `cd05283 Specific + K00083` and `cd08301 Specific + K18857` evidence.
- Verified the same MDR92 input set: CAD had 12 CDD-supported candidates, 11 KO-supported candidates, and an intersection of 11, with 12 retained by the project; both ADH annotations identified 14, with 10 classical ADH candidates retained after reference-subgroup review.
- Recorded why 4 ADHL3-like candidates require separate grouping even though both annotations pass, their models are complete, and 9/9 Zn-binding sites are conserved. Preserved distinctions among GSNOR, multiple KOs, not tested, and below threshold.
- Added intersection, union, and set-difference checks within the same input set to the general workflow, preventing agreement between two annotations from being treated as a final functional assignment. Kept the original CAD80 and MDR92 workflows separate.
- Verified agreement between the workbook, raw tables, and final lists for every ID; the original workbook was not modified. Searches, tree inference, and functional experiments were not rerun.
- Passed skill-format validation and checks of 14 relative links. An independent new-species scenario review correctly distinguished the retained CAD exception, ADHL3-like proteins, classical ADH, multiple KOs, untested long models, and CDD Non-specific hits.

## genome-family-identification 1.1.0 — 2026-09-25

- Added a complete example based on updated coffee SGD results: 86 GH1 candidates include 1 with exact-sequence heterologous-pathway evidence, 2 untested close CarSGD homologs, 5 MsSGD1-similarity experimental priorities, and 78 with unresolved function.
- Added multiple evidence routes, decision precedence, and reference-panel sensitivity to the general workflow. Close homologs do not inherit a reference sequence's experimental evidence.
- Distinguished actual code-based selection gates from supporting review evidence; specified HSP coverage and score-difference definitions, the missing coverage gate in historical group B, and the case in which the tree cannot define an SGD-exclusive monophyletic group.
- Verified agreement between original table groups and exported lists, exact-sequence correspondence, and key branches in the existing tree. BLAST, tree inference, and functional experiments were not rerun. The original coffee results and their 8-member working list were unchanged.
- Passed skill-format validation and checks of 9 relative links. An independent new-species scenario test preserved exact-sequence evidence, identified risks from high local similarity and short fragments, and avoided mechanically applying the 20-bit threshold.
- Cross-species threshold validity and functional accuracy have not been tested end to end; this release was a compatible example extension.

## genome-family-identification 1.0.0 — 2026-09-25

- Established a workflow covering literature references, BLASTP/HMM candidate retrieval, Pfam/CDD, KO, phylogeny, and model-integrity checks.
- Recorded family membership, model integrity, functional evidence, project inclusion, and decision origin separately.
- Included coffee MDR, CAD, ADH, STR, TDC, MATE, and SGD examples with their evidence boundaries.
- Distinguished historical integrated judgment, retrospective rules that reproduce a list, and prospective rules for new tasks in the CAD example.
- Completed skill-format and reference-file checks, installation-content consistency checks, and historical case reviews recorded in the project.
- Not yet completed: an end-to-end test of the complete workflow in a new species.

## Repository preparation — 2026-09-25

- Included all six files from the installed 1.0.0 skill without changing their contents.
- Added instructions for GitHub storage, restoration, version updates, and review.
- Set 1.0.0 as the initial version baseline; subsequent release status is determined by remote commits and Releases.
