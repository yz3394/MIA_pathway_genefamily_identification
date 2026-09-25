# Coffee GH1 → SGD: evidence tiers and experimental priorities

Use this example when narrowing a broad enzyme family whose related proteins have overlapping substrate activities. Verified against the Coffee project's saved tables, executable rules, FASTA files, and trees on 2026-09-25. This is a self-contained method example; the original data are optional provenance and are not bundled with the Skill. Its thresholds and expected counts apply to this historical run only.

## 1. Establish the broad family before narrowing function

BLASTP and PF00232.25 HMM searches recovered the same 86 GH1 proteins/loci from the ET-39 annotated proteome. Pfam and CDD supported GH1 membership. They did not distinguish strictosidine glucosidase activity. Preserve all 86 in the family inventory even when selecting fewer proteins for experiments or alignment.

## 2. Build a reference panel with explicit evidence roles

The original six-reference comparison panel contained CrSGD `AAF28800.1`, RsSGD `Q8GU20`, and four enzymes with other principal substrates: RsRG `Q9SPP9`, OeGLU `Q8GVD0`, VfFH `Q75W17`, and CiIpeGLU1 `B6ZKM3`.

The eight-reference panel added two Rubiaceae proteins:

- **MsSGD1 `WNF20694.1`**, linked to nucleotide `OP800437.1`: strictosidine conversion supported by the [peer-reviewed Wu et al. study](https://pmc.ncbi.nlm.nih.gov/articles/PMC10530425/). The project records an engineered-yeast lysate assay, not purified-enzyme kinetic constants.
- **MsSGD2 `WNF20695.1`**, linked to `OP800438.1`: low conversion reported in the [same authors' earlier preprint](https://pmc.ncbi.nlm.nih.gov/articles/PMC9882157/). Keep this evidence separate from the published MsSGD1 assay and label MsSGD2 a weak-activity comparator.

Alternative-principal-substrate enzymes and weak-activity comparators are not necessarily strictosidine-negative controls. RsRG and CiIpeGLU1 have reported strictosidine activity. Avoid turning assay-specific conversion percentages into universal activity thresholds.

Adding MsSGD1/2 changed the number whose nearest sufficiently covered reference was a strong SGD reference from 2 to 24 (23 nearest MsSGD1, 1 nearest RsSGD). This is reference-sampling sensitivity, not evidence that 22 new functions were experimentally established.

**CarSGD was a separate exact-sequence evidence route.** `XP_027073002.1` is associated with CarSGD in [US20220228180A1, Example 7](https://patents.google.com/patent/US20220228180A1/en), which reports tetrahydroalstonine production in an engineered yeast pathway. The complete 536-aa sequence is identical to `Cara012g004270.1`. Record heterologous-pathway support; do not claim purified-enzyme kinetics or native coffee function. This accession was checked separately and was not a ninth sequence added to the eight-reference tree panel.

## 3. Reconstruct the implemented, ordered selection rules

The current evidence-table builder evaluates A, then B, then C, otherwise D. The resulting tiers are mutually exclusive, although evidence routes can overlap before precedence is applied.

| Tier | Executed condition in this run | Result and interpretation |
|---|---|---|
| A | Full-length, residue-by-residue equality to the CarSGD reference FASTA | 1: `Cara012g004270.1`; same sequence with heterologous-pathway evidence |
| B | Not A; CarSGD best-HSP identity ≥90% and best single PF00232 model coverage ≥95% | 2: `Cara011g034180.1`, `Cara012g004300.1`; untested close homologs |
| C | Not A/B; nearest sufficiently covered reference is MsSGD1; best single PF00232 model coverage ≥80%; all four mapped RsSGD reference residues identical; strong-SGD minus comparison-reference best-HSP score ≥20 bits | 5 untested candidates for secondary experimental priority |
| D | Remaining family members | 78 with unresolved SGD function; not activity-negative |

C contains `Cara001g007250.1`, `Cara002g030050.1`, `Cara002g030980.1`, `Cara017g009040.1`, and `Cara018g010230.1`. Its sequential count reconstruction is **23 nearest MsSGD1 → 15 with domain coverage ≥80% → 10 with four residues conserved → 5 with score margin ≥20 bits**. Five candidates passing the first three conditions have margins 1–17 bits: the 20-bit cutoff is a project prioritization choice, not a biological boundary.

The 8-member working list is A∪B∪C. Only A has this exact-sequence experimental connection; B and C remain function-unresolved. D retains possible SGD activity and is not a definitive exclusion list. Never force another species to yield 1/2/5/78 or exactly eight priorities.

### Coverage and score definitions matter

- For each candidate/reference pair, the analysis keeps the highest-bitscore HSP.
- The field `best_full_length_reference` actually means the highest-scoring pair whose best HSP spans ≥70% of both the query and reference coordinate lengths. It does not mean exact full-length alignment.
- `SGD_minus_other_bitscore` compares the strongest SGD-reference HSP with the strongest comparison-reference HSP. These two scores were computed before applying the dual-70% filter; do not describe this margin as coverage-filtered.
- Tier B's historical code did not explicitly gate on CarSGD alignment coverage. Its two selected proteins happen to cover 100% of the reference and 99.06–100% of their own sequences, with identities 96.455% and 92.407%. For a new run, define and check both alignment coverages as well as identity. Do not transfer the old identity/domain rule as a universal near-full-length homology test.
- Best single-domain model coverage, union model coverage across hits, protein coverage, and HSP identity have different denominators. Retain those definitions in outputs.

## 4. Use phylogeny and integrity to review the tiers

The main tree included 50 coffee proteins meeting this run's 300–700-aa and best PF00232 coverage ≥80% criteria, plus eight references. The other 36 remained in the GH1 inventory. Sensitivity trees included 71 length-qualified proteins and, separately, all 86 proteins represented by 96 GH1 fragments. Fragment count is not gene count.

Main IQ-TREE branch support (SH-aLRT / ultrafast bootstrap):

- CrSGD + RsSGD: 100/100, no coffee protein.
- MsSGD1 + the alternative-principal-substrate RsRG: 94.7/95.
- The three CarSGD-related coffee proteins: 97.3/99; with weak-activity MsSGD2: 99.9/100.

These observations do not define a substrate-exclusive SGD clade. In the broader FastTree analyses, the branch joining those three proteins and MsSGD2 also contains two shorter coffee models; do not describe all tree variants as recovering an identical exclusive three-protein group. FastTree SH-like support is not bootstrap or a probability of enzyme function.

All three CarSGD-related proteins favor MsSGD2 over the strong-SGD panel by 190–202 bits. Applying C's positive score margin to every route would wrongly discard A's exact-sequence experimental connection and the associated B priorities. Sequence proximity to a weak-activity reference does not determine the candidate's activity.

RsSGD reference sites H161/E207/W388/E416 are mapped from the stated reference construct, not copied as raw coffee residue positions. All four are retained in 31 of 86 proteins; they are not an SGD-specific signature. Conversely, 122–139-aa fragments can cluster near MsSGD1 despite only 15–29% PF00232 coverage. Route these to model review and retain their family evidence.

The selected eight have 473–562-aa proteins, one qualifying PF00232 domain, all four reference residues, and no flags under the project's model-screening rules. These observations support review of the selected set. **Tree inclusion, branch support, length, and the integrity label were not Boolean gates in the A/B/C classifier.** The builder checks source CDS/protein consistency for all 86; such consistency does not prove biological model completeness.

## 5. Transfer the method, with evidence boundaries

1. Define the broad inventory, the intended functional claim, and any experimental-priority output separately.
2. Verify experimentally studied accessions and constructs; separate strong target activity, weak activity, other main substrates, and untested annotation. Add relevant lineages when available and version the panel.
3. Check exact sequence correspondence to study records, then evaluate close homologs and comparative priorities as distinct routes. Specify precedence or retain overlaps explicitly.
4. Choose coverage, domain, residue, and ranking criteria for the current references and taxonomic distance. Preserve their roles as gates, support, or review triggers. Do not turn a score margin into evidence of inactivity.
5. Review alignment, phylogeny, and model completeness. Inspect consequential excluded fragments or alternative references; do not require target-only monophyly when characterized activities are interspersed.
6. Deliver the broad family list and the evidence-tiered working set, with exact rules, conflicts, reference-panel version, and unresolved candidates. An experimental priority is not an exhaustive inventory of the enzyme activity.

## Original project provenance

Paths are relative to `20260921_Coffee_gene_family_etc/SGD_family/`:

- `SGD_identification_record_20260925.md`
- `5.SGD_specific_analysis/01_references/reference_evidence.tsv`, `README.md`, `patent_CarSGD_evidence.md`, and `XP_027073002.1.fasta`
- `5.SGD_specific_analysis/03_sequence_features/compare_verified_references.py` and `CarSGD_XP_027073002_matches.tsv`
- `5.SGD_specific_analysis/04_integrated/build_sgd86_evidence.py` and `SGD86_sequence_evidence_with_MsSGD.tsv`
- `5.SGD_specific_analysis/02_alignment_phylogeny/key_clades.tsv` and `all86_sensitivity/README.md`
- `6.GH1_and_SGDpecific.list/` and `6.SGD.list/`

Audit input SHA-256: integrated table `f318a15853fae44b5e38032789e80e88adc9766e16bccc652ab5bc3787d3a988`; classifier script `79774fc2e185c938e83768a3732e250f3f3889e0d84802a0e55c1ed3460fb4d2`. These identify the reviewed snapshot, not a dependency required on a new machine.
