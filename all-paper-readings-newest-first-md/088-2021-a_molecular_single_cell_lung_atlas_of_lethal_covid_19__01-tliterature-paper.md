# 035. A molecular single-cell lung atlas of lethal COVID-19

## 基本信息
- 年份：2021
- 期刊：Nature
- DOI：https://doi.org/10.1038/s41586-021-03569-1
- PMID：33915568
- 主题：lethal COVID-19；lung tissue atlas；snRNA-seq；immune-epithelial-stromal circuits

## 为什么重要
这篇把 severe infection 的测量位置从 blood 拉到 diseased lung tissue。对 T-cell 和人群免疫力的 method report，它最重要的价值不是提供 TCR，而是说明 circulating immune state 与 tissue pathology 不是同一个观测问题：真正的 severe outcome 需要在 organ damage context 中建模 immune cells、epithelium、fibroblasts 与 intercellular circuits。

## 数据与研究设计
- 物种/器官：`Homo sapiens`；lung
- 核心模态：single-nucleus RNA-seq of lung tissue；伴随组织/成像验证与外部 imaging data context
- processed atlas：Broad Single Cell Portal 记录 `116,313` total cells；研究 summary 描述 `>116,000` nuclei
- 样本规模：`19` COVID-19 autopsy lungs / decedents 与 `7` pre-pandemic controls；portal summary 另说明 early release 包含 `20` frozen COVID lungs from 19 decedents
- 技术背景：short post-mortem interval lung autopsy samples；10x nuclei profiling
- 主要比较：COVID lethal lung vs controls，涉及 immune fraction shifts、impaired epithelial regeneration、pathological fibroblasts and ligand-receptor/protein-activity circuits

## 文章中的算法/分析流程
### 1. Lung atlas construction
- 数据对齐到 joint human and SARS-CoV-2 reference。
- portal summary 明确写出 CellBender 去除 technical artifacts/ambient RNA，再用 Seurat 做 QC 与 integration。
- 细胞类型识别采用 cluster markers、published signatures 与 manual curation 三路结合，而非完全 supervised transfer。

### 2. Tissue-state interpretation
- 论文不只比较 immune abundance，也用 damaged epithelial states、fibroblast expansion 与 inflammatory interaction context解释 lethal pathology。
- 对 T-cell 算法而言，这提示“state”应受 tissue context conditioning，不能把 lung immune cells 当 blood PBMC 的批次。

### 3. Network/circuit layer
- 文中把 protein activity inference 与 ligand-receptor interaction analysis用于推断 pathological circuits 和 candidate targets。
- 这不是新的 graph neural model，而是 atlas 后处理 network interpretation；后续算法可以把这些 circuits 变成 explicit tissue-cell graph learning task。

## 与 T 细胞—人群免疫力的关系
- 直接 TCR 层缺失；T cells 只是 lung immune landscape 的一部分。
- 但这篇对“免疫力不能只看 PBMC”非常关键：severe COVID 的 organ-level failure 涉及免疫细胞与肺实质细胞的协同失调。
- 若我们的文章要讨论 tissue-aware immune-state modeling，这篇应放在 blood atlas 之后作为 organ pathology counterpoint。

## 数据可用性
- Processed GEO：`GSE171524`
- Processed portal：Broad Single Cell Portal `SCP1219`
- Raw controlled data：Broad DUOS study `DUOS-000130`
- 作者代码仓库：<https://github.com/IzarLab/CUIMC-NYP_COVID_autopsy_lung>
- 代码输入：
  - repo 主要围绕 processed lung atlas objects、metadata、gene signatures 和 figure-specific R/Python analysis artifacts
  - upstream paper pipeline还依赖 Cell Ranger/CellBender/Seurat 生成 expression matrices and integration objects
- 代码输出：
  - figure-level atlas analyses、cell-fraction/state comparisons、signature/network interpretation artifacts
  - repo 根目录有 `code_overview.csv` 连接代码与 figures/tables
- 模型结构与意义：`snRNA preprocessing + artifact removal + atlas integration + tissue-state/network interpretation`；它为 organ-context immune modeling 提供公开 atlas 与 reproducibility scaffolding

## 算法贡献和不足
- 直接算法创新：**中低**。主要依赖现有 preprocessing、integration 与 network interpretation。
- 数据资源价值：**高**，因为公开 tissue atlas、processed portal、GEO 与代码较完整。
- 新算法空间：
  1. tissue-aware immune-state embeddings
  2. organ damage graph models linking immune, epithelial and stromal states
  3. blood-to-tissue transfer with uncertainty for severe infection

## 可放入 method report 的表述
Melms et al. 提供 severe infection 的 lung tissue benchmark：现有 atlas workflow 已能经 CellBender/Seurat 与 circuit inference 解析 lethal COVID-19 的 immune-stromal-epithelial pathology，但还缺将 tissue context、donor outcome 与 immune-cell states联合建模的统一算法。

## 一句话结论
这篇不是 TCR 方法论文，却是 tissue-aware population immunity 章节的重要资源文献，因为它把严重感染的算法问题从 PBMC state shift推进到 organ pathology circuits。
