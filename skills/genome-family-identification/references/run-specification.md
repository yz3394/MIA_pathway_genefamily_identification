# Run specification and output contract

Copy the following into the run's methods record, then fill it with source-backed values before consequential classification. This is a planning schema, not an executable configuration file. Unknown fields remain explicit; absent information must not silently become a default.

```yaml
run_id: <unique run identifier>
task_mode: <new discovery / existing-result audit / targeted update>
target:
  species_and_material: <taxon and cultivar/strain>
  assembly_accession_and_release: <source identifier>
  annotation_release: <source identifier>
  protein_fasta: <path and sha256>
  genome_fasta: <path and sha256, or unavailable>
  cds_fasta: <path and sha256, or unavailable>
  gene_annotation: <GFF/GTF path and sha256, or unavailable>
  id_mapping_and_isoform_policy: <source and rule>
  ploidy_subgenome_policy: <retain distinct loci; describe provenance>
  annotation_models_without_proteins: <verified count or not checked>
family:
  name: <requested name>
  operational_definition: <what qualifies for this analysis>
  target_level: <structural family / evolutionary subgroup / function-related candidates>
  neighboring_families_or_functions: <comparators and reasons>
  expected_architecture: <evidence-based, allowing supported exceptions>
  reference_manifest: <table of sequence and experimental provenance>
  hmm_manifest: <model accession/version/hash and biological scope>
search:
  blast: <database, E-value, identity/coverage rules, HSP handling, caps, masking>
  hmm: <full target or subset; GA sequence/domain scores or justified alternative>
  candidate_merge: <union/source tags; explicit exceptions>
adjudication:
  historical_rationale: <recorded integrated reasoning, including user clarification>
  rule_provenance: <historically documented / retrospectively validated / prospective>
  rule_registry: <rule ID, predicate or review condition, purpose, evidence source, scope>
  evidence_roles: <required gates / supporting evidence / alternative interpretations / review triggers>
  family_evidence_rule: <criteria and sources>
  integrity_review_triggers: <criteria and rationale>
  functional_subgroup_rule: <required combination of evidence, if needed>
  phylogeny_plan: <references, alignment, model, support, rooting; if needed>
  diagnostic_sites: <reference version/positions and experimental basis>
  genome_rescue_trigger: <missing expected member, suspicious model, or requested completeness>
  project_selection: <requested/adopted scope and decision source>
  ko_evidence: <provider annotation vs new assignment; release, query set, thresholds, formal pass flags>
output:
  new_run_directory: <path>
  requested_deliverables: <tables, sequence types, report, figures>
  comparison_baseline: <previous run or none>
```

## Reference and model records

Reference table: `reference_id`, `accession_version`, `species`, `sequence_sha256`, `length_aa`, `source_url_or_local_file`, `publication`, `paper_label`, `evidence_type`, `tested_substrate_or_process`, `experimental_result`, `reference_role`, `duplicate_of`, `caveat`.

Model table: `model_accession_version`, `name`, `database_release`, `model_length`, `sequence_GA`, `domain_GA`, `file_sha256`, `source`, `scope`, `role` (required / alternative / auxiliary / comparator), `rationale`. Record thresholds from the actual model used. Do not require all alternative models to match one protein.

Integrated-decision table: `candidate_id`, `previous_status`, `requested_status`, `decision_origin`, `rule_ids`, `rule_provenance`, `supporting_evidence`, `alternative_or_conflicting_evidence`, `exception_reason`, `source_pointer`, `date`. Record the user's integrated rationale. Test whether source-backed rules can reproduce an existing list, comparing both included and excluded IDs; label a successful reconstruction as retrospective unless contemporary records establish otherwise. Preserve manual review and list provenance without inferring that human selection lacked evidence.

## Candidate evidence table

Keep one row per protein model during search and adjudication, and a separately defined locus summary for gene counts. Use child tables for multiple HSPs/domains/sites instead of hiding them in a “best hit” column.

Core fields:

```text
gene_id, transcript_id, protein_id, source_release, subgenome_or_haplotype,
sequence_length, sequence_sha256, discovery_sources,
blast_reference_id, blast_bitscore, blast_evalue,
blast_query_coverage, blast_subject_coverage, coverage_method,
hmm_sequence_pass, hmm_domain_pass, hmm_models,
cdd_models, cdd_source_database, cdd_hit_types, domain_architecture,
ko_source, ko_database_release, ko_run_scope, ko_model, ko_score,
ko_threshold, ko_evalue, ko_formal_pass, ko_test_status, alternative_ko_assignments,
family_membership, family_reason, model_integrity, integrity_reason,
functional_group, functional_evidence, functional_reason,
phylogeny_evidence_pointer, residue_evidence_pointer,
project_selected, decision_origin, rule_ids, rule_provenance,
supporting_evidence, alternative_or_conflicting_evidence, exception_reason,
decision_reason, raw_evidence_pointers
```

Use controlled, documented values. For example, `family_membership` may be `supported / unresolved / excluded`, and `model_integrity` may be `complete_supported / partial / uncertain / disrupted / not_assessed`. `functional_evidence` should describe the actual evidence, including `unresolved` and `direct_assay` where applicable; do not label all in-silico assignments “verified”. Project selection and scientific status are independent.

For method execution, distinguish `not_tested`, `tested_not_reported`, `reported_below_threshold`, and `passed_threshold`. Record whether an assignment supports the target, supports an alternative, or remains ambiguous in a separate interpretation field. A second KO can require review without proving a biological contradiction. Keep multiple KO sources in a child table when their results differ.

## Useful deliverables

- `candidate_evidence.tsv`: every reviewed candidate and its disposition.
- `family_supported.*`, `review_candidates.*`, `excluded_candidates.tsv`: explicit sets with reasons.
- `project_selected.*`: the requested/adopted analysis set with evidence tiers intact.
- ID lists and source-identical FASTA for the requested sets; do not manufacture unavailable CDS.
- `methods.md`, reference/model/input manifests, commands/logs, validation results and checksums.
- New-versus-old set differences when updating an existing analysis.

These names are suggestions; use the current project's conventions. A function-priority subset may overlap a family set. A protein can contain supported domains from multiple families. Report unique-locus unions and nested subsets explicitly.

## Acceptance checks

Verify a small set of invariants relevant to the run: unique mappings, no missing selected sequences, source equality, count reconciliation, no unexplained losses between stages, inclusion-threshold parsing, and recorded reasons for overrides. Verify tree tips against the actual tree input, not the full family list. Test new parsers against raw reports and at least one boundary case. Run scripts only into new outputs; historical scripts may overwrite their own files or assert old family-specific counts.
