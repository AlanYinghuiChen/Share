### 1.获取Gini index >= 0.1区域的序列

对于连续的转录本序列中Gini index >= 0.1的窗口，进行合并，并且获取其序列。  

（转录本上的gini index计算可以参考晓帆的脚本）  

文件路径：  

/BioII/lulab_b/chenyinghui/project/shapeMap/result_two/06.gini_transcript/Rfam/fasta  

脚本：  

/BioII/lulab_b/chenyinghui/project/shapeMap/result_two/06.gini_transcript/Rfam/fasta/read_transcript_gini.sh  

脚本解释：  

python  read_transcript_gini_4.py  [GTF file]  [输入：转录本Gini index结果文件目录]  [输出：gini index>=0.1窗口（合并后）所在区域类型统计表]  [输出：窗口（合并后）长度统计表]  [输出：窗口的fasta序列目录]   



### 2. 使用cmscan将窗口序列比对到Rfam

cmscan是[Infernal](https://github.com/EddyRivasLab/infernal)的子程序。

目录：  

/BioII/lulab_b/chenyinghui/project/shapeMap/result_two/06.gini_transcript/Rfam/cmscan

脚本：

a. 每个转录本生成一个cmscan脚本

/BioII/lulab_b/chenyinghui/project/shapeMap/result_two/06.gini_transcript/Rfam/cmscan/cmscan.sh

b.运行脚本，进行cmscan比对  

cd /BioII/lulab_b/chenyinghui/project/shapeMap/result_two/06.gini_transcript/Rfam/cmscan  

nohup col_UV+_NAI.cmscan.sh 1>col_UV+_NAI.cmscan.sh.o 2>col_UV+_NAI.cmscan.sh.e &  

nohup uvr_UV+_NAI.cmscan.sh 1>uvr_UV+_NAI.cmscan.sh.o 2>uvr_UV+_NAI.cmscan.sh.e & 



### 3.提取cmscan比对信息

目录：   

/BioII/lulab_b/chenyinghui/project/shapeMap/result_two/06.gini_transcript/Rfam/motif_alignment_info   

脚本：   

cmscan_result_summary.shcmscan_result_summary.sh    



### 4.绘制Gini index的Aggregated plot

目录：

/BioII/lulab_b/chenyinghui/project/shapeMap/result_two/06.gini_transcript/Rfam/aggregated_plot

脚本：

aggregating_plot_col_UV+_NAI.sh

aggregating_plot_uvr_UV+_NAI.sh

