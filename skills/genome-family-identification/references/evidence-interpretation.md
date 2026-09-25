# Evidence interpretation and implementation traps

## HMMER

`hmmsearch` queries profile models against a sequence database; `hmmscan` queries sequences against a profile database. Their table query/target identities are reversed. Detect the program and record it in parsed output. Save sequence and domain tables together with the main output.

For models containing GA values, `--cut_ga` uses distinct sequence and domain score thresholds for reporting and inclusion. Check both levels. In default/other threshold modes, reported rows can fail inclusion. `domtblout` has no literal `!` inclusion column: use documented thresholds or cross-check the main report's inclusion markers, and reconcile `tblout` domain counts. Preserve sequence-only hits for review rather than calling them domain-confirmed.

Do not impose the BLAST E-value cutoff on HMM hits. A full-library rescan may identify competing architectures, but rescanning the same Pfam model is not an independent validation. Archive the actual profile version and threshold values; database releases and models can change.

Official specification: [HMMER source manual, hmmsearch](https://raw.githubusercontent.com/EddyRivasLab/hmmer/master/documentation/man/hmmsearch.man.in), consulted 2026-09-25. Check the installed version's help for executable options.

## Coordinates and coverage

Label the numerator, denominator, and aggregation method for every coverage field.

- BLAST best-HSP query coverage and subject coverage differ from coverage computed by the union of non-overlapping HSP intervals. Do not sum overlapping HSP lengths. Check order/orientation where aggregate coverage is interpreted as a continuous homologous region.
- HMM span coverage, `(hmm_to - hmm_from + 1) / model_length`, is not non-gap residue coverage. Multiple fragments can span a large portion of the model while missing internal core positions.
- Keep protein and model coordinates together. Several records can be split matches of one domain; repeated domains require evidence for distinct units. For actual non-gap coverage, retain alignments or a justified derived alignment, not just endpoint tables.
- A short model covering 100% of its own positions says little about the completeness of a long protein. Assess full architecture and appropriate same-model references.

## CDD, Pfam, and competing annotations

Record the source model as well as the search service. CDD imports Pfam and other collections and contains NCBI-curated models. Distinguish `cd*` model hierarchies, imported `pfam*` records, and broad `cl*` superfamilies. Specific/non-specific are database hit categories, not direct tests of a particular substrate. Full output may contain evidence omitted by concise output.

A broad superfamily match does not prove a narrower family. A relevant non-specific match is not automatically meaningless; assess its family scope, competing models, and other evidence. A lower E-value from an overlapping broad model does not justify relabeling a protein's substrate. Do not tally correlated models as votes or assign function by majority count.

Source: [NCBI CDD Help](https://www.ncbi.nlm.nih.gov/Structure/cdd/cdd_help.shtml), consulted 2026-09-25.

## KO and integrated adjudication

Record each KO's source, database release, query set, score, applicable threshold, E-value, and assignment status. A provider GFF annotation and a new KofamScan assignment are distinct observations. Use the latter run's formal pass indicator and actual method settings; a small E-value alone does not substitute for the KO-specific threshold. Preserve multiple formal assignments and explain their interpretation alongside CDD, Pfam, phylogeny, and integrity evidence.

Before using a missing KO as evidence, confirm that the protein and relevant model were actually tested. Candidate subsets, model subsets, and output/reporting limits constrain the meaning of an absent row. For example, the saved Coffee Kofam run covers 92 MDR proteins, including only 12 of the 17 CAD shortlist proteins. Its other five shortlist proteins are outside that run, not threshold failures.

Formalize integrated reasoning with separate required gates, support, competing interpretations, and review triggers, each with a rule ID and family-specific rationale. A candidate can pass broad family/project gates while its narrow functional assignment is unresolved or model integrity is uncertain. Do not retroactively impose KO or a pure reference clade as mandatory if the project's documented selection retained justified exceptions. See [the CAD example](cad-integrated-decision-example.md).

## References, phylogeny, and function

Adjudicate the tested sequence's experimental evidence, not just its name or “reviewed” database status. Negative results apply to the tested substrates and assay conditions. Weak activity and alternative major substrates matter when selecting comparators.

A family tree may not resolve substrate specificity when functions are distributed across several branches. Adding a verified reference can change the nearest-reference label. Keep the reference set versioned and use local alignment inspection, known sites, domain architecture, and experimental context together. A protein absent from a full-length main tree remains in the family table if otherwise supported.

Keep support measures and tree procedures faithful to the executed analysis. A failed ModelFinder attempt does not mean model selection finished. A fixed seed records an important reproducibility parameter; exact byte-identical trees are not guaranteed across software versions, platforms, and threading settings. [IQ-TREE command reference](https://iqtree.github.io/doc/Command-Reference), consulted 2026-09-25.

## Gene models and missing members

Reconstruct CDS from the correct genome and annotation strand/phase, then compare with the supplied CDS and protein using documented translation settings. Report unresolved phase, internal stop, or translation discrepancies. Even exact agreement does not prove that an annotation captures the biological transcript.

Use targeted genome searches, protein-to-genome alignment, or transcript evidence when a missing expected member or suspicious model affects the conclusion. Cluster genomic HSPs by plausible local locus and orientation; chromosome-wide query coverage cannot describe a single rescued gene. Keep candidate fragments and proposed reconstructions separate. If genome rescue was not performed, describe the result as an inventory within the searched annotation/proteome and state that annotation omissions remain possible.

## Minimal command patterns

Adapt to the installed tools and new run directory; these are patterns, not prevalidated commands for an unknown dataset. Set meaningful paths and family-specific parameters first. Do not rerun into historical outputs.

```bash
makeblastdb -in "$target_faa" -dbtype prot -parse_seqids -out "$blast_db"
blastp -query "$reference_faa" -db "$blast_db" \
  -evalue "$blast_evalue" -max_target_seqs "$target_cap" \
  -num_threads "$threads" -seg "$seg_mode" \
  -outfmt '6 qseqid sseqid pident length qstart qend sstart send evalue bitscore qlen slen' \
  -out "$blast_output"
hmmsearch --cpu "$threads" --cut_ga --noali \
  --tblout "$seq_hits" --domtblout "$domain_hits" \
  -o "$hmm_report" "$profile_hmm" "$target_faa"
hmmscan --cpu "$threads" --cut_ga --noali \
  --tblout "$scan_seq_hits" --domtblout "$scan_domain_hits" \
  -o "$scan_report" "$pressed_pfam_db" "$candidate_faa"
```

Capture stderr, exit status, installed versions, defaults that affect results, and threshold metadata. `hmmscan` requires a compatible pressed profile database; creating indexes is a write and must respect shared database permissions. Mixed model collections without GA cannot use `--cut_ga` indiscriminately. Select CDD commands from the installed tool/version and a versioned database, retaining the post-processing settings. Do not convert historical Coffee numerical filters into hidden defaults.
