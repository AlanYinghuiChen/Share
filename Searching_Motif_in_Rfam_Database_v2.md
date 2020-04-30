### 1.获取Gini index >= 0.1区域的序列

对于连续的转录本序列中Gini index >= 0.1的窗口，进行合并，并且获取其序列。  

（转录本上的gini index计算可以参考晓帆的脚本）  

- 文件路径：  

/BioII/lulab_b/chenyinghui/project/shapeMap/result_ziyuan/04.fasta    

- 脚本：  
  
```
UV照射条件变化：
/BioII/lulab_b/chenyinghui/project/shapeMap/result_ziyuan/04.fasta/get_delta_gini_fasta_UV-cahnge.sh

uvr8敲除与野生型比较（光照条件不变）：   
/BioII/lulab_b/chenyinghui/project/shapeMap/result_ziyuan/04.fasta/get_delta_gini_fasta_uvr8-cahnge.sh
```

- 脚本解释：  

```
python  read_transcript_gini_4.py  [GTF file]  [输入：转录本Gini index结果文件目录]  [输出：gini index>=0.1窗口（合并后）所在区域类型统计表]  [输出：窗口（合并后）长度统计表]  [输出：窗口的fasta序列目录]   
```


### 2. 使用cmscan将窗口序列比对到Rfam

cmscan是[Infernal](https://github.com/EddyRivasLab/infernal)的子程序。

- 目录：  

/BioII/lulab_b/chenyinghui/project/shapeMap/result_ziyuan/05.cmscan

- 脚本：

  - a. 每个转录本生成一个cmscan脚本
 
```
UV照射条件变化： 
/BioII/lulab_b/chenyinghui/project/shapeMap/result_ziyuan/05.cmscan/cmscan.UV-change.sh

uvr8敲除与野生型比较（光照条件不变）：   
/BioII/lulab_b/chenyinghui/project/shapeMap/result_ziyuan/05.cmscan/cmscan_uvr8-change.sh
```
  - b.运行脚本，进行cmscan比对  
```
cd /BioII/lulab_b/chenyinghui/project/shapeMap/result_ziyuan/05.cmscan/

nohup sh col_UV+_vs_UV-_NAI.cmscan.sh 1>col_UV+_vs_UV-_NAI.cmscan.sh.o 2>col_UV+_vs_UV-_NAI.cmscan.sh.e &  
...
```


### 3.提取cmscan比对信息

- 目录：   
```
/BioII/lulab_b/chenyinghui/project/shapeMap/result_ziyuan/06.motif_alignment_info   
```

- 脚本：   
```
UV照射条件变化： 
/BioII/lulab_b/chenyinghui/project/shapeMap/result_ziyuan/06.motif_alignment_info/cmscan_result_summary.sig_UV-change.sh

uvr8敲除与野生型比较（光照条件不变）：   
/BioII/lulab_b/chenyinghui/project/shapeMap/result_ziyuan/06.motif_alignment_info/cmscan_result_summary.sig_uvr8-change.sh
```

