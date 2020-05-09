
### 1）使用REDItools寻找REDIportal数据库中已知的RNA editing位点  

- 脚本
每种癌症类型各有一个生成脚本的总脚本  
```
/BioII/lulab_b/chenyinghui/project/exRNA/editing/CRC_RNA-edit.mRNA.sh
```

```
dataPath='/BioII/lulab_b/jinyunfan/projects/exSEEK/exSeek-dev/output/pico-siqi-filter/bam_sorted_by_coord'
outDir='/BioII/lulab_b/chenyinghui/project/exRNA/editing/NC'
sample_list='/BioII/lulab_b/chenyinghui/project/exRNA/editing/sampleList/NC.txt'
genome_fa="/BioII/lulab_b/jinyunfan/projects/exSEEK/exSeek-dev/genome/hg38/fasta/genome.fa"
GTF='/BioII/lulab_b/chenyinghui/database/Homo_sapiens/GRCh38/gencode_v27/gencode.gtf'
script="$outDir/script"
if [ ! -d $script ]
then
mkdir -p $script
fi

for i in `cat $sample_list`
do
echo -e "#!/bin/bash
#BSUB -J \"REDItools\"
#BSUB -o $i.stdout.log
#BSUB -e $i.stderr.log
#BSUB -n 2
#BSUB -q Z-LU

export PATH=/BioII/lulab_b/chenyinghui/software/conda2/bin:\$PATH
echo REDItools start \`date\`
mkdir $outDir/$i

/BioII/lulab_b/chenyinghui/software/conda2/bin/python2 /BioII/lulab_b/chenyinghui/software/REDItools/REDItools-1.2.1/reditools/REDItoolKnown.py \\
-t 8 \\
-e -d -u \\
-c 10 \\
-m 255 \\
-q 20 \\
-v 3 \\
-n 0.1 \\
-i $dataPath/$i/genome_rmdup.bam \\
-f $genome_fa \\
-l /BioII/lulab_b/chenyinghui/database/REDIportal/REDIportal.main_table1_full.reditools.hg38.simple.sort.txt.gz \\
-o $outDir/$i

echo REDItools end \`date\`

```

### 2）统计每个样本找到的RNA editing数量
- 脚本
```
/BioII/lulab_b/chenyinghui/project/exRNA/editing/2.RNA-edit.count.sh
```

```
rootDir='/BioII/lulab_b/chenyinghui/project/exRNA/editing/'
sampleType="CRC ESCA HCC LUAD NC STAD"
outDir="/BioII/lulab_b/chenyinghui/project/exRNA/editing/stat"
if [ -f $outDir/all.xls ]; then rm $outDir/all.xls; fi
if [ ! -d $outDir ]; then mkdir -p $outDir; fi

for type in $sampleType
do
	dataPath="/BioII/lulab_b/chenyinghui/project/exRNA/editing/filtered/$type"
	if [ -f $outDir/$type.xls ]; then rm $outDir/$type.xls; fi
	sample_list="/BioII/lulab_b/chenyinghui/project/exRNA/editing/sampleList/$type.txt"
	for i in `cat $sample_list`
	do
		num=`sed -n '2,$p' $dataPath/${i}.xls| wc -l`
		echo -e "$i\t$num" >> $outDir/$type.xls
		echo -e "$type\t$num" >> $outDir/all.xls
	done
done
```
### 3) 对REDItools的结果进一步过滤，对过滤后的RNA editing进行注释

- 过滤标准 (参考REDItools的默认参数)
  - 只保留A -> G、或T -> C的RNA-editing位点
  - 要求变异频率 (alt reads/ total reads) > 0.1
  - 要求total reads > 10
  - 要求 alt reads > 3
 
 - 脚本
 ```
 /BioII/lulab_b/chenyinghui/project/exRNA/editing/3.RNA-edit.filter.sh
 ```
 
 ```
 rootDir='/BioII/lulab_b/chenyinghui/project/exRNA/editing/'
sampleType="CRC ESCA HCC LUAD NC STAD"

for type in $sampleType
do
	dataPath="/BioII/lulab_b/chenyinghui/project/exRNA/editing/$type"
	outDir="/BioII/lulab_b/chenyinghui/project/exRNA/editing/filtered/$type"
	script="$outDir/script"
	if [ ! -d $script ]; then mkdir -p $script; fi
	sample_list="/BioII/lulab_b/chenyinghui/project/exRNA/editing/sampleList/$type.txt"
	for i in `cat $sample_list`
	do
		if [ `ls $dataPath/$i/known*/outTable_* |wc -l` == 1 ] 
		then 
			result="`ls $dataPath/$i/known*/outTable_*`"
		else
			echo -e "the result file number of $i not equal to 1! please check."
			continue
		fi
		echo -e "echo start \`date\`
python /BioII/lulab_b/chenyinghui/project/exRNA/editing/bin/edit_site.filter.py $result $i $outDir
echo end \`date\`
" > "$outDir/script/${i}.filter.sh"
		echo -e "#!/bin/bash
#BSUB -J 'Annotation'
#BSUB -o $i.Annotation.stdout.log
#BSUB -e $i.Annotation.stderr.log
#BSUB -R 'span[hosts=1]'
#BSUB -n 1
#BSUB -q Z-LU
echo 9.Annotation start \`date\`
source /BioII/lulab_b/containers/singularity/wrappers/bashrc
perl /BioII/lulab_b/chenyinghui/software/annovar/annovar/table_annovar.pl \\
$outDir/$i.xls \\
/BioII/lulab_b/chenyinghui/database/Homo_sapiens/annovar \\
--buildver hg38 \\
--outfile $outDir/$i.anno \\
--remove \\
--nastring . \\
--thread 1 \\
--protocol refGene \\
-operation g
echo 9.Annotation end \`date\`
" > "$outDir/script/${i}.anno.sh"
	done
done
 ```
 
 ### 4) 将同种癌症类型的样本信息合并成矩阵文件
 
 - 脚本
 ```
 /BioII/lulab_b/chenyinghui/project/exRNA/editing/4.feature.matrix.sh
 ```
 
 ```
rootDir='/BioII/lulab_b/chenyinghui/project/exRNA/editing'
sampleType="CRC ESCA HCC LUAD NC STAD"

for type in $sampleType
do
echo "python $rootDir/bin/feature.matrix.py $rootDir/sampleList/$type.txt $rootDir/filtered/$type $rootDir/matrix $type" > "$rootDir/matrix/script/$type.matrix.sh"
 ```
### 5) 将癌症与正常样本的矩阵合并

- 脚本

```
/BioII/lulab_b/chenyinghui/project/exRNA/editing/matrix/merge/merge.sh
```
