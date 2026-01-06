# DMSP-database (Ratified sequences of enzymes involved in DMSP, DMS and MeSH metabolism)

## The roles and environmental importance of dimethylsulfoniopropionate (DMSP)
<img width="1026" height="792" alt="Screenshot 2025-09-12 at 12 19 17" src="https://github.com/user-attachments/assets/de6eb6ff-6ef5-492c-b893-1e01ddaf673a" />


## DMSP synthesis pathways
<img width="571" height="770" alt="image" src="https://github.com/user-attachments/assets/146025fa-a7e2-4429-b027-f2d51341f2c6" />


## DMSP degradation pathways
<img width="659" height="769" alt="image" src="https://github.com/user-attachments/assets/54aae1c5-48a6-4f56-a9cc-bbdbe7e78dd6" />


## Identification of 10 single copy genes for relative abundance normalization
Extracts the 40 single-copy universal marker genes (described in Ciccarelli *et al.*, *Science*, 2006 and Sorek *et al.*, *Science*, 2007) from metagenomes using [fetchMGs](https://github.com/motu-tool/FetchMGs).
```bash
fetchMGs extraction nonredundant_genes.faa gene single_copy_genes
```

Select a subset of 10 single-copy universal marker genes that suitable for metagenomes (described in Alessio *et al.*, *Nature Communications*, 2019):
```bash
cd single_copy_genes
seqkit seq -n *fetchMGs.faa | grep -E "COG0012|COG0016|COG0018|COG0172|COG0215|COG0495|COG0525|COG0533|COG0541|COG0552" > single_copy_genes.list
```

## Identification of DMSP/DMS/MeSH metabolism related genes
megL gene is identified by KEGG (K01761):
```bash
hmmsearch --noali -T 502.37 --tblout megL_hmm.out megL-K01761.hmm nonredundant_genes.faa
```
Other genes with two or more ratified sequences are identified by combining hmmsearch and blastp.
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
```
Other genes with only one ratified sequences are identified by direct blastp
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

