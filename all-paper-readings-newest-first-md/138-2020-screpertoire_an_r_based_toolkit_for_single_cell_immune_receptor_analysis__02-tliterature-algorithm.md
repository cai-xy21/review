# Algorithm Report 080

## Paper
scRepertoire: an R-based toolkit for single-cell immune receptor analysis

## 算法视角定位
scRepertoire 是单细胞 TCR/BCR repertoire 分析的早期开源 R 工具。它不是深度模型，也不是 receptor specificity predictor；它的贡献是把 10x Cell Ranger V(D)J 输出整理成可与 Seurat/SingleCellExperiment/monocle3 expression object 连接的 repertoire metadata，使 clonotype、clonal diversity、overlap、homeostasis 和 expression embedding 可在同一工作流中分析。

## 数据模态与样本设计
- 模态：single-cell immune receptor profiling, TCR/BCR V(D)J contigs, paired scRNA-seq expression object。
- 示例数据：12,911 tumor-infiltrating and peripheral-blood T cells from 3 renal clear cell carcinoma patients。
- 物种/器官：human tumor-infiltrating and peripheral blood T cells in example dataset。
- 适用平台：主要面向 10x Genomics Chromium Immune Profiling / Cell Ranger V(D)J outputs。

## 关键算法问题
1. 如何把 per-cell V(D)J contig annotations 与 scRNA-seq cell barcodes 合并。
2. 如何定义 clonotype：gene segment, CDR3 nucleotide, CDR3 amino-acid 或 combinations。
3. 如何比较样本内/样本间 clone expansion、diversity、overlap 和 clonal homeostasis。
4. 如何把 clonotype metadata 投影到 UMAP/tSNE/cluster annotation 上。

## 工具中的算法贡献
### 1) V(D)J import and barcode-level merge
scRepertoire 读取 Cell Ranger annotated filtered contigs，把 TCR/Ig contigs 按 sample 合并，并与 Seurat 等 scRNA object 的 cell barcode 对齐。

**算法意义**：它解决的是工程和数据结构问题：让 receptor data 进入 single-cell expression workflow。

### 2) Flexible clonotype calling
工具允许按 receptor gene usage、CDR3 nucleotide、CDR3 amino acid 或组合方式定义 clonotype。

**方法学价值**：
- 不同定义对应不同 biological granularity。
- 对 T-cell state analysis，clonotype definition 会直接影响 expansion 和 sharing 结论。

### 3) Clonal quantification and diversity metrics
scRepertoire 提供 clone size、clonal proportion、clonal homeostasis、diversity、overlap 等函数，并支持按 sample、group、cluster 等 metadata 分层。

**算法意义**：它把 bulk repertoire 常用统计迁移到 single-cell setting，但没有建模 sequence similarity 或 antigen specificity。

### 4) Expression-linked repertoire visualization
工具可把 clonotype labels、clone size 和 diversity summaries 加到 Seurat/SingleCellExperiment/monocle3 对象中，支持在 UMAP/tSNE 上展示 clonal expansion。

**方法学价值**：这形成了经典 `state annotation -> clonotype overlay -> clone distribution interpretation` 管线。

## 不是它解决得很好的问题
1. 不预测 antigen specificity；与 GLIPH2/深度 TCR specificity models 是不同层级。
2. 不提供 clone-aware trajectory inference。
3. 不对 TCR sequence 进行 embedding/graph learning。
4. 多链、多样本、batch/sample confounding 需要用户自己判断。
5. 默认输出更偏 descriptive statistics，不是 donor-level inferential model。

## 数据可用性评估
- DOI v1：https://doi.org/10.12688/f1000research.22139.1
- 常用引用 DOI v2：https://doi.org/10.12688/f1000research.22139.2
- PubMed/PMC：PMID 32789006；PMCID PMC7400693
- GitHub：<https://github.com/ncborcherding/scRepertoire>
- Archived source/data Zenodo：<https://doi.org/10.5281/zenodo.3856827>
- 示例数据：GitHub/Zenodo Data folder；12,911 renal clear cell carcinoma tumor-infiltrating/peripheral-blood T cells。
- 复用性：高。R package、vignettes、example data、source archive 均可获取。

## 代码输入、输出、模型结构
- 输入：
  - Cell Ranger V(D)J `filtered_contig_annotations.csv` 或类似 contig annotation files。
  - 可选 Seurat/SingleCellExperiment/monocle3 expression object。
  - sample/group/cluster metadata。
- 核心函数层：
  - `combineTCR()` / `combineBCR()`：合并多样本 receptor contigs。
  - `combineExpression()`：把 clonotype metadata 加入 expression object。
  - `clonalHomeostasis()`, `clonalProportion()`, `clonalDiversity()`, `clonalOverlap()` 等：计算 repertoire summaries。
  - visualization functions：在 dimensional reduction 或 group summaries 上展示 clone information。
- 输出：
  - per-cell clonotype table
  - expression object metadata with clonotype/clone-size columns
  - diversity/overlap/homeostasis summaries
  - repertoire visualization plots
- 模型结构：无参数化学习模型；是 rule-based data integration + descriptive repertoire statistics toolkit。
- 意义：为 TCR/BCR 与 scRNA state 的耦合分析提供最常用的工程接口之一。

## 对新算法贡献程度评估
- 定义任务价值：高
- 数据资源价值：中
- 直接算法创新：中
- 对后续方法启发：高

综合评估：**重要工具型方法论文；其创新在 workflow/data-structure integration，不在统计或深度模型。**

## 可发展的新算法空间
### A. Clone-aware trajectory inference
在 scRepertoire 输出基础上，把 clonotype hyperedges 作为 trajectory constraints，估计 clone state transition。

### B. TCR/BCR sequence representation learning
将 CDR3 sequence, V/J gene usage 和 cell phenotype 共同嵌入，超越 exact clonotype matching。

### C. Donor-aware repertoire statistics
把 clonal diversity/overlap 的比较放入 mixed model 或 Bayesian hierarchy，控制 donor/sample/cell count 差异。

### D. Antigen-specificity-supervised extension
把 tetramer、functional assay、known epitope database 与 scRepertoire metadata 联合，训练 specificity-aware T-cell state model。

## 适合纳入 method report 的表述
scRepertoire 代表了单细胞 immune receptor analysis 的基础工具层：它让 TCR/BCR clonotypes 可以进入 Seurat 风格的单细胞表达分析流程。后续新算法的空间不在重复导入和可视化，而在 clone-aware, donor-aware, and specificity-aware joint modeling。
