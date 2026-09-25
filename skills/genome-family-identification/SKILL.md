---
name: genome-family-identification
description: 全基因组基因家族鉴定、候选成员复核及方法记录。Identify and audit genome-wide protein-coding gene-family candidates, especially plant families, using literature-curated references, BLASTP, HMM/Pfam, NCBI CDD, KO annotation, gene-model checks, and reference phylogeny. Use for new species or families and candidate-list review. Does not establish enzyme activity from sequence alone or automatically expand into expression, synteny, or promoter analysis.
metadata:
  version: "1.1.0"
---

# Genome-wide gene-family identification

Produce a traceable candidate set whose family definition, sequence provenance, model quality, and functional interpretation are explicit. Support both new discovery and review of existing results. The workflow was distilled from coffee MDR, CAD, ADH, STR, TDC, MATE, and SGD analyses; its example thresholds are not universal biological rules.

## Establish the task

Read the project's current methods, adopted lists, and input manifests before choosing a workflow. For an audit, reconstruct what was actually done; do not describe a recommended step as completed. Reuse valid outputs where inputs and parameters match.

Record user-supplied historical decision context. Distinguish the historical integrated rationale, a retrospectively validated rule that reproduces its ID set, and a prospective rule for new data. Missing executable code does not imply absent biological rationale; exact list reconstruction does not establish that only those filters were used historically.

Resolve the species, assembly/annotation release, input proteome, target family definition, and intended output. Ask only when an unresolved choice changes the scientific question or source data. Infer routine execution details from the project and state material assumptions. Record these choices using [the run specification](references/run-specification.md).

Define whether the target is a broad structural family, a narrower evolutionary group, a function-related group, or an experiment-priority list. A user-adopted set may span several evidence levels. Preserve that set and annotate the levels; do not silently replace it with a narrower automated set.

For a new run, use a new output directory and record inputs, software/database releases, commands, parameters, and SHA-256 hashes. Do not import fixed Coffee paths, ID suffix rules, expected counts, manual selections, or absolute protein lengths into another species.

Before execution, locate the available software, database releases, and compute resources. Adapt commands to that environment and record missing requirements instead of assuming the Coffee software paths exist. This skill provides the workflow and decision rules; sequence databases and analysis tools are selected for each run. Write user-facing methods and results in the user's language while preserving scientific identifiers and machine-readable field names.

## 1. Audit references and target data

- Use primary literature, supplements, and authoritative sequence/model records. Save exact accession and version, species, isoform, sequence hash, citation, and the type of functional evidence. Distinguish an author's label, database prediction, direct biochemical evidence, and in vivo evidence.
- Include references spanning relevant family branches. For a narrow enzyme group, add related proteins with characterized alternative substrates or weak activity. Such comparators are not automatically negative controls.
- Check query species, duplicate sequences, historical identifiers, fusions, fragments, and whether the cited accession matches the protein tested in the paper. Keep duplicate provenance even if identical sequences are collapsed for searching.
- Check target FASTA IDs and sequences and its relationship to genome, CDS, and GFF/GTF releases. Obtain gene–transcript–protein mappings from annotation or a validated explicit mapping; do not universally strip the final dot suffix. Count proteins and loci separately, retain distinct homeolog/paralog loci and unplaced scaffolds, and do not collapse genes just because their proteins are identical.
- Search all available annotated proteins where practical, then resolve isoforms at the locus level. If using one representative per locus, document its selection and check alternate isoforms for missed or disrupted family domains when relevant. State how many annotation models lack exported proteins. If genome/GFF data are unavailable, limit completeness claims accordingly.

## 2. Discover candidates through complementary searches

Use curated-reference BLASTP and suitable HMM profiles against the target proteome as complementary discovery routes when both are available. Retain their union and label each source, intersection, and method-only set for review. If no suitable profile exists, document that limitation and use an appropriate homology strategy; a custom HMM needs a reviewed seed alignment and validation before being treated as equivalent to a curated model.

Choose BLAST thresholds for the family, taxonomic distance, and retrieval purpose. Do not copy a high identity cutoff from another species. Save all relevant HSPs, query/subject lengths and coordinates, identity, bit score, E-value, and an explicit coverage definition. Check whether output caps truncated the result set. Rank very strong hits by bit score rather than treating printed E=0 as an exact probability.

For profiles with curated GA thresholds, normally use `--cut_ga`, recording both sequence and domain thresholds and model accession/version. Without GA, justify the chosen reporting and inclusion rules with references and comparators. Save the main report, `tblout`, `domtblout`, logs, and model metadata. Distinguish reported sequence hits, included domains, proteins with qualifying domains, and gene loci. Parse `hmmsearch` and `hmmscan` with their different query/target orientations; do not count every reported table row as an included domain.

Avoid a universal BLAST∩HMM membership rule. Review one-method candidates and conflicting evidence. Whole-proteome targeted HMM discovery and full-Pfam scanning of an already selected candidate set have different scopes; the latter cannot recover proteins absent from its input.

## 3. Review domain evidence and model integrity

Use candidate-level full-library domain annotation and CDD/other appropriate models when they resolve family identity, competing interpretations, or domain architecture. Choose local versus remote tools according to available data and execution authorization. Record model provenance, coordinates, scores, thresholds, and CDD hit type. Imported Pfam records in CDD and a second scan using the same Pfam model are correlated evidence, not independent functional confirmation.

Check which regions support the proposed family and competing families. Overlapping models may describe the same domain. Split alignments may describe one interrupted domain. Do not infer the number of complete domains from hit-row counts alone. A broad superfamily model or Rossmann hit does not establish a narrow enzyme family.

Assess integrity separately: domain order, repeats/fusions, terminal and internal losses, non-gap alignment coverage, internal stops, unusual lengths, and reference-specific insertions. Compare candidates and appropriate references on the same models. Length and coverage anomalies trigger review; they are not universal exclusion rules. A short domain can cover its HMM completely while the overall protein remains incomplete.

For anomalies that affect the conclusion, inspect genomic sequence, exon structure, CDS reconstruction/translation, and available transcript evidence. Source agreement proves consistency with the annotation, not biological completeness or activity. Keep repaired or proposed models separate with explicit provenance. Do not call a pseudogene, correct a published sequence, or merge adjacent loci solely from a protein-domain anomaly.

Read [evidence interpretation](references/evidence-interpretation.md) when implementing parsers, reviewing unusual models, or reconciling conflicting classifications.

## 4. Resolve subgroups when the question requires it

For function-related subgroups, use phylogeny containing accession-verified functional references and close alternatives, informative residue mapping, and integrity evidence. A candidate-only tree describes relationships within the candidates; it cannot by itself assign reference-defined function. For a broad structural-family inventory, a functional tree is conditional rather than a mandatory cosmetic step.

Assign evidence roles before combining results: Pfam supports domain membership/architecture; relevant CDD models refine conserved-family identity; reference phylogeny supports subgroup placement; KO annotations provide functional support and alternative interpretations. Use family-specific required gates, supporting evidence, review triggers, and explicit exception reasons. Do not reduce integrated judgment to a vote count or require every annotation to agree.

When KO is relevant, distinguish provider annotations from newly computed assignments, record the database/version and actual query set, and retain formal model-threshold results separately from exploratory hits. For each method, distinguish not tested, tested but not reported, reported below threshold, passing support, and a supported alternative requiring review. Evidence only obtained for a later subset cannot explain earlier exclusions outside that subset. See [the CAD decision example](references/cad-integrated-decision-example.md) for a validated reconstruction and evidence conflicts.

Inspect alignment quality before tree inference; consider homologous-domain trees for fusions or fragments. Save untrimmed and trimmed alignments and record taxa, excluded sequences, trimming, model selection or the actual fixed model, seed, support method, and rooting. Record unsuccessful steps honestly. Label FastTree SH-like support, SH-aLRT, and bootstrap as their actual statistics.

Map functional sites using an alignment to a named reference accession/version, retaining reference position, candidate position, residue, and gap/ambiguity state. Raw residue numbers are not transferable between proteins. Conserved sites support compatibility; nearest BLAST hit, a tree branch, KO label, or conserved residues alone cannot prove substrate specificity. An atypical residue may warrant a broader comparator set or experimental prioritization, rather than automatic exclusion.

When a candidate exactly matches an experimentally used protein sequence, verify the match and source experiment and preserve that evidence at its actual level: heterologous pathway support, direct enzyme assay, or endogenous function. Do not downgrade source-backed experimental evidence merely because a similarity ranking favors another reference. New evidence can require a proposed priority update while the previously adopted list remains a historical artifact.

For an experiment-priority set, allow separately justified routes such as exact experimental-sequence correspondence, close homologs of that sequence, and comparative sequence prioritization. Record route precedence and overlaps instead of requiring every route to pass the same similarity filter. Close homologs do not inherit the reference's experimental evidence. Version the reference panel and reassess rankings when relevant lineages or functional alternatives are added. If characterized target enzymes are interspersed with other activities, do not impose a target-only monophyletic branch. Read [the SGD tiering example](references/sgd-evidence-tiering-example.md) for this case and the distinction between executable selection gates and supporting review evidence.

Use sensitivity analyses only for consequential uncertainty: omitted fragments, questionable references, alternative domain regions, or competing subgroup placements. Family membership and inclusion in a particular phylogenetic dataset are separate decisions.

## 5. Adjudicate and deliver

Maintain independent fields for:

| Field | Question |
|---|---|
| `family_membership` | Is the specified broad family supported, unresolved, or excluded? |
| `model_integrity` | Is the current model complete-supported, partial, anomalous/uncertain, or disrupted? |
| `functional_evidence` | Which narrower function or subgroup is supported, unresolved, or experimentally demonstrated, and by what evidence? |
| `project_selected` | Is the candidate included in the set requested/adopted for this project? |
| `decision_origin` | Did the choice follow a recorded rule, a source-grounded review, or an explicit user selection? |

Retain per-candidate rule IDs, rule provenance, supporting evidence, conflicting or alternative interpretations, exception reasons, and raw evidence pointers, including excluded and unresolved candidates. Preserve distinctions between “not searched”, “not detected under these criteria”, and “excluded with positive contrary evidence”. Do not sum nested or overlapping families without locus-level set accounting. Handle user-selected candidates without upgrading their scientific confidence.

For new discovery, deliver an evidence table, family-supported and review/excluded lists, the requested project set, protein sequences, and CDS/genomic sequences when available and in scope. Include the methods, input/query/model manifests, actual commands, provenance, and a concise explanation of count definitions and limitations. A review-only request may only need a findings report and change recommendations.

Check IDs/mappings and sequence identity against sources, unique locus counts, set differences and intersections, per-step inclusions/exclusions, and tree-tip/sequence-set agreement. Reconstruct CDS and check translation where supported inputs exist. Check that any ranking or label can be traced to its documented rule; keep manual decisions in a separate table. File checks demonstrate computational consistency, not biological accuracy. Report unperformed genome-rescue, missing references, and unresolved models without claiming exhaustive recovery of all functional genes.

Use [coffee examples](references/coffee-examples.md) only for the relevant family or an analogous failure mode. Use [the run specification and output fields](references/run-specification.md) when starting a run; adapt them to the existing project rather than imposing a new folder hierarchy. Downstream motif discovery, synteny, expression, promoters, and selection analyses require their own task scope.
