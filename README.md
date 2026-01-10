# DMSP-database (Ratified sequences of enzymes involved in DMSP, DMS and MeSH metabolism)

## The roles and environmental importance of DMSP
<img width="1026" height="792" alt="Screenshot 2025-09-12 at 12 19 17" src="https://github.com/user-attachments/assets/de6eb6ff-6ef5-492c-b893-1e01ddaf673a" />


## DMSP synthesis pathways
<img width="571" height="770" alt="image" src="https://github.com/user-attachments/assets/146025fa-a7e2-4429-b027-f2d51341f2c6" />


## DMSP degradation pathways
<img width="659" height="769" alt="image" src="https://github.com/user-attachments/assets/54aae1c5-48a6-4f56-a9cc-bbdbe7e78dd6" />

## DMS and MeSH metabolism
<img width="1021" height="278" alt="image" src="https://github.com/user-attachments/assets/d9018553-8fb5-4a87-bad8-0240471866cd" />


## Identification of DMSP/DMS/MeSH metabolism related genes
megL gene (K01761) is identified by the KEGG database.
```bash
    hmmsearch --noali -T 502.37 --tblout megL_hmm.tbl megL-K01761.hmm nonredundant_genes.faa
```
Genes with two or more ratified sequences are identified by combining hmmsearch and blastp.
Using dddP as an example:
```bash
#prefilter by hmm
hmmsearch --noali -E 30 --tblout dddP_hmm.out dddP.hmm nonredundant_genes.faa

#get hits from hmm results
grep -v "^#" dddP_hmm.out | awk '{print $1}' > dddP.prefilter.genelist
seqkit grep -f dddP.prefilter.genelist nonredundant_genes.faa > dddP.prefilter.faa

#further filter by blastp (identity >= 40%; coverage >= 70%)
diamond blastp \
  --db  dddP.dmnd\
  --query dddP.prefilter.faa \
  --out dddP.id40_cov70.out \
  --outfmt 6 \
  --max-target-seqs 1 \
  --id 40 \ 
  --subject-cover 70 \
  --query-cover 70

#Notably, dmdA requires additional filtering because of its high sequence similarity to non-dmdA genes within the same GcvT family. Therefore, a phylogenetic approach was applied to distinguish true dmdA from related homologs, and to remove sequences clustering with known non-dmdA genes.
#Outgroup (sarcosine oxidase) and known non-dmdA sequences used for tree construction are provided in Non-dmdA.faa.

```
Other genes with only one ratified sequence are identified by direct blastp.
Using SAR11_1336 as am example:
```bash
diamond blastp \
  --db  SAR_1336.dmnd \
  --query nonredundant_genes.faa \
  --out SAR11_1336.id40_cov70.out \
  --outfmt 6 \
  --max-target-seqs 1 \
  --id 40 \  #For TpMMT, higher identity cutoff (id=60) is used.
  --subject-cover 70 \
  --query-cover 70
```

