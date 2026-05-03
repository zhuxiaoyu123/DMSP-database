# DMSP-database
Database of DMSP/DMS/MeSH metabolism-related genes

## Enzymes involved in DMSP, DMS and MeSH metabolism
<img width="1052" height="716" alt="image" src="https://github.com/user-attachments/assets/9a410f77-1a9f-4744-a422-91285e71595f" />

- In *Burkholderia thailandensis*, **BurB** is responsible for producing DMSP, which serves as a precursor for virulence-associated compounds.

<br>

<img width="1118" height="1334" alt="image" src="https://github.com/user-attachments/assets/0a8ea19b-b2a6-4e22-9189-5b06cdeb952d" />


- **AcuH** is also known as **AcuK**.

---


## Identification of DMSP/DMS/MeSH metabolism related genes
| Pathway | Key gene | Taxonomy | Annotation method |
| :--- | :--- | :--- | :--- |
| Met to MeSH | megL | prokaryotic | K01761 |
| Met to DMSP | burB | prokaryotic | custom database |
| Met to DMSP | dsyB | prokaryotic | K24666 |
| Met to DMSP | DSYB | eukaryotic | custom database |
| Met to DMSP | DSYE | eukaryotic | K28066 |
| Met to DMSP | dsyG | prokaryotic | custom database |
| Met to DMSP | dsyGD | prokaryotic | K28065 |
| Met to DMSP | mmtN | prokaryotic | K28064 |
| Met to DMSP | TpMMT | eukaryotic | custom database |
| Met to DMSP | MMT | eukaryotic (plant) | custom database |
| DMSP transporter | dmpX | prokaryotic | custom database |
| DMSP transporter | SAR11_1336 | prokaryotic | custom database |
| DMSP transporter | BCCT | prokaryotic | TCDB database |
| DMSP to DMS | Alma | eukaryotic | custom database |
| DMSP to DMS | dddD | prokaryotic | K28073 |
| DMSP to DMS | dddX | prokaryotic | K28072 |
| DMSP to DMS | dddL/Q | prokaryotic | K16953 |
| DMSP to DMS | dddP | prokaryotic and eukaryotic | K28067 |
| DMSP to DMS | dddW | prokaryotic and eukaryotic | K28068 |
| DMSP to DMS | dddY | prokaryotic | K28069 |
| DMSP to DMS | dddK | prokaryotic | custom database |
| DMSP to DMS | dddU | prokaryotic | custom database |
| Acrylate to Acryloyl-CoA | acuN | prokaryotic | K28070 |
| Acrylate to Acryloyl-CoA | prpE | prokaryotic | K01908 |
| Acryloyl-CoA to 3HP-CoA or MTA-CoA to MeSH | acuH | prokaryotic | K28071 |
| Acryloyl-CoA to Propionyl-CoA | acuI | prokaryotic | K19745 |
| 3HP-CoA to MalSA | dddA | prokaryotic | K28060 |
| 3HP-CoA to MalSA | dddB | prokaryotic | K28061 |
| MalSA to acetyl-CoA | dddC | prokaryotic | K00140 |
| DMSP to MMPA | dmdA | prokaryotic | K17486 |
| DMSP to MMPA | dmdA_like | prokaryotic | custom database |
| MMPA to MMPA-CoA | dmdB | prokaryotic | K20034 |
| MMPA-CoA to MTA-CoA | dmdC | prokaryotic | K20035 |
| MTA-CoA to MeSH | dmdD | prokaryotic | K20036 |
| DMSO to DMS | torZ/dorA | prokaryotic | K07812 |
| DMSO to DMS | dmsA | prokaryotic | K07306 |
| DMS to DMSO | ddhABC | prokaryotic | K16964 (ddhA) |
| DMS to DMSO | tmm | prokaryotic | K18277 |
| DMS to DMSO | dsoABCDEF | prokaryotic | custom database (dsoB) |
| DMS to MeSH | dmoA | prokaryotic | K16967 |
| MeSH to S | mtoX | prokaryotic | K17285 |
| MeSH to DMS | mddA | prokaryotic and eukaryotic | K21310 |
| MeSH to DMS | mddH | prokaryotic | custom database |
| MeSH to DMS | mddM1 | prokaryotic | custom database |
| MeSH to DMS | mddM2 | prokaryotic | custom database |
| DMS to Coenzyme-M | mtsAB | prokaryotic | K16954 (mtsA) |
| DMS to Coenzyme-M | mtsD | prokaryotic | custom database |
| DMS to Coenzyme-M | mtsF | prokaryotic | custom database |
| DMS to Coenzyme-M | mtsH | prokaryotic | custom database |

Genes with related KO categories are annotated by KEGG [BlastKOALA](https://www.kegg.jp/blastkoala/).<br>

> ⚠️ **Note:** Although KofamScan may be more convenient for local analyses, its stringent thresholds can lead to fragmented proteins being missed, which are common in metagenomic and metatranscriptomic datasets. Based on my testing, *BlastKOALA* does not appear to have this issue and is therefore recommended.<br>

BCCT family transporter genes are identified by [TCDB](https://www.tcdb.org/).<br>
The remaining genes are identified by blasting against custom databases built with ratified sequences.
Using dddK as an example:
```bash
#blastp (identity >= 40%; query coverage >= 70%; subject coverage >= 70%)
diamond blastp \
  --db  dddK.dmnd\
  --query dddK.prefilter.faa \
  --out dddK.id40_cov70.out \
  --outfmt 6 \
  --max-target-seqs 1 \
  --id 40 \ 
  --subject-cover 70 \
  --query-cover 70

For DmdA-like, DddK, MddH, and TpMMT, a set of previously identified non-homologous was included as negative reference sequences in the BLASTP search to help distinguish true hits from closely related non-target genes.
```
> ⚠️ **Note:** DmdA was first identified in Ruegeria pomeroyi (https://www.science.org/doi/10.1126/science.1130657) and is assigned to K17486. More recently, functional dmdA-like genes have been reported in SAR92 (https://journals.asm.org/doi/10.1128/mbio.01467-23), as well as in Vibrio and Psychrobacter (https://journals.asm.org/doi/10.1128/aem.01806-21). The SAR92 dmdA-like sequences cannot be annotated by BlastKOALA, whereas the dmdA-like genes from Vibrio and Psychrobacter are assigned to K00605 (gcvT / AMT, glycine cleavage system T protein, aminomethyltransferase). Additionally, between the canonical DmdA clade and these newly reported dmdA-like sequences, the phylogenetic tree contains several previously described non-dmdA genes. These together indicate these newly reported dmdA-like sequences are substantially diverged from the canonical Ruegeria DmdA. Therefore, individual BLASTP screening are applied for these dmdA-like genes. <br>
<img width="703" height="451" alt="image" src="https://github.com/user-attachments/assets/e8331b10-cd77-4df1-9d3a-00c48fb801a2" />






