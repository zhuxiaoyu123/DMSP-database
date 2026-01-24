# DMSP-database
## Enzymes involved in DMSP, DMS and MeSH metabolism
<img width="1019" height="692" alt="image" src="https://github.com/user-attachments/assets/1e52b3d2-75eb-4c9b-8480-c3a2baef9b9b" />
<img width="801" height="938" alt="image" src="https://github.com/user-attachments/assets/c1eaaa62-4f37-4579-9c93-bf3a09e06548" />


## Identification of DMSP/DMS/MeSH metabolism related genes
megL gene (K01761) is identified by [BlastKOALA](https://www.kegg.jp/blastkoala/).

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
  --id 40 \
  --subject-cover 70 \
  --query-cover 70
```


