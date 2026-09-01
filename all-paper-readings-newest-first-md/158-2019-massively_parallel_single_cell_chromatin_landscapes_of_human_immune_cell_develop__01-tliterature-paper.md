# 092. Massively parallel single-cell chromatin landscapes of human immune cell development and intratumoral T cell exhaustion

## 基本信息
- 年份：2019
- 期刊：Nature Biotechnology
- DOI：https://doi.org/10.1038/s41587-019-0206-z
- PMID：31375813
- 主题：scATAC-seq；human hematopoiesis；intratumoral T cell exhaustion；cis-regulatory landscape

## 为什么重要
把 T cell exhaustion 从 RNA 状态推进到 chromatin regulatory state，是 clone/RNA/protein 算法之外的表观组学基准。 对“单细胞组学 × T 细胞 × 人群免疫力”方法报告而言，这篇文献主要用于回答两个问题：第一，现有技术或算法已经能把哪些免疫信息带入单细胞分析；第二，哪些层次仍停留在后处理统计，适合进一步发展新算法。

## 数据与研究设计
- 数据性质：Homo sapiens immune development and tumor-infiltrating T cell exhaustion；single-cell chromatin accessibility；GEO GSE129785。
- 物种/器官：以人类外周免疫细胞、PBMC、T cells/macrophages 或 immune-cell model systems 为主；少数方法论文含 mouse tissue/cell-line benchmark。
- 模态：根据论文不同覆盖 scRNA-seq、HTO/sample barcode、ADT/protein、TCR/BCR clonotype、CRISPR guide、scATAC-seq、bulk ATAC/RNA/H3K27ac 或 paired RNA+ATAC。
- 公开获取：见“数据可用性”小节，已尽量补到 accession 或官方入口级别。

## 核心亮点
1. 把实验设计或算法流程推进到更适合 cohort-scale immune profiling 的形态。
2. 为 T cell state、activation、clonotype、chromatin priming 或 disease genetics 提供可量化特征。
3. 可作为后续 donor-aware、clone-aware、multimodal 或 genotype-to-state model 的输入层或 benchmark。

## 文章中的算法/分析流程
### 1. 输入层
massively parallel scATAC-seq atlas + regulatory element/gene-score/motif analysis + exhausted T-cell chromatin interpretation。 输入通常是 cell barcode 对齐的 count/feature matrix，或 sorted-cell epigenomic tracks/peak matrices，再叠加 sample、condition、stimulation、cell type、donor 等 metadata。

### 2. 核心计算层
- 若是 multiplexing 方法，核心是 barcode count matrix 的归一化、阈值/混合模型式分型、singlet/doublet/negative 判定。
- 若是 multimodal 方法，核心是把 RNA、protein、ATAC、TCR 或 perturbation readout 通过同一 cell barcode 对齐，再做联合聚类、状态注释和模态间解释。
- 若是 regulatory genomics 方法，核心是 peak calling/quantification、differential accessibility/activity、motif/footprint、GWAS enrichment 或 chromatin-potential 推断。

### 3. 输出层
- cell/sample assignment 或 doublet calls。
- 多模态 single-cell object：RNA、ADT、ATAC、TCR、guide RNA 等矩阵和 metadata。
- regulatory state annotation：accessible peaks、gene activity、motif enrichment、GWAS-relevant cell states。
- 可进一步进入 donor-level immune phenotype、T cell activation/exhaustion state 或 disease-risk interpretation。

## 代码输入、输出、模型结构和意义
- 代码/工具：Analysis uses scATAC peak/cell matrix, clustering, gene activity, motif enrichment, GWAS overlap and Cicero-style cis-regulatory inference；no single standalone named package is the main contribution。
- 输入：raw/processed count matrices、barcode tables、peak matrices、fragment files、metadata；对于 CHEERS 等统计工具，还包括 GWAS summary/loci 和 chromatin state activity matrix。
- 输出：demultiplexed singlets/doublets、multimodal objects、state labels、regulatory programs、SNP/state enrichment results。
- 模型结构：多为 workflow/统计模型/图谱构建；其中 CHEERS 属于明确统计 enrichment 方法，SHARE-seq chromatin potential 属于跨模态动态推断，multiplexing 方法属于 barcode classification。

## 与 T 细胞—人群免疫力的关系
- 对 T cell cohort，multiplexing 方法降低样本混池成本并帮助控制 batch/doublet，是人群免疫研究的实验设计基础。
- RNA/protein/TCR/ATAC 联合观测让 T cell 状态不再只依赖转录组，可同时看到表面表型、克隆结构、染色质 priming 和 perturbation response。
- GWAS/regulatory genomics 论文把人群遗传差异连接到具体 T cell activation state 或 stimulation-responsive enhancer，是 population immunity 的关键桥梁。

## 对新算法贡献程度
- 直接算法贡献：中到高，取决于条目；092 的核心贡献可概括为 `massively parallel scATAC-seq atlas + regulatory element/gene-score/motif analysis + exhausted T-cell chromatin interpretation。`。
- 数据资源价值：中到高；多数条目开放 accession 或官方入口，适合作为 benchmark 或 reference prior。
- 对新算法启发：高，尤其是 donor-aware multimodal model、clone-state regulatory model、variant-to-cell-state model 和 missing-modality integration。

## 数据可用性
- DOI：https://doi.org/10.1038/s41587-019-0206-z
- 公开数据/入口：Homo sapiens immune development and tumor-infiltrating T cell exhaustion；single-cell chromatin accessibility；GEO GSE129785。
- 代码/工具：Analysis uses scATAC peak/cell matrix, clustering, gene activity, motif enrichment, GWAS overlap and Cicero-style cis-regulatory inference；no single standalone named package is the main contribution。
- 数据性质补充：物种、样本、器官和模态见上文；如果是方法论文，新数据规模通常不是唯一重点，算法复用依赖公开代码和处理流程。
- 复现性评价：可复现性整体较好，但 human raw data 可能因隐私进入 dbGaP/EGA；多模态实验的完全复现还依赖 wet-lab protocol 和 feature-barcode 处理细节。

## 新算法开发空间
- 建立 donor-aware 层级模型，把 sample multiplexing 后的 donor/sample 结构显式纳入统计推断。
- 将 TCR/BCR clonotype、RNA、protein、ATAC 和 stimulation metadata 合并到统一 latent model。
- 针对缺失模态和不完整公开数据，发展 robust multimodal imputation / mosaic integration。
- 将 GWAS、HLA、chromatin activity 与 T cell state 连接，构建 variant-to-immune-state prediction。

## 一句话结论
092 是 81-94 这一段方法文献中的关键节点：它把 `scATAC-seq；human hematopoiesis；intratumoral T cell exhaustion；cis-regulatory landscape` 转化为可计算、可复用或可作为 benchmark 的单细胞免疫分析问题。
