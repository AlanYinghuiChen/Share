
The format of hg38_refGene.txt :

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
