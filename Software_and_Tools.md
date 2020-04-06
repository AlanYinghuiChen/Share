### 2) DNA-seq  
#### (2.1) Mapping and QC  
- Remove adaptor: cutadapt, trimmomatic
- Mapping: Bowtie2, STAR  
- QC: fastqc  
#### (2.2) Mutation
- Mutation discovery: GATK, Varscan
- Mutation annotation: ANNOVAR
#### (2.3) Assembly
- denovo assembly software: Trinity
#### (2.4) CNV
- Whole Genome Seq: Control-FREEC  
- Whole exome Seq: CONTRA, ExomeCNV
#### (2.5) SV (structural variation)
- breakdancer

### 5) Epigenetic Data
- DNA Methylation 
  - Bi-sulfate data:
  - IP data:
#### DNAase-seq: 
- review : [Yongjing Liu, et al. Brief in Bioinformatics, 2019.](https://academic.oup.com/bib/article-abstract/20/5/1865/5053117?redirectedFrom=fulltext)
- peek calling:  [F-Seq](http://fureylab.web.unc.edu/software/fseq/)
- peek annotation: [ChIPpeakAnno](https://www.bioconductor.org/packages/release/bioc/html/ChIPpeakAnno.html)
- Motif analysis: MEME-ChIP
#### ATAC-seq:
 - pipeline recommended by [Harward informatics](https://github.com/harvardinformatics/ATAC-seq)
 - peek calling: MACS2
 - annotating peak: [ChIPseeker](https://bioconductor.org/packages/release/bioc/html/ChIPseeker.html)
 - Motif discovery: [HOMER](http://homer.ucsd.edu/homer/introduction/basics.html)
 
