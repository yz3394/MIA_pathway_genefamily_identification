# Coffee case examples: transferable decisions

These are snapshots verified from project files on 2026-09-25, not default parameters or target counts for future species. Source paths below are relative to the original Coffee project's `20260921_Coffee_gene_family_etc/`. This reference is self-contained; those local files are optional provenance, not a runtime dependency of the skill.

| Family | Actual project result | Decision to preserve when generalizing |
|---|---|---|
| MDR | Whole-proteome PF08240/PF00107 GA dual-domain set 105; user-adopted 300–500-aa subset 92. Separate curated CDD set 143, integrity-supported subset 98; 92∩98=80. | A length-selected analysis set and an integrity-supported set have different definitions. Two-domain detection is narrower than some broader MDR definitions. |
| CAD | AtCAD references → BLAST 80 → dual domains 72 → integrated review of 17 → length-selected 12. A separate manual MDR92 view has 12 `cd05283 Specific` and 11 formal K00083 assignments, retaining all 12. | Preserve integrated rationale and distinct input universes. Keep 2 classical CAD-related, 7 CAD-like, 3 unresolved and the KO/integrity exception. See [the CAD reconstruction](cad-integrated-decision-example.md) and [paired CDD/KO audit](mdr-cad-adh-cdd-ko-example.md). |
| ADH | BLAST 75; dual-domain 63; length subset 58; integrated review → 10 complete classical-ADH candidates. In the manual MDR92 view, `cd08301 Specific` and formal K18857 identify the same 14, including 4 ADHL3-like proteins retained separately. | Both annotation passes, complete models, and 9/9 Zn sites still give 14. Reference phylogeny/comparison resolves the strict 10-member subset; distinguish it from GSNOR and ADHL3-like candidates. See [the paired CDD/KO audit](mdr-cad-adh-cdd-ko-example.md). |
| STR | Strict high-identity BLAST 6; broader BLAST 55; HMM 50; integrated working list 49. | PF03088 defines STR/SSL-domain candidates. Its model length is only 89 positions in this run; high model coverage is not proof of full-protein completeness or STR catalysis. |
| TDC | Broad BLAST 23; full-proteome PF00282 26; combined reviewed pool 35. Adopted set 11 = 5 high-priority TDC-like + 5 TYDC/PAAS-related AAAD + 1 model-review candidate. | PF00282 has broader scope than TDC. Query audit, close functional alternatives, and model integrity are essential. Preserve the project's 11-member scope and its subdivisions. |
| MATE | Strict BLAST 57; broader BLAST 105; HMM sequence hits 102, qualifying-domain proteins 101; adopted set 101. | Identity >60% missed domain-supported candidates. Keep sequence-only and domain-supported sets distinct; no reference phylogeny is documented in this final workflow. |
| SGD | BLAST and PF00232 yield the same 86 GH1 candidates. The updated working set has 8: 1 exact sequence linked to CarSGD heterologous-pathway evidence + 2 untested CarSGD close homologs + 5 untested MsSGD1 similarity priorities. The other 78 remain function-unresolved; the 7 untested priorities also lack functional confirmation. | Use distinct evidence routes and preserve all unresolved members. Reference-panel changes affect ranking; nearest-reference labels cannot override exact-sequence experimental correspondence. See [the detailed SGD tiering example](sgd-evidence-tiering-example.md). |

## Reference problems worth checking in new families

- TDC: the initial 14 “TDC” literature records included TYDC, uncertain AAADs, DUF674 proteins, a truncated model, and an assay-negative case. DUF674 queries recruited 9 non-AAAD candidates. Audit each reference's tested sequence and function.
- MATE: a file labeled as Arabidopsis queries also contained rice sequences. Check species against records, not filenames.
- SGD: seven reference records contained only five distinct sequences because three PDB chains were identical. Repeated chains are not independent evidence.
- CAD: a repeated At4g34230 entry represented promoter constructs, not two protein-coding loci.

## Profile starting points, to verify anew

| Target | Models used in these cases | Scope limitation |
|---|---|---|
| MDR / ADH / CAD | PF08240.18 + PF00107.33 | Shared MDR architecture; narrower function needs additional evidence. Consider alternative MDR models when defining a broad search. |
| STR/SSL | PF03088.23 | Broad STR-like group, not verified strictosidine synthases. Auxiliary models must be justified for the new target. |
| TDC/AAAD | PF00282 | Broad decarboxylase-related family; include alternative activities and distant model members for interpretation. |
| MATE | PF01554.24 | Domain-family detection; substrate transport specificity and model completeness remain separate. |
| GH1/SGD | PF00232.25 | GH1 detection; a general GH1 match cannot identify SGD activity. |

GA scores, model versions, full-library releases, BLAST thresholds, domain combinations, and any length/coverage rules must come from the current run specification. The historical profiles do not define a universal complete model collection.

## Integrity and inference counterexamples

- MDR: a sequence-level PF08240 pass without a qualifying domain must not enter the dual-domain set. The 92-member length set still contains 12 models labeled integrity-uncertain in an independent review.
- TDC `Cara010g013980.1`: source genome/GFF/CDS/protein agree, yet a central catalytic region is absent. Source consistency alone does not establish a complete functional protein; phylogenetic lineage should not be replaced by the name of a nearest alternative BLAST hit.
- MATE: `ptg000035lg000130` is an unplaced locus with domain support. An unfamiliar ID prefix is not an exclusion criterion.
- SGD: short fragments can cluster near verified SGD references. Tree support does not restore missing catalytic or binding regions. Verified SGD references were not a substrate-exclusive monophyletic group in these analyses.
- SGD `Cara012g004270.1`: its 536-aa sequence exactly matches `XP_027073002.1`, linked to CarSGD in published patent US20220228180A1. The patent reports product formation in an engineered yeast pathway. This supports a heterologous pathway role, without establishing purified-enzyme kinetics or the native substrate in coffee. It is absent from the earlier five assay priorities; nearest-reference scores must not erase this additional evidence. See [the patent, Example 7](https://patents.google.com/patent/US20220228180A1/en), checked 2026-09-25.

## Source pointers

- `MDR_family/MDR_鉴定过程与最终结果.md`
- `1-MDR_superfamily_list-20260923.xlsx` (manual CDD/KO and phylogeny shortlist record)
- `CAD_family_belong_toMDR/CAD_基因家族鉴定过程与最终12条_20260925.md`
- `ADH_family_belong_toMDR/ADH_鉴定过程与最终结果.md`
- `STR_family/STR_family_identification_record.md`
- `TDC_family/README.md`; `TDC_family/5.TDC.gene.family/README.md`; `TDC_family/3.query_audit.TDC/README.md`
- `MATE_family/README.md`; `MATE_family/4.verified.MATE/README_final_gene_list.md`
- `SGD_family/README.md`; `SGD_family/5.SGD_specific_analysis/04_integrated/SGD86_sequence_evidence_with_MsSGD.tsv`
- `SGD_family/SGD_identification_record_20260925.md`; `SGD_family/6.SGD.list/README.md`; `SGD_family/5.SGD_specific_analysis/04_integrated/build_sgd86_evidence.py`
- `SGD_family/5.SGD_specific_analysis/01_references/patent_CarSGD_evidence.md`; `SGD_family/5.SGD_specific_analysis/01_references/XP_027073002.1.fasta`
- `过程/domain_reaudit_20260921/mdr_audit/MDR_CDD_independent_confirmation.md`
