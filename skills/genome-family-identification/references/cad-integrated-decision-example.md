# CAD: integrated rationale and a validated reconstruction

This is a Coffee ET-39 case, checked on 2026-09-25. It demonstrates how to standardize a decision while retaining its evidence context. Its model IDs and numeric filters are not a universal CAD classifier.

## Historical context

The user clarified that the narrowing from 72 to 17 used CDD, Pfam, KEGG KO annotations, and phylogenetic clustering together. Preserve that integrated rationale. An earlier description only called this a user-selected list and did not capture the biological basis adequately.

The subsequently supplied manual workbook `1-MDR_superfamily_list-20260923.xlsx` records a complementary view starting from the 92-member length-selected MDR pool: 12 `cd05283/CAD1 specific` proteins, 11 formal K00083 assignments, and 12 manually retained CAD candidates. This directly documents the paired annotations and the retained KO exception. It does not make `Specific AND K00083` an exact rule for all 12. See [the joint CAD/ADH reconstruction](mdr-cad-adh-cdd-ko-example.md) for worksheet ranges and the different ADH narrowing step. The 92-member MDR pool and 80-member CAD BLAST pool are different input sets, with 64 proteins in common.

## Retrospective rules that reproduce the lists

Starting with `all_80_CAD_evidence_assessment.tsv`:

| Rule | Exact predicate on the saved table | Result and check |
|---|---|---|
| R1 | `PF08240_PF00107_pair == True` | 72; exact ID-set equality to the historical dual-domain list |
| R2 | Within R1, `CDD_best_model == cd05283` AND `CDD_hit_type == Specific` | 17; exact ID-set equality to the historical shortlist |
| R3 | Within R2, inclusive `250 <= length_aa <= 500` | 12; exact ID-set equality to the adopted final list |

The predicates refer to the saved summary's fields, not an unverified “any CDD hit” interpretation. These are retrospectively validated list-reconstruction rules. Their success establishes that the existing lists can be rebuilt from these fields, not that these were the sole historical considerations. Functional interpretation still uses the other evidence below.

## Evidence roles and observed exceptions

| Evidence | Role in this case | Recorded result |
|---|---|---|
| Pfam | MDR-type domain membership and architecture; R1 reconstruction gate | All 12 have the two targeted domains |
| CDD | More specific conserved-family support; R2 reconstruction gate | All 17 have summary `cd05283/CAD1 Specific` support |
| Length | Project analysis-set filter, R3 | 12 retained; five longer than 500 aa retained in the exclusion audit |
| KO | Functional support and competing interpretations | K00083 formal assignment in 11/12; three also pass K23232 |
| Reference phylogeny/comparison | Functional subgroup interpretation | 2 classical CAD-related, 7 CAD-like, 3 unresolved MDR subgroup |
| Integrity review | Quality of the current protein model | 11 complete-supported; one uncertain |

`Cara014g015390.1` passes the reconstruction gates and stays in the adopted 12. Its K00083 score is 516.3 against threshold 536.60 despite E=3.7e-157, so it is not formally assigned K00083 in this run. Its provider GFF has K00083; keep that source distinct. It remains CAD-like in the prior reference analysis and has `uncertain_domain_integrity`. Record these together; neither silently delete it nor label it a confirmed active CAD.

`Cara022g002010.1`, `Cara013g014310.1`, and `Cara014g015380.1` pass both K00083 and K23232. This warrants an annotation-ambiguity flag, not automatic exclusion or a claim of dual enzyme activity. Specific CDD plus KO support does not replace reference-defined subgroup interpretation.

## Coverage of the KO evidence

The saved new KofamScan run tested the 92-member 300–500-aa MDR set. It covers 64/80 CAD initial candidates and only 12/17 shortlist proteins. The five longer shortlist proteins are outside this KO run. Mark them `not_assessed_in_this_run`, not `below_threshold`. The available run therefore cannot by itself explain every historical 72→17 decision. The user's reference to integrated KO review must remain distinct from claims about this particular saved run; any earlier provider annotation or other KO run needs its own provenance.

## Per-candidate record

Retain `rule_ids`, `rule_provenance`, raw evidence locations, KO source/run scope and status, phylogenetic group, integrity, alternative interpretations, and the final decision reason. For this reconstruction, broad project inclusion follows R1–R3; KO, reference grouping and integrity qualify interpretation rather than acting as additional universal vetoes. A prospective narrow classical-CAD analysis needs its own evidence-based subgroup rules and must not silently replace this project's broader adopted set.

## Original source locations

Paths are relative to the original Coffee `20260921_Coffee_gene_family_etc/` and are optional provenance, not runtime dependencies:

- `CAD_family_belong_toMDR/pfam_full_80_20260925/results/all_80_CAD_evidence_assessment.tsv`
- `CAD_family_belong_toMDR/run_20260923/CAD_dual_domain_gene_list.tsv`
- `CAD_family_belong_toMDR/selected_17_length_250-500aa_20260923/length_screen_selected17.tsv`
- `CAD_family_belong_toMDR/1.verified.12.CAD/12_candidate_CAD_protein_IDs.txt`
- `MDR_family/4.KEGG_KO_annotation/92_MDR_length300_500.kofam.detail.tsv`
- `MDR_family/4.KEGG_KO_annotation/92_MDR_KOfam_vs_Hub_GFF.tsv`

The project-local companion `Gene_family_workflow_skill_20260925/validation/reconstruct_cad_decisions.py` emits an 80-candidate decision trace and the adopted 12's integrated evidence table, with input hashes and exact set checks. It is a bounded audit script for this historical case, not the Skill's general execution pipeline.
