### The format of hg38_refGene.txt  

```
585	ENST00000619216.1	chr1	-	17368	17436	17368	17368	1	17368,	17436,	0	MIR6859-1	none none	-1,
```

  `bin` smallint(5) unsigned NOT NULL,  
  `name` varchar(255) NOT NULL,  
  `chrom` varchar(255) NOT NULL,  
  `strand` char(1) NOT NULL,  
  `txStart` int(10) unsigned NOT NULL,  
  `txEnd` int(10) unsigned NOT NULL,  
  `cdsStart` int(10) unsigned NOT NULL,  
  `cdsEnd` int(10) unsigned NOT NULL,  
  `exonCount` int(10) unsigned NOT NULL,  
  `exonStarts` longblob NOT NULL,  
  `exonEnds` longblob NOT NULL,  
  `score` int(11) default NULL,  
  `name2` varchar(255) NOT NULL,  
  `cdsStartStat` enum('none','unk','incmpl','cmpl') NOT NULL,  
  `cdsEndStat` enum('none','unk','incmpl','cmpl') NOT NULL,  
  `exonFrames` longblob NOT NULL,  
  
  Detail at [Biostar](https://www.biostars.org/p/18480/), [UCSC golden Path](http://hgdownload.cse.ucsc.edu/goldenPath/hg38/database/refGene.sql)

### Build a refGene database from your own GFF file
 Detail at [what-about-gff3-file-for-new-species](http://annovar.openbioinformatics.org/en/latest/user-guide/gene/#what-about-gff3-file-for-new-species)
 
 Occasionally, the user will sequence a new species yourself, so the genome build that is not available from UCSC or Ensembl or anywhere. Generally speaking, you can use one of the several gene prediction tools to compile gene structure from the contigs that you build, and this will usually be in GFF3 format. The GFF3 or GTF file downloaded from Ensembl or compiled by the user need to be converted to the GenePred format. The conversion can be performed by the gff3ToGenePred or gtfToGenePred tools, available at http://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/. In our experience, occasionally some GFF3 files from Ensembl cannot be converted correctly. Thus, for non-human species not available from the UCSC annotation database, we recommend using the GTF file to generate the gene definition file.  

We gave an example to annotate Arabidopsis in this paper, and the procedure is reproduced below:  

Please go to http://plants.ensembl.org/info/website/ftp/index.html to download the GTF file and the genome FASTA file for this plant into a folder called atdb.  
```
mkdir atdb  
cd atdb  
wget ftp://ftp.ensemblgenomes.org/pub/release-27/plants/fasta/arabidopsis_thaliana/dna/Arabidopsis_thaliana.TAIR10.27.dna.genome.fa.gz  
wget ftp://ftp.ensemblgenomes.org/pub/release-27/plants/gtf/arabidopsis_thaliana/Arabidopsis_thaliana.TAIR10.27.gtf.gz  
```
Please decompress both files:  
```
gunzip Arabidopsis_thaliana.TAIR10.27.dna.genome.fa.gz   
gunzip Arabidopsis_thaliana.TAIR10.27.gtf.gz  
```
Please use the gtfToGenePred tool to convert the GTF file to GenePred file:    
gtfToGenePred -genePredExt Arabidopsis_thaliana.TAIR10.27.gtf AT_refGene.txt  
Please generate a transcript FASTA file with our provided script:  
```
perl retrieve_seq_from_fasta.pl --format refGene --seqfile Arabidopsis_thaliana.TAIR10.27.dna.genome.fa AT_refGene.txt --out   AT_refGeneMrna.fa  
After this step, the annotation database files needed for gene-based annotation are ready. Now you can annotate a given VCF file. Please note that the --buildver argument should be set to AT.  
```
Recently, a user shared his experience to annotate barley genome. These procedures were reproduced below. Big thanks to Yong-Bi Fu at Plant Gene Resources of Canada, Agriculture and Agri-Food Canada / Government of Canada.   

Procedures to make a database for using ANNOVAR from sequence assembly and annotation published in ENSEMBL PLANTS, using barley as example below  

At the ANNOVAR directory, make a db directory by typing:  
mkdir barleydb  
go to barleydb and download two files (one assembled sequence and one annotation) from ensembl plants website  
cd barleydb  

```
wget ftp://ftp.ensemblgenomes.org/pub/plants/release-39/fasta/hordeum_vulgare/dna/Hordeum_vulgare.Hv_IBSC_PGSB_v2.dna.toplevel.fa.gz  

wget ftp://ftp.ensemblgenomes.org/pub/plants/release-39/gff3/hordeum_vulgare/Hordeum_vulgare.Hv_IBSC_PGSB_v2.39.gff3.gz
Note that you could download these two files by other means and put them in barleydb
```
3a. need to install gff3ToGenePred first, if you don't have it. Note it is a binary file.

Note that one way to do it as, assuming you installed anaconda in your server: conda install -c bioconda ucsc-gff3togenepred

Note that another way is to google for it, get it from the web, and put it in the ANNOVAR directory: hgdownload.cse.ucsc.edu/admin/exe/linux.x86_64/gff3ToGenePred

3b. unzip two downloaded barley H*.gz files and convert gff3 to GenePred format
```
gunzip Hordeum*.gz
./gff3ToGenePred Hordeum_vulgare.Hv_IBSC_PGSB_v2.39.gff3 HV39-refGene0.txt
add a random first field to HV39_refGene0.txt using nl (adding the line number to each line)
nl HV39_refGene0.txt >HV39_refGene.txt
return to the ANNOVAR directory and do below
cd ..
```
```
retrieve_seq_from_fasta.pl barleydb/HV39_refGene.txt -seqfile barleydb/Hordeum_vulgare.Hv_IBSC_PGSB_v2.dna.toplevel.fa -format ensGene -outfile barleydb/HV39_refGeneMrna.fa
```
If the ANNOVAR directory is not placed in the PATH of .bash_profile or .bashsrc, you must use perl in the front of the command:
```
perl retrieve_seq_from_fasta.pl barleydb/HV39_refGene.txt -seqfile barleydb/Hordeum_vulgare.Hv_IBSC_PGSB_v2.dna.toplevel.fa -format ensGene -outfile
```
