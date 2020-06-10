### Sample_type   

在cfRNA测序中，该chimeric RNA在哪种癌症中被发现



###  OncoKB-Gene

在OncoKB中提示有药物靶标的基因



### OncoKB-Level

FDA-approved: FDA批准的药物靶标

详细可查看[OncoKB](https://www.oncokb.org/actionableGenes#levels=1)



### OncoKB-Alteration

药物针对哪种类型的变异



### OncoKB-Tumor_Type

药物针对的癌症类型



### OncoKB-Drugs

靶标基因对应的药物名称



### 从JunctionReadCount到最后一列annots

每一列的解释可以参考如下：

__JunctionReads__: 跨越融合位点(junction site)的reads数量   

__SpanningFragCount__: 对于某一成对的reads（mate pair），如果其中1条read完整地比对在融合位点上游的融合基因内，另一条read完整地比对在融合位点下游的融合基因内，则该成对的reads记为spanning fragments。SpanningFragCount表示spanning fragments数量。   
根据融合断点的位置和reads的长度，有时可能会出现SpanningFragCount 等于0，所有反映融合事件的reads都以JunctionReads形式出现的情况。   

__SpliceType__: 断点是否出现在参考转录本结构注释文件提供的外显子连接点(exon junction site)。 

__FFPM__: 支持Chimeric RNA的reads数目取决于融合转录的表达量以及测序的reads数量。随着测序总体数据量的升高，测序导致的重复的Chimeric RNA的reads数量也会升高。因此，我们需要对支持融合事件的reads数量进行标准化，即 FFPM (fusion fragments per million total reads)。STAR-Fusion中默认的FFPM阈值为0.1，意味着在总reads数为10 Million前提下，至少需要有1条read支持该Chimeric RNA。该默认参数可以有效过滤假阳性结果。如果需要更改FFPM过滤标准，可以在STAR-Fusion `--min_FFPM`参数中进行设置。

__LargeAnchorSupport__: 如果融合位点两侧25bp被junction reads覆盖，则记为"YES_LDAS"(LDAS = long double anchor support)。如果融合事件缺少LargeAnchorSupport以及spanning fragment支持，那么该Chimeric RNA结果很可能是假阳性。     

__LeftBreakEntropy, RightBreakEntropy__: 融合位点两侧15bp的外显子碱基的香农熵([Shannon entropy](http://bearcave.com/misl/misl_tech/wavelets/compression/shannon.html))。最大熵为2，表示复杂度最高。最低为0(表示15个碱基为单一核苷酸)。低熵位点通常应被视为不太可靠的融合位点。   

__annot__: 最后一列"annots"利用 FusionAnnotator(与STAR-Fusion绑定的程序)为Chimeric RNA提供了一个简化的注释。对于人类基因组，fusion注释信息基于[CTAT_HumanFusionLib](https://github.com/FusionAnnotator/CTAT_HumanFusionLib/wiki)数据库，其中包括许多已知与癌症相关的融合基因的信息资源，以及一些在正常组织中也频繁出现的、与癌症无关的融合基因（这些注释信息有助于我们过滤掉假阳性的融合基因）。





