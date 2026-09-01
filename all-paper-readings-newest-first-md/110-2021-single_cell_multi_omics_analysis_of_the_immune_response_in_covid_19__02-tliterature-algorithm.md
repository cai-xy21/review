# Algorithm Report 009

## Paper
Single-cell multi-omics analysis of the immune response in COVID-19

## 题录核验
- 期刊：Nature Medicine
- DOI：https://doi.org/10.1038/s41591-021-01329-2
- 注意：旧稿中写成 Nature Biotechnology `10.1038/s41587-021-00927-2`，并混入 plasma-proteomics 队列信息；本条已更正为 Stephenson et al. 2021 的 single-cell CITE-seq/VDJ COVID PBMC atlas。

## 算法视角定位
这是多模态感染队列分析的代表作。它的主要贡献不在基础算法创新，而在于展示了如何把多模态 readout、队列结构和临床严重度放入统一分析框架，使单细胞感染研究真正向 donor-level prediction 和 translational modeling 迈进。

## 核心算法贡献
- 应用型贡献大于方法型贡献。
- 通过多模态分析把细胞状态、蛋白/多组学信号与临床结局联系起来。
- 为 multimodal severity modeling、multi-omic fusion 和 donor-level prediction 提供真实 benchmark。
- 它证明了感染场景中多模态数据整合不是锦上添花，而是影响解释力的核心部分。
- 具体流程上，文章使用 Harmony 做跨样本 integration，并用 kBET 量化 integration 后 sample mixing 改善；同时用 CITE-seq protein、RNA 和 TCR/BCR repertoire 共同解释 severe disease 中 clonally expanded CD8 effector/effector-memory T cells、proliferating CD4/CD8 T cells、BCR sharing 和 plasmablast isotype shift。
- BCR convergence 分析将跨个体共享的 heavy/light chain V/J usage 与 CDR3 amino-acid similarity 编码为 adjacency matrix，是 receptor-to-donor network 建模的一个可复用模板。

## 关键方法学价值
- 为感染场景下的 multi-omic fusion 提供真实问题设定。
- 把 cell-level 表型与 donor-level 临床结局之间的桥接任务显式化。
- 为 recovery/exhaustion-like dynamics 的联合建模提供案例。
- 为处理模态不一致、模态缺失和队列异质性的算法需求提供背景。

## 相比既有工作的改进
它比单模态感染 atlas 更接近真实转化分析，说明单细胞免疫研究必须从 cell-level 描述走向 donor-level prediction。相比只用 RNA 的分析，它强调模态互补性对临床关联解释的增益。

## 适合抽象出的计算任务
- multimodal donor-level prediction
- severity-aware immune representation
- dynamic recovery / exhaustion modeling
- modality-robust immune state fusion
- multi-view clinical outcome prediction
- donor-level aggregation from cell-level embeddings

## 数据/代码可用性
- 数据：143 个 PBMC samples；1,141,860 个测序细胞，QC 后 781,123 个细胞；队列来自三个 UK 中心，覆盖 asymptomatic 到 critical COVID-19 及 healthy/non-COVID severe respiratory illness/IV-LPS 对照。模态含 transcriptome、188 surface proteins、BCR/TCR antigen receptor repertoire。
- 正式 accession：ArrayExpress `E-MTAB-10026`；HCA project `b963bd4b-4bc1-4404-8425-69d74bc636b8`；EGA `EGAS00001005465`。
- 代码：<https://github.com/scCOVID-19/COVIDPBMC>
- 代码输入/输出：输入 CITE-seq RNA/ADT matrices、VDJ/AIRR repertoire files、sample/donor/time/severity metadata；输出 integrated embeddings、multi-omic annotations、severity/time comparisons、B/T receptor analyses、myeloid/B/T/HSPC 子分析与论文复现图表。
- 复用性：高。
- 多模态资源价值：高。

## 对新算法开发的贡献程度
- 评级：**中等（场景与数据价值）/低（直接算法创新）**
- 原因：算法创新有限，但场景定义、多模态结构和数据价值都很高。

## 对我们方法论文的启发
- multimodal donor-level prediction
- severity-aware immune representation
- dynamic recovery / exhaustion modeling
- 缺失模态条件下的鲁棒表示学习
- 将细胞层异质性压缩成患者层风险表征的可解释聚合机制

## 方法局限对建模的提醒
- 多模态维度高但样本数有限，容易造成模型不稳定。
- 不同模态的噪声结构不同，简单拼接往往不够。
- 临床结局标签常受时间、治疗和基础状态共同影响。

## 总结
这篇论文的重要性在于为多模态单细胞感染研究提供了接近临床转化的真实建模场景。对方法研究来说，它是 multimodal infectious immunology 的关键 benchmark-style resource paper。
