# 031. Single-cell transcriptomics of blood reveals a natural killer cell subset depletion in tuberculosis

## 基本信息
- 年份：2020
- 期刊：EBioMedicine
- DOI：https://doi.org/10.1016/j.ebiom.2020.102686
- PMID/PMCID：32114394 / PMC7047188
- 主题：tuberculosis blood immunity；PBMC scRNA-seq；cell-subset frequency shift；T-cell/NK-cell population context

## 为什么纳入
这篇不是 TCR 或 T-cell algorithm paper，主发现落在 TB 相关 NK-cell subset depletion；但它适合作为“感染场景下人群外周免疫单细胞 readout”的边界文献。它展示了疾病分层时最常见的一类分析链：先在 PBMC 单细胞图谱中发现状态/比例异常，再用大得多的 donor validation cohort 检验可测表型。

## 数据与研究设计
- 物种/器官：`Homo sapiens`；peripheral blood / PBMC
- discovery scRNA-seq：7 名个体，HC `n=2`、LTBI `n=2`、active TB `n=3`
- scRNA 细胞规模：10x 初始测到 `68,369` cells；去除约 `8.5%` 低质量/空滴/doublet 后分析 `62,628` cells
- validation cohort 1：flow cytometry，HC `n=81`、TB `n=50`
- validation cohort 2：flow cytometry，HC `n=39`、LTBI `n=27`、TB `n=37`
- 核心模态：10x scRNA-seq 与 flow cytometry；没有论文级 scTCR/scBCR、ADT 或 ATAC
- 文章在 PBMC 层先区分 T、B、myeloid 主群，再细化到 `29` 个 subsets；因此 T cells 是图谱主体之一，但核心 biomarker 结论并非 T-cell clonotype

## 文章中的算法/分析流程
### 1. Discovery atlas
- reads 经 Cell Ranger 对齐到 human reference 后进入 Seurat。
- 作者用无监督聚类、marker-based annotation 与 t-SNE 可视化形成 PBMC subset map。
- 这一层的算法贡献是应用型工作流：把 HC/LTBI/TB 三组细胞放在同一表达空间，比较 subtype markers 与 subset composition。

### 2. 疾病分层比较
- 论文主要比较各免疫 subset 在 HC、LTBI 与 active TB 中的比例差异，并以差异表达和 pathway interpretation 支撑 subset identity。
- 对人群免疫算法而言，这相当于 `cell annotation -> donor/group-aware abundance comparison -> candidate biomarker` 的经典路径。
- 局限也很明显：discovery donor 数只有 7，若直接按 cell 数做显著性，容易把 donor replication 与 cell replication 混在一起。

### 3. 验证层
- 关键 subset 通过 flow cytometry 在更大 cohort 复核。
- 该设计比只给 atlas 更有价值，因为它把“单细胞发现”转成了可在 donor cohort 中复测的免疫表型。

## 与 T 细胞和人群免疫力的关系
- PBMC 中大部分分析细胞来自 T-cell compartment，文中也给出 T-cell markers 与 T-cell subset frequency shifts。
- 对我们的方法综述，它更适合支撑“感染状态会重塑 PBMC 组成，少数 donor 的 cell-rich atlas 仍需要 donor-level validation”这一论点。
- 它不能支撑 TCR specificity、clone expansion 或 multimodal T-cell state learning，因为这些模态没有进入论文主数据。

## 算法贡献与局限
- 直接算法创新：**低**。主要使用 Cell Ranger、Seurat、差异表达/比例比较与 flow validation。
- 任务定义价值：**中**。TB/LTBI/HC 的分层适合做 disease-state composition benchmarking。
- 数据资源价值：**中低**。细胞数大，但 discovery donor 少，且本轮未在原文公开声明中定位到标准 GEO/SRA/GSA accession。
- 对新算法的启发：
  - 需要 donor-aware compositional inference，避免“62k cells 替代 7 donors”。
  - 可以把 discovery atlas 与 low-dimensional flow biomarker validation 设计成跨模态转译任务。
  - 感染分层下的 T/NK/myeloid abundance shift 可作为 immune-state score 的输入，而不是仅做单 subset ROC。

## 数据可用性
- 文章公开可核对的样本性质：人 PBMC；HC/LTBI/active TB 三组；scRNA discovery `7` donors、`62,628` analyzed cells；两组 flow validation 共覆盖 HC/TB/LTBI cohort
- DOI 已更正为链接形式：https://doi.org/10.1016/j.ebiom.2020.102686
- 文章正文/公开页面本轮未定位到 GEO、SRA、GSA-Human 或 ArrayExpress 原始测序 accession
- 文章没有提供作者专用代码仓库；分析方法按 Cell Ranger 与 Seurat 官方流程描述
- 2022 年存在 corrigendum；后续正式引用图表或 subset frequency 时应以更正版为准

## 可放入 method report 的表述
Cai et al. 是感染队列中“cell-rich discovery, donor-rich validation”的代表性 PBMC scRNA 研究：它主要依靠 Seurat 聚类与 subset-frequency comparison 发现 TB 相关免疫偏移，而不是提出新的 T-cell 算法，因此更适合作为 donor-aware composition modeling 的动机文献。

## 一句话结论
这篇文章的价值在于把 TB 人群外周免疫差异落到单细胞 subset 与可验证表型上；它的数据和分析链能暴露 donor-level 统计建模空间，但不是新算法或 TCR 资源型核心文献。
