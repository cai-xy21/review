# 081. Scirpy: a Scanpy extension for analyzing single-cell T-cell receptor-sequencing data

## 基本信息
- 年份：2020
- 期刊：Bioinformatics
- DOI：https://doi.org/10.1093/bioinformatics/btaa611
- PMID：32761198
- 代码：<https://github.com/scverse/scirpy>
- 文档：<https://scirpy.scverse.org>
- 主题：单细胞 TCR/BCR repertoire 分析；AnnData/Scanpy 生态；clonotype network；receptor-transcriptome 联合注释

## 为什么重要
Scirpy 是 Python/scverse 生态里最核心的单细胞免疫受体分析工具之一。它本身不是一个新的湿实验数据集，而是把 10x V(D)J、TraCeR、AIRR Rearrangement 等受体输出接入 AnnData，并提供 clonotype 定义、克隆扩增、V/J 使用、CDR3 序列距离、network visualization 与转录组 metadata 联合分析。对“T 细胞-人群免疫力”方向，它对应的是最底层的 receptor tooling layer：如果要把 TCR/BCR 作为 population immune phenotype 的一部分，Scirpy 类工具决定了数据结构和默认统计范式。

## 数据与研究设计
- 数据性质：方法/软件论文；主要贡献是工具和数据结构，不产生新的大型人群数据。
- 适用数据：人/鼠或其他物种的 single-cell V(D)J contig annotations，常见来源包括 10x Genomics Cell Ranger V(D)J、TraCeR、AIRR 格式；可与 scRNA-seq AnnData 对象通过 cell barcode 对齐。
- 物种/器官：工具不限定物种或组织；与本文综述最相关的是 human PBMC、肿瘤浸润 T cells、感染/疫苗队列中的 TCR/BCR 数据。
- 公开数据：论文自身没有 accession 级别的新实验数据；复现实例依赖公开示例数据和 scirpy/scverse 文档中的 example datasets。

## 核心亮点
1. 把 immune receptor metadata 标准化放入 AnnData，降低 Python 单细胞分析中 TCR/BCR 与 RNA 分析割裂的问题。
2. 支持 AIRR-compliant 数据结构，便于跨工具复用。
3. 提供 clonotype network 与 sequence-distance based receptor grouping，使克隆分析不仅停留在 exact clonotype counts。
4. 与 Scanpy/AnnData 可视化、聚类、差异分析自然衔接，适合做 receptor-state coupling。

## 文章中的算法/分析流程
### 1. 数据读入与结构化
- 输入通常是 V(D)J contig table：cell barcode、chain、V/D/J/C gene、CDR3 nucleotide/amino-acid sequence、productive flag、UMI/read count。
- Scirpy 将每个细胞的 receptor chains 写入 AnnData 中的 immune receptor 字段，并保留与 RNA 表达矩阵共享的 observation index。
- 支持过滤 non-productive chains、处理多链细胞、定义 receptor arm 与 locus。

### 2. clonotype 与 clonal expansion
- 可按 exact CDR3 nucleotide/amino-acid、V/J gene、paired alpha/beta 或 heavy/light chain 组合定义 clonotype。
- 输出 clonotype id、clone size、clone expansion category，并可投影到 UMAP、cell type、sample、disease group 等 metadata。
- 算法意义在于把 receptor sequence identity 转换为可用于单细胞统计建模的 cell-level covariate。

### 3. receptor similarity network
- Scirpy 支持基于 CDR3 序列距离构建 receptor similarity graph，并进行 clonotype cluster/network visualization。
- 这一步比 exact clonotype 更适合研究潜在 shared specificity，但仍不是抗原特异性的监督模型；它只能给出 sequence-similarity hypothesis。

### 4. repertoire summary statistics
- 常用输出包括 clonal expansion、alpha/beta chain pairing、V/J gene usage、spectratype、clone overlap、repertoire diversity 等。
- 这些指标可按 donor、组织、疾病严重度、疫苗时间点或细胞状态分组比较。

## 代码输入、输出、模型结构和意义
- 代码输入：Cell Ranger V(D)J `filtered_contig_annotations.csv`、AIRR rearrangement table、TraCeR output、AnnData/Scanpy expression object。
- 代码输出：带 receptor annotations 的 AnnData；clonotype labels；clonal expansion tables；receptor similarity graph；V/J usage/diversity plots；UMAP 上的 clone/state overlay。
- 模型结构：不是深度模型，而是数据结构标准化 + sequence identity/distance + graph/network summaries + AnnData 集成 API。
- 意义：为后续 clone-aware multimodal model 提供统一输入层；很多更复杂模型可直接读取 Scirpy 生成的 clonotype/chain metadata。

## 对新算法贡献程度
- 直接算法创新：中。核心不是新概率模型，而是 receptor analysis 的 Python 标准化实现。
- 数据资源价值：低到中。论文不产生新 cohort，但工具显著提升公开 TCR/BCR 数据复用。
- 对新算法启发：高。它暴露了现有流程的边界：receptor 多作为 metadata 后处理，尚未充分进入统一表示学习。

## 新算法开发空间
- 将 Scirpy 的 clonotype graph 与 scRNA/ADT/ATAC graph 联合建模，而不是只做 overlay。
- 对 donor-level receptor diversity、clonal sharing 与 immune fitness 建立层级模型。
- 发展 antigen-aware 或 specificity-supervised extension，把 CDR3 similarity、HLA、pMHC binding 和 transcriptional state 合并。
- 对多链、多克隆、低质量 V(D)J calls 给出 uncertainty-aware clonotype assignment。

## 数据可用性
- 新实验数据：无明确新 accession；软件论文主要开放代码。
- 代码仓库：<https://github.com/scverse/scirpy>
- 文档与教程：<https://scirpy.scverse.org>
- 软件输入/输出：见上文。
- 复现性评价：代码开放、文档成熟、生态稳定；但示例数据不等同于统一 benchmark，真实 cohort 分析仍取决于原始 V(D)J 数据质量。

## 一句话结论
Scirpy 是 Python 单细胞免疫受体分析的基础设施型论文；它的主要贡献是把 TCR/BCR 从零散表格变成可与 AnnData 状态空间联动的标准分析对象。
