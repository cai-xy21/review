# 032. A single-cell and single-nucleus RNA-Seq toolbox for fresh and frozen human tumors

## 基本信息
- 年份：2020
- 期刊：Nature Medicine
- DOI：https://doi.org/10.1038/s41591-020-0844-1
- 主题：human tumor profiling toolbox；fresh scRNA-seq；frozen snRNA-seq；protocol/QC comparison

## 为什么纳入
这篇和“T 细胞人群免疫力”不是生物学核心文献，而是临床 tissue single-cell study 的方法底座。对于肿瘤或炎症组织中的 T-cell state work，fresh dissociation 与 frozen nuclei 方案会改变 cell recovery、RNA content、cell-type composition 和 malignant/immune annotation 稳定性；如果不把这种 measurement layer 写清楚，后续算法会把 protocol bias 当成 biology。

## 数据与研究设计
- 物种/组织：`Homo sapiens`；human tumor specimens，跨八类 tumor/sample settings
- 数据规模：`216,490` cells and nuclei
- 样本规模：`40` samples，来自 `23` specimens
- 核心比较：fresh tumor scRNA-seq 与 frozen/hard-to-dissociate tumor snRNA-seq
- 实验任务：为不同 tumor type、fresh/frozen status、dissociation/nuclei isolation protocol 选择给出 decision support
- 数据开放：GEO `GSE140819`；HCA project `a62dae2e-cd69-4d5c-b5f8-4f7e8abdbafa`；tumor toolbox summary portal

## 文章中的算法/计算流程
### 1. 上游流水线
- 作者把 FASTQ 到 clustering 的初始分析放进 `Cumulus` workflow。
- 计算对象不是一个新 latent model，而是一套可追踪的 pipeline：alignment/counting、QC、dimension reduction、clustering 与后续 sample summaries。

### 2. 协议评价指标
- 论文用 reads/UMI downsampling、cell/nucleus quality、recovery rate 与 recovered composition 比较协议。
- 这一步的重要性在于“公平比较 measurement protocols”：测序深度不齐会直接扭曲 detection rate、cell recovery 和 apparent diversity。

### 3. 论文专用下游 QC/annotation
- 文中把 Cumulus 产生的 AnnData 转成 Seurat object，再执行 empty-drop/doublet filtering、cell subset annotation、cell-type-specific QC 与 inferCNV/CNA evaluation。
- 这是一种 **protocol-aware QC stack**，对 tumor immune atlas 比单一 global QC threshold 更合理。

## 与 T 细胞—人群免疫力的关系
- 论文不是 T-cell biological atlas，也没有 scTCR；它解决的是 tissue profiling 的输入质量与协议偏差。
- 若我们的 method report 涉及肿瘤 T cells、组织 Treg 或 inflamed tissue T-cell states，这篇可放在“数据层偏差”章节，提醒 fresh/frozen、cell/nucleus 和 dissociation protocol 会改变 immune composition。
- 它可作为跨 protocol mapping、cell-versus-nucleus harmonization 与 missing-cell-type robustness 的 benchmark 来源。

## 算法贡献和不足
- 直接算法创新：**中低**。贡献在 toolbox、pipeline 与 evaluation framework，不是新的表示学习模型。
- 数据资源价值：**高**。公开 sample-level protocol comparison 对算法开发非常实用。
- 关键不足：
  - 比较目标主要是 assay/protocol evaluation，不是 donor-level immune outcome prediction。
  - nucleus 与 whole-cell 数据存在 feature-space and biology asymmetry；直接 batch correction 可能把真实 compartment differences 过校正。
  - 无受体组，不能直接回答 TCR-state coupling。

## 数据可用性
- 公开数据 accession：GEO `GSE140819`
- HCA Data Explorer project：`a62dae2e-cd69-4d5c-b5f8-4f7e8abdbafa`
- 公开 portal：tumor toolbox site，展示各样本 analysis summary
- 代码仓库：
  - `Cumulus` workflows：<https://github.com/klarman-cell-observatory/Cumulus>
  - 论文专用 analysis scripts：<https://github.com/klarman-cell-observatory/HTAPP-Pipelines>，`HTAPP_methods_toolbox/`
- 代码输入：
  - Cumulus：FASTQ/counting inputs 与 sample metadata
  - HTAPP downstream workflow：Cumulus AnnData/processed single-cell objects；用于 scRNA/snRNA protocol QC 与示例分析
- 代码输出：
  - count/clustering objects and summaries
  - QC-filtered Seurat/AnnData-derived analyses
  - subset annotations、QC reports、CNA/inferCNV-derived tumor characterization outputs
- 模型结构与意义：不是独立 ML model；是 `Cumulus upstream processing -> protocol-aware QC/annotation -> protocol comparison metrics`，把实验 protocol selection 变成可计算、可复核的 evaluation workflow

## 对新算法开发的启发
1. **Cell/nucleus domain adaptation**：保留真实 cell-state difference，同时校正 protocol-induced measurement shift。
2. **Protocol-aware uncertainty**：给 fresh/frozen、low-RNA immune cells、hard-to-dissociate tissue 的 annotation confidence。
3. **Composition bias audit**：把 immune subset recovery distortion 作为算法输出，而不是事后备注。

## 可放入 method report 的表述
Slyper et al. 提供了 human tumor tissue single-cell studies 的 measurement-layer toolbox：其算法价值不在新 embedding，而在 fresh scRNA 与 frozen snRNA 的 protocol-aware QC、composition evaluation 和公开 pipeline，为后续 tissue T-cell atlas integration 提供了偏差审计基线。

## 一句话结论
这是一篇数据采集和 QC 层非常值得写的 tissue profiling 方法论文；它告诉后续 T-cell 算法，跨协议整合前必须先处理 measurement bias。
