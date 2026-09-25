# MDR → CAD and plant ADH: paired CDD/KO evidence

Use when distinguishing CAD-related and plant-ADH-related candidates within an MDR superfamily pool, or auditing a manual classification. This Coffee ET-39 example was checked against a user-supplied workbook and saved annotation tables on 2026-09-25. Model accessions, KO thresholds, length ranges, and counts are historical settings to verify for a new run.

## 1. What the manual record establishes

`1-MDR_superfamily_list-20260923.xlsx` contains a 92-protein MDR set selected at 300–500 aa, CDD results, KofamScan annotations, manual CAD labels, and an ADH reference-tree shortlist. Original cell values and accession/hit-type columns were read directly; cell colors and broken lookup cells were not used to infer membership.

| Evidence within the same MDR92 pool | CAD-related | Plant-ADH-related |
|---|---|---|
| Target curated CDD model | `cd05283 / CAD1` | `cd08301 / alcohol_DH_plants` |
| Required CDD hit type for these sets | `specific` | `specific` |
| CDD-supported proteins | 12 | 14 |
| Relevant KO | `K00083`, cinnamyl-alcohol dehydrogenase | `K18857`, alcohol dehydrogenase class-P |
| Formal Kofam assignments | 11 | 14 |
| CDD ∩ KO | 11 | 14 |
| CDD ∪ KO | 12 | 14 |
| Adopted project set | 12 broad CAD-related candidates | 10 complete classical plant ADH candidates |

These pairs accurately identify the annotations used in the manual review. Their different set relationships prevent a universal `CDD AND KO = final family` rule. The workbook records support for the historical interpretation; it does not timestamp every manual choice or establish that no other evidence was considered.

Both K00083 and K18857 use domain scores in this saved Kofam model release. Compare each KO's selected score type to its own threshold rather than assuming all KO models use a full-sequence score.

## 2. CAD: retain the documented KO exception

The 12 `cd05283 specific` proteins are exactly the 12 manually labeled CAD proteins and the saved final CAD list. All 11 K00083-positive proteins belong to those 12. There is no KO-only CAD candidate in this pool.

`Cara014g015390.1` is the exception:

- `cd05283/CAD1 specific` support and both targeted MDR Pfam domains.
- K00083 score **516.3 < 536.60**, E-value `3.7e-157`; no formal Kofam assignment in this run.
- The source-provider GFF carries K00083, which is a different annotation source.
- Retained in the broad CAD set, with prior CAD-like interpretation and `uncertain_domain_integrity`.

An AND gate would remove this adopted member. Preserve its CDD support, failed Kofam threshold, provider annotation, model flag, and explicit retention reason as separate fields. A small E-value does not turn it into a formal KO pass.

`Cara013g014310.1`, `Cara014g015380.1`, and `Cara022g002010.1` pass both K00083 and K23232 (8-hydroxygeraniol dehydrogenase). Retain both annotations without claiming demonstrated dual activity. The per-protein `best_pass_score` can belong to K23232; retrieve the actual K00083 score from the protein–KO detail row when comparing against its threshold.

The adopted 12 comprise 2 classical CAD-related, 7 CAD-like, and 3 unresolved MDR-subgroup candidates in the prior reference analysis. The broad project label therefore has a different scope from a strict inventory of canonical lignin-pathway CAD enzymes.

The separate CAD-query route starts from 80 BLASTP candidates, of which 72 have the target domain pair; the saved `cd05283 Specific` summary reconstructs 17, then 250–500 aa retains 12. MDR92 and CAD80 intersect in only 64 proteins. The five longer members of the 17 were outside the MDR92 KO run. See [the CAD decision example](cad-integrated-decision-example.md); do not concatenate these two input universes as though every step processed the same pool.

## 3. ADH: annotation agreement identifies 14, reference review narrows to 10

The workbook's CDD-specific list and formal K18857 list contain exactly the same 14 proteins. Its reference-tree column contains 10, identical to the explicitly labeled final list and the saved complete classical-ADH set.

The four additional candidates are:

| Protein | K18857 score (historical threshold 650.87) | Prior reference-supported interpretation |
|---|---:|---|
| `Cara013g025480.1` | 660.4 | ADHL3-like |
| `Cara014g004260.1` | 660.6 | ADHL3-like |
| `Cara015g026610.1` | 652.6 | ADHL3-like |
| `Cara016g000670.1` | 651.5 | ADHL3-like |

All four also have the MDR domain pair, `complete_supported` models, full CDD-model coverage, and **9/9 AtADH1-aligned Zn-ligand matches**. Adding those gates to the annotation intersection still gives 14. They are not removed for incomplete models or lost Zn sites.

Their best reviewed reference and nearest tree reference are `UP_A1L4Y2 / ADHL3_ARATH`. The project's reference metadata labels it `uncharacterized_ADH_like_not_canonical_ADH`. The saved classifier requires placement in the canonical reference clade and agreement with the best reviewed-reference group for a classical-ADH call. The four are kept separately as ADHL3-like candidates, recorded under `other_MDR` for the narrow classification. That category does not prove absence of alcohol-dehydrogenase activity.

The final 10 have classical-reference placement, compatible reference similarity, domain/site evidence, and complete-supported models. The saved tree statistic is FastTree **SH-like local support**, not bootstrap or a functional probability. The lower K18857 scores of the four ADHL3-like proteins are an observation, not an independently validated higher-score cutoff; do not invent one to force 14 into 10.

The earlier broad ADH recommendation contained 11 = these 10 + `Cara013g019550.1`, a 232-aa partial model. That fragment is CDD Non-specific and was outside the MDR92 KO input. Preserve it in the model-review inventory rather than calling it a tested KO failure. Its existence also explains why the historical 11→10 completeness review and the manual 14→10 subgroup review are different comparisons.

### Keep class III ADH / GSNOR and multiple KOs explicit

`Cara001g028380.1` and `Cara002g008290.1` have `cd08300 / alcohol_DH_class_III specific` support and formal K00121. Their K18857 scores, 648.6 and 650.1, fall below 650.87. Keep them in the separately interpreted class III ADH/GSNOR group for this narrow task.

Conversely, three members of the final classical ADH set—`Cara014g010450.1`, `Cara019g024740.1`, and `Cara020g003480.1`—pass both K18857 and K00121. Therefore K00121 presence alone cannot exclude classical ADH or assign GSNOR function. Use domain subtype, reference context, and the actual scope of the requested family.

## 4. Prospective implementation

1. **Fix the input pool.** Record proteome release, locus/isoform handling, and any prior length or integrity selection. A length-selected MDR subset is not an exhaustive proteome-wide functional inventory.
2. **Extract exact CDD evidence.** Match the model accession and normalize the hit-type field explicitly. Never test whether the string merely contains `specific`, because `non-specific` contains it too. Preserve CDD version, query/model coordinates, score, coverage, and model provenance. A generic MDR superfamily hit or an imported Pfam record has a different role.
3. **Extract each formal KO assignment.** For KofamScan use its threshold-qualified output (`*` in this saved detail format), retaining KO ID, score type, model threshold, and KO-specific score. Keep below-threshold, not-tested, and provider-only results distinct. Preserve multiple passing KOs.
4. **Compare sets before choosing gates.** Report CDD-only, KO-only, concordant, and unassessed candidates within the same pool. Concordance supplies annotation support; differences trigger review. Adopt an AND rule only if justified for the intended subgroup, and report documented exceptions.
5. **Resolve narrower reference-defined groups.** Compare CAD versus CAD-like branches, classical plant ADH versus ADHL3-like and class III/GSNOR groups, as appropriate. Record curated reference accessions, experimental status, tree placement, alignment/sites, model integrity, and conflicts. Shared motifs and complete models do not by themselves resolve these functions.
6. **Keep the decisions separate.** Output broad-family membership, CDD/KO candidate branch, narrower reference group, integrity, project inclusion, and reason. Retain reviewed alternatives and fragments. Exact reconstruction of a historical list does not establish experimental activity or universal threshold validity.

## Source provenance

Original project paths below are relative to `20260921_Coffee_gene_family_etc/`; they are optional provenance, not required on a new machine.

- Manual workbook: `1-MDR_superfamily_list-20260923.xlsx`.
  - CAD labels: `MDR gene list!A1:C13`; formal CAD KO rows: `CAD KEGG!A1:M12`.
  - ADH CDD shortlist: `ADH!A1:B15`; tree shortlist: `ADH!C1:D11`; adopted list: `ADH!A19:A29`.
  - Formal ADH KO rows: `ADH KEGG!A1:M15`; specific/non-specific source distinction: `ADH CDD!A1:I36`.
  - Interpretation summary: `CDD annotation!A1:C4`; complete annotation evidence: `CDD!A1:O888` and `92_MDR_KO_by_protein!A1:M93`.
- Original MDR92 CDD export: `MDR_family/92_MDR_length300_500.protein-hitdata.txt`.
- Kofam detail and summaries: `MDR_family/4.KEGG_KO_annotation/92_MDR_length300_500.kofam.detail.tsv`, `92_MDR_KO_by_protein.tsv`, `92_MDR_KOfam_vs_Hub_GFF.tsv`.
- Independent CDD/integrity table: `过程/domain_reaudit_20260921/mdr_audit/MDR_CDD_integrated_all_193.tsv`.
- Prior reference interpretation: `过程/run_20260921_ET39/05_classification/mdr_analysis/MDR_reviewed_classification.tsv` and `coffee_Zn_binding_residue_summary.tsv`.
- Adopted lists: `CAD_family_belong_toMDR/1.verified.12.CAD/12_candidate_CAD_protein_IDs.txt`; `ADH_family_belong_toMDR/4.Pfam_full_75_20260925/canonical_ADH_high_confidence_10.protein_ids.txt`.

The workbook has pre-existing `#REF!` values in annotation-note columns and `#N/A` at `MDR gene list!C10`, where a lookup cannot find the CAD candidate without a formal KO. These were cross-checked against raw annotations rather than interpreted as biological exclusions. The original workbook was not edited. A local bounded audit records all 92 candidate decisions and hashes of the reviewed inputs.

Official definitions: [CDD CAD1](https://www.ncbi.nlm.nih.gov/Structure/cdd/cddsrv.cgi?uid=cd05283), [CDD plant ADH](https://www.ncbi.nlm.nih.gov/Structure/cdd/cddsrv.cgi?uid=cd08301), [CDD hit interpretation](https://www.ncbi.nlm.nih.gov/Structure/cdd/cdd_help.shtml), [KEGG K00083](https://www.genome.jp/entry/K00083), [KEGG K18857](https://www.genome.jp/entry/K18857), and [KofamScan assignment rules](https://github.com/takaram/kofam_scan). These model/orthology descriptions support annotation within their scope; they do not substitute for enzyme assays of the coffee proteins.
