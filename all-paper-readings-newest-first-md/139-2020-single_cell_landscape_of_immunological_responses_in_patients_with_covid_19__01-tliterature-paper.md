# 033. Single-cell landscape of immunological responses in patients with COVID-19

## 基本信息
- 年份：2020
- 期刊：Nature Immunology
- DOI：https://doi.org/10.1038/s41590-020-0762-x
- 主题：COVID-19 PBMC；adaptive immune landscape；scRNA-seq；TCR/BCR profiling

## 为什么重要
这篇早期 COVID PBMC work 在 T-cell 方向比许多纯 cytokine 或 bulk studies 更贴近我们的问题，因为它同时记录 expression landscape 与 adaptive receptor readouts。它不是新的 repertoire algorithm，但它展示了感染严重度语境下最常见的 `PBMC state map + TCR/BCR clone statistics` 分析范式。

## 数据与研究设计
- 物种/器官：`Homo sapiens`；peripheral blood / PBMC
- 研究对象：COVID-19 patients `n=13`，healthy controls `n=5`
- 核心模态：10x 5' scRNA-seq；TCR and BCR V(D)J profiling
- 公共数据：GSA-Human `HRA000150`
- 分析对象：
  - major PBMC compartments and disease-associated cell states
  - T-cell expansion and V(D)J usage
  - B-cell plasmablast/clone response and repertoire patterns

## 文章中的算法/分析流程
### 1. PBMC landscape
- 论文沿用 10x/Seurat 风格 preprocessing、cluster annotation 和差异表达解释。
- 这层解决“感染 cohort 中哪些 PBMC states 与 control 不同”，并为 adaptive receptor analysis 提供 cell-state metadata。

### 2. Repertoire statistics
- scTCR/scBCR 将 V(D)J calls 与 transcriptomic cell barcodes关联。
- 作者分析 expanded TCR/BCR clones、V(D)J gene usage 和 B-cell/T-cell subset context。
- 这是一类后验耦合：receptor calls 被映射回表达 atlas，再做 clone expansion and usage comparison；并非 sequence-aware representation learning。

### 3. Disease comparison
- 论文把感染状态下的 abundance、activation/exhaustion-like programs 与 adaptive receptor readout 并列解释。
- 对 method report 的意义是明确一个尚未被统一解决的问题：disease severity、donor、clone、cell state 通常分开统计，还没有在这篇工作中进入一个共同 generative/hierarchical model。

## 与 T 细胞—人群免疫力的关系
- 直接相关：有 T-cell transcriptomes 与 TCR expansion/readout。
- 它支持感染场景下 T-cell state 与 receptor repertoire 都会随 disease context 改变。
- 但 cohort 较小、早期 COVID sampling heterogeneity 较大，因此更适合作为 scenario/resource citation，不应当作跨人群 immune-fitness gold standard。

## 算法贡献与新空间
- 直接算法创新：**低到中**，因为主要依赖成熟 10x + Seurat + V(D)J descriptive analytics。
- 数据资源价值：**中高**，因为 adaptive receptor 与 expression 同 cell-level coupling 对后续算法很有用。
- 可做的新算法：
  1. severity-aware TCR-state model
  2. donor-clone-cell hierarchical repertoire statistics
  3. adaptive immune response score integrating transcriptome, clone expansion and receptor usage uncertainty

## 数据可用性
- Raw sequence accession：Genome Sequence Archive for Human `HRA000150`
- 作者代码：文章未提供公开仓库；code availability 声明说明 pipeline follows 10x Genomics and Seurat，custom scripts available on request
- 代码边界：
  - 输入：10x scRNA/V(D)J reads/counts and donor labels
  - 输出：PBMC expression clusters、TCR/BCR clone/usage summaries、source data tables
- 模型结构：无独立新模型；本质是 `scRNA annotation -> V(D)J barcode linkage -> clone/usage statistics in disease context`

## 可信度评估
- 期刊与问题重要性：高
- 模态贴合度：高于纯 scRNA COVID resource，因为有 TCR/BCR
- 局限：public code 缺失；human raw access may require GSA-Human process；cohort size small

## 可放入 method report 的表述
Wen et al. 是感染场景下 transcriptome-repertoire coupling 的早期 PBMC 例子：已有研究能把 TCR/BCR clone expansion 回填到 single-cell state map，但 donor-aware、severity-aware 和 receptor-sequence-aware 的统一模型仍未形成。

## 一句话结论
这篇文章适合放在 COVID adaptive single-cell immunity 章节，作为“已有 pipeline 能做 clone-state descriptive coupling，但仍缺统一算法”的代表。
