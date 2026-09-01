# 《Targeting REGNASE-1 programs long-lived effector T cells for cancer therapy》精读

## 论文信息

- 作者：Jun Wei、Lingyun Long、Wenting Zheng 等
- 期刊：*Nature*
- 年份：2019；576: 471–476；在线发表于 2019 年 12 月 11 日
- DOI：10.1038/s41586-019-1821-z
- 原文：[Nature](https://www.nature.com/articles/s41586-019-1821-z)
- PubMed：[PMID 31827283](https://pubmed.ncbi.nlm.nih.gov/31827283/)
- 免费全文：[PMC6937596](https://pmc.ncbi.nlm.nih.gov/articles/PMC6937596/)
- 多组学总入口：[GEO GSE126072](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE126072)

## 一句话结论

体内 3,017-gene 代谢文库筛选将 REGNASE-1 识别为 CD8⁺ T 细胞抗肿瘤积累的强负调节器；其缺失通过 BATF 和线粒体程序使 TIL 同时具有广泛扩增、效应功能与长期持久性，并可与 PTPN2/SOCS1 删除产生进一步协同。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 主 metabolic library | 3,017 genes × 6 guides | 分成两个 sublibraries；各 3 guides/gene + 500 NTC |
| 主 screen mice | 60 recipients | 每个 sublibrary 的 3 个 pooled biological replicates；每 replicate 合并 20 只小鼠 |
| input/TIL coverage | 约 500× / 50× cells per sgRNA | 每个 screen sample 是 pooled tumours，不是单鼠独立重复 |
| secondary genome-wide screen | Brie library，4 guides/gene | 在 sgRegnase-1 背景寻找协同/依赖；2 pooled replicates，每组 10 mice |
| scRNA-seq | 4 GEO samples；目标 6,000 GEM/sample | control 与 sgRegnase-1 TIL；每样本由 6–8 个肿瘤合并，论文未给出一个明确统一的最终 QC cell 总数 |
| RNA-seq | GSE126071，18 samples | 包含 TIL/PLN、Regnase-1/control 等比较；具体组别看 GSM |
| ATAC-seq | GSE126070 8；GSE137014 14 | 早期/后补两套 ATAC 子系列，不能只下一个 |
| microarray | GSE137016，6 samples | sgRegnase-1 vs sgBatf/Regnase-1 等组合 |
| GEO SuperSeries | GSE126072，共 50 sample records | 5 个子系列；约 809.9 MB 聚合 TAR |

## 1. 研究要解决的问题

成熟效应 T 细胞通常杀伤强但持久性差，记忆样 T 细胞持久但即时效应有限。作者问：是否存在一个调控节点，能在肿瘤微环境中把二者组合成“long-lived effector”状态？

研究进一步寻找：

1. 影响 ACT 后肿瘤内积累的代谢相关基因；
2. REGNASE-1 缺失后的转录、染色质和代谢机制；
3. 哪个直接/关键下游因子解释该状态；
4. 是否还能通过第二次 screen 找到与 REGNASE-1 协同的工程组合。

## 2. 筛选与验证框架

### 2.1 metabolic in vivo screen

作者设计 3,017 个代谢酶、转运体和代谢相关转录调节基因，每基因 6 条 guide，拆成 AAAQ05/AAAR07 两个 sublibrary。Cas9-OT-I 细胞转导后进入 B16-OVA 肿瘤模型；比较 input 与肿瘤回收 TIL 的 guide enrichment。

每个 sublibrary 使用 60 只受体，随机分成 3 个 biological replicate，每个 replicate 合并 20 个肿瘤。这个设计提高回收覆盖度，但统计 n 是 3 个 pooled replicate，不是 60 个独立 screen replicate。

### 2.2 secondary Brie screen

在固定 `sgRegnase-1` 背景再导入 genome-wide Brie library，目的是发现：

- Regnase-1 缺失状态所依赖的因子，如 BATF；
- 可进一步增强的组合，如 PTPN2、SOCS1。

此处 input 约 10⁷ cells（约 120× coverage），每个 pooled TIL sample 平均回收约 3×10⁶ cells（约 40× coverage），2 个 pooled biological replicates、每个合并 10 个 recipients。

### 2.3 多组学验证

RNA-seq、ATAC-seq、scRNA-seq 和 microarray 共同比较 control、Regnase-1 KO、Batf/Regnase-1 double KO、Ptpn2/Regnase-1、Socs1/Regnase-1 等状态；功能层使用 B16-OVA、B16-F10、Ph⁺ B-ALL、OT-I、pmel-1 和 CAR-T 模型。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这项研究的“数据图谱”有两层：

1. **筛选层**：metabolic library 与 Brie secondary screen 的 guide counts/enrichment，主要在 Supplementary Tables；
2. **状态机制层**：GEO 中的 bulk RNA、bulk ATAC、scRNA、single-cell ATAC/后续 ATAC、microarray。

`GSE126072` 统管 5 个子系列，是下载组学的正确入口；但它不等于包含两次 screen 的全部 raw amplicon counts。

### 3.2 多大规模、覆盖哪些生物情境

| 子系列 | 样本数 | 数据与比较 |
|---|---:|---|
| [GSE126070](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE126070) | 8 | ATAC-seq；Regnase-1/control TIL 等 |
| [GSE126071](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE126071) | 18 | RNA-seq；TIL、外周/PLN、control/Regnase-1 等 |
| [GSE137014](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE137014) | 14 | ATAC-seq；包含组合扰动的 peak abundance |
| [GSE137015](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE137015) | 4 | scRNA-seq；control/sgRegnase-1 TIL，多 sample/replicate |
| [GSE137016](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE137016) | 6 | microarray；Regnase-1/Batf 组合等 |
| GSE126072 SuperSeries | 50 | 上述 5 组总和 |

scRNA 建库时每样本由 **6–8 只小鼠肿瘤合并**，目标生成 **6,000 GEM/sample**。论文没有在正文给出一个清晰的“最终总 QC cells”数字，因此报告时不应把 4×6,000=24,000 当作实际可分析细胞数。

### 3.3 公共数据包有什么

| 文件/资源 | 体积 | 内容与用途 |
|---|---:|---|
| `GSE126072_RAW.tar` | 809.9 MB | SuperSeries 聚合的 bigWig、CEL、TAR、TXT；可能与子系列下载重复 |
| `GSE126070_RAW.tar` | 715.8 MB | 8 个 ATAC 样本的 bigWig 聚合 |
| `GSE126071_RAW.tar` | 2.3 MB | 18 个 RNA-seq 样本的处理 TXT；raw reads 在 SRA |
| `GSE137014_JW_stATAC_peaks_abundance_matrix.txt.gz` | 17.2 MB | ATAC peak abundance matrix；快速比较组合扰动 |
| `GSE137015_RAW.tar` | 85.2 MB | 4 个 scRNA 样本的处理 TAR；raw reads 在 SRA |
| `GSE137016_RAW.tar` | 6.6 MB | 6 个 Clariom S CEL；处理表达也在 sample table |
| Supplementary Table 1 | 约 586.4 KB | metabolic library、guide 和主 screen 结果 |
| Supplementary Table 2 | 约 1.3 MB | secondary screen/相关分析 |
| Supplementary Table 3 | 约 4.8 MB | 转录/染色质等结果，需按 workbook guide 定位 sheet |

此外，论文引用 GSE54191、GSE76279、GSE84105 等公开感染/耗竭数据来生成 signature。这些属于外部比较，不计入 GSE126072 的 50 个新样本。

### 3.4 如何获取：按目的选择

#### 路线 A：快速复核状态机制

下载 RNA processed TXT、`GSE137014_JW_stATAC_peaks_abundance_matrix.txt.gz`、GSE137015 处理 TAR 和 GSE137016 CEL/series matrix。这样无需先下载全部 raw FASTQ。

#### 路线 B：完整重做组学

从每个子系列的 SRA Run Selector 分别获取 RNA、ATAC 和 scRNA runs。不要只从 SuperSeries 一次性导出后混跑；不同 assay 的建库、参考和分析软件完全不同。

#### 路线 C：重做 screen

从 Supplementary Tables 1–2 获取 guide library 和 normalized read counts。若想从 amplicon FASTQ 开始，需按 Methods/样本元数据追踪 screen sequencing runs；论文 Data availability 只明确将 microarray、RNA、ATAC、scRNA 置于 GEO，因此 screen raw reads 的公共性不如 processed tables 清晰。

#### 路线 D：避免重复下载

GSE126072 的 809.9 MB SuperSeries TAR 聚合了子系列文件。若已按子系列下载，先比对文件名和 MD5，不要再下载聚合 TAR。

### 3.5 下载后先做什么

1. 用 GSM title 建立 `genotype × tissue × assay × replicate` manifest；
2. scRNA 先核对最终 cell counts、样本 pooling 和 control/KO 标签；
3. RNA-seq 设计中区分 TIL 与 PLN/外周，避免把组织效应误当 genotype；
4. ATAC 两个子系列可能采用不同处理层，统一 peak set 前先检查 genome build；
5. screen 统计以 pooled replicate 为单位，不以单鼠数夸大 n；
6. double-KO 与 single-KO 分开计算 interaction，而非只比较 fold change 大小。

## 4. 主要发现

Regnase-1 是主 screen 最强富集基因之一。其缺失导致 TIL 大量积累并保留强效应功能，同时提高 TCF-1/naive-memory-associated gene programs 和线粒体 fitness。细胞在早期可扩增，后期增殖下降、凋亡减少，表现为“扩增后进入更稳定的持久效应状态”。

BATF 是关键下游：Regnase-1 缺失上调 BATF，Batf 共删除削弱其积累和线粒体表型。secondary screen 还找到 Ptpn2、Socs1，可在 Regnase-1 缺失基础上进一步改善 ACT。

## 5. 状态与分子 driver

REGNASE-1 是 RNase，调控 mRNA 稳定性；TCR 刺激可诱导其裂解。论文提出：

`tumour antigen → reduced Regnase-1 activity / Regnase-1 KO → BATF derepression → mitochondrial and effector programme → extensive expansion + survival → long-lived effector state`。

该状态打破“效应强必然持久差”的简单二分，但不是说所有 TCF-1⁺ 细胞都等同于经典 memory stem cell。其 phenotype 强烈依赖 TME 和抗原情境；在外周，Regnase-1 KO 可呈不同的激活/凋亡程序。

## 6. 推荐图版

- **Fig. 1**：3,017-gene in vivo metabolic screen 与 Regnase-1 命中。
- **Fig. 3**：RNA/scRNA、持久效应 phenotype；本综述最推荐。
- **Fig. 4**：BATF 与线粒体机制。
- **Fig. 5**：secondary Brie screen 与 PTPN2/SOCS1 组合。

如果只能选一张，选 Fig. 3；若强调组合导航，选 Fig. 5。

## 7. 创新价值

1. 以体内 ACT 为选择压力，而不是只筛体外增殖。
2. 明确寻找能同时支持 persistence 与 effector function 的状态。
3. 用 secondary genome-wide screen 解析单靶点下游依赖并寻找协同组合。
4. 将 RNA、ATAC、scRNA、代谢和多种肿瘤模型串成机制链。

## 8. 局限性

1. screen replicate 来自多鼠 pooled TIL，个体变异被平均，n 不能按鼠数计算。
2. OT-I/B16-OVA 是模型抗原体系。
3. REGNASE-1 调控广泛 mRNA，长期缺失可能有过度激活或组织毒性风险。
4. scRNA 目标 loading 不等于最终细胞规模，正文未给出明确总 QC count。
5. screen raw amplicon data 的公共获取不如处理表清晰。
6. double KO 协同仍需在人源临床相关模型系统评估。

## 9. 对本综述架构的作用

该文是“navigate T cell state”的代表性研究：通过体内筛选找到一个能够把细胞导向“long-lived effector”而非经典 memory/terminal effector 二选一的控制点。secondary screen 又说明状态导航可以是组合控制问题，而非单基因最优化。

数据仍是离线采样；要构建实时优化系统，需要能连续报告 BATF、线粒体 fitness、增殖与效应之间的动态平衡。

## 10. 可直接用于综述的观点

> 体内代谢 CRISPR screen 将 REGNASE-1 定位为抗肿瘤 CD8⁺ T 细胞的关键状态制动器；其缺失经 BATF 和线粒体重编程产生兼具广泛扩增、强效应和长期持久性的 TIL，而第二次 genome-wide screen 又揭示 PTPN2/SOCS1 等可组合的导航节点（Nature 2019, Wei）。

## 11. 避免误读

- 不要把 60 只受体写成 60 个独立 screen replicates；实际为 3 个 pooled replicates/sub-library。
- 不要把 6,000 GEM/sample 当最终 QC cell 数。
- 不要把 GSE126072 当作两次 screen raw data 的完整包。
- 不要把 TCF-1 上调直接等同于经典 memory stem state。
- 不要忽略 Regnase-1 phenotype 的 TME/抗原情境依赖。
