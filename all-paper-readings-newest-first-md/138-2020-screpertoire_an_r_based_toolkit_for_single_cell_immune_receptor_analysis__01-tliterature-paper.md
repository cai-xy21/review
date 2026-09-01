# 080. scRepertoire: an R-based toolkit for single-cell immune receptor analysis

## 基本信息
- 年份：2020
- 期刊：F1000Research
- DOI v1：https://doi.org/10.12688/f1000research.22139.1
- 常用 v2 DOI：https://doi.org/10.12688/f1000research.22139.2
- PMID/PMCID：PMID 32789006；PMCID PMC7400693
- 主题：single-cell TCR/BCR analysis；clonotype metadata integration；Seurat/SCE workflow

## 为什么重要
scRepertoire 是单细胞免疫受体分析的基础工具层。它不预测 antigen specificity，也不是深度学习模型；它的价值在于把 10x Cell Ranger V(D)J 或 AIRR-like contig annotations 转为可与 Seurat、SingleCellExperiment、monocle3 等表达对象连接的 clonotype metadata。对 T-cell population immunity 算法来说，它代表了 `receptor calls -> clonotype table -> expression object metadata -> repertoire statistics` 的标准工程接口。

## 数据与研究设计
- 文章类型：工具/软件论文
- 适用数据：single-cell TCR/BCR V(D)J contigs + paired scRNA-seq expression object
- 示例数据：12,911 tumor-infiltrating and peripheral-blood T cells from 3 renal clear cell carcinoma patients
- 物种/器官：示例为 human renal clear cell carcinoma tumor-infiltrating/peripheral blood T cells
- 平台：主要面向 10x Genomics Chromium Immune Profiling / Cell Ranger V(D)J outputs；新版支持更多格式

## 核心亮点
1. **receptor metadata integration**：将 per-cell V(D)J contigs 与 scRNA cell barcodes 合并。
2. **flexible clonotype definition**：可按 V/J gene usage、CDR3 nucleotide、CDR3 amino acid 或组合定义 clonotype。
3. **repertoire summary functions**：clone size、homeostasis、proportion、diversity、overlap、V/J usage 等。
4. **expression-linked visualization**：将 clone 信息投影到 UMAP/tSNE/cluster metadata 上。

## 核心贡献
- 提供 R workflow 将 immune receptor data 与 Seurat/SCE/monocle3 object 连接起来。
- 将 bulk repertoire 常用的 diversity/overlap/homeostasis 统计迁移到 single-cell barcode 层。
- 形成 TCR/BCR 与 transcriptome 联合可视化和分组统计的常用基础流程。
- 为后续 clone-aware trajectory、sequence representation 和 specificity-aware modeling 提供输入层。

## 与 T 细胞-人群免疫力的关系
TCR/BCR clonotype 记录了适应性免疫暴露和克隆扩增历史。scRepertoire 使研究者可以在 T-cell state map 上叠加 clone size、共享性和多样性，初步连接 T-cell phenotype 与 population-level antigen experience。

## 工具中的算法/分析流程
### 1. Import and combine receptor contigs
`combineTCR()` 和 `combineBCR()` 读取 Cell Ranger filtered contig annotations，并按 sample 合并 receptor calls。

### 2. Clonotype definition
用户可按 gene segments、CDR3 nucleotide、CDR3 amino acid 或 paired-chain combinations 定义 clonotype。定义粒度会直接影响 clone expansion 和 overlap 结论。

### 3. Merge with expression object
`combineExpression()` 将 clonotype、clone size、sample/group 等 metadata 合并到 Seurat/SCE/monocle3 对象，支持在 expression embedding 上查看 clonal expansion。

### 4. Repertoire statistics
`clonalHomeostasis()`、`clonalProportion()`、`clonalDiversity()`、`clonalOverlap()` 等函数输出 clone abundance/diversity/overlap summaries。

## 对算法工作的启发
1. **Clone-aware trajectory inference**：把 clonotype hyperedges 作为 state transition constraint。
2. **TCR/BCR sequence representation learning**：超越 exact CDR3 matching，用 sequence embedding/graph 建模相似性。
3. **Donor-aware repertoire statistics**：用 mixed model/Bayesian hierarchy 控制 donor/sample/cell-count bias。
4. **Specificity-supervised model**：将 tetramer、epitope database、functional assay 与 transcriptome state 联合。

## 数据与代码可用性
- GitHub 原入口：<https://github.com/ncborcherding/scRepertoire>
- 当前常用仓库：<https://github.com/BorchLab/scRepertoire>
- Zenodo archive/source data：<https://doi.org/10.5281/zenodo.3856827>
- 示例数据：GitHub/Zenodo Data folder；renal clear cell carcinoma TIL/PBMC example
- 输入：
  - Cell Ranger V(D)J `filtered_contig_annotations.csv`
  - AIRR、BD、MiXCR、TRUST4、WAT3R 等 receptor call formats（新版支持）
  - 可选 Seurat/SingleCellExperiment/monocle3 expression object
- 输出：
  - per-cell clonotype table
  - expression object metadata with clonotype/clone-size fields
  - diversity/overlap/homeostasis/VJ usage summaries
  - UMAP/tSNE and group-level repertoire plots
- 模型结构与意义：rule-based data integration + descriptive repertoire statistics toolkit；无参数化学习模型

## 可信度评估
- 期刊层面：F1000Research；工具型论文，开源代码是主要可信度来源
- 可复现性：高，package、vignettes、example data 和 Zenodo archive 明确
- 局限：不预测 antigen specificity；不做 clone-aware trajectory；不建模 donor hierarchy；多链和 low-quality contigs 仍需用户判断
- 综合判断：**工具层算法贡献中等，工程复用价值高，新模型空间很大**

## 一句话结论
scRepertoire 是单细胞 TCR/BCR 分析的基础入口：后续新算法不应重复导入和可视化，而应在它输出的 clonotype metadata 基础上发展 clone-aware、donor-aware 和 specificity-aware joint modeling。
