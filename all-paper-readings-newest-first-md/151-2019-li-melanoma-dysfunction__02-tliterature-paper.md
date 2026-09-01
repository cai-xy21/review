# 052. Dysfunctional CD8 T cells form a proliferative, dynamically regulated compartment within human melanoma

## 基本信息
- 年份：2019
- 期刊：Cell
- DOI：https://doi.org/10.1016/j.cell.2018.11.043
- PMID：30595452
- 主题：melanoma TIL；CD8 dysfunction dynamics；MetaCell；gene modules；TCR sharing

## 为什么重要
这篇文章把 human melanoma 中的 CD8 exhaustion/dysfunction 从“终末耗竭标签”推进为“动态、增殖、克隆相关的连续 compartment”。它不是单纯做细胞分群，而是用 MetaCell coarse-graining、gene module scoring 和 TCR sharing 共同说明：dysfunctional CD8 T cells 可能是肿瘤局部持续抗原刺激下形成的活跃分化过程。对我们写算法综述很有价值，因为它明确暴露了一个方法问题：T 细胞状态算法不能只输出离散 cluster 或单 marker score，必须同时处理连续程序、增殖、克隆结构和肿瘤反应性。

## 数据与研究设计
- 供者：25 名 melanoma patients，包含不同分期和治疗历史；另重点分析 9 个 treatment-naive stage III subcutaneous melanomas
- 样本：melanoma tumour lesions；另有若干 matched PBMC samples
- 物种/器官：Homo sapiens；melanoma tumour immune infiltrate 和外周血 PBMC
- 数据规模：47,772 single cells in projection；46,612 immune cells + 1,160 malignant cells；T/NK-focused analysis 包含 29,825 QC-positive T/NK cells
- 技术：MARS-seq single-cell transcriptomics；index sorting；改造的 MARS-seq/scTCR-seq 方案用于 T-cell clonotype analysis
- 目标：解析 CD8 transitional、cytotoxic、dysfunctional states 的连续关系，判断 dysfunctional compartment 是否增殖、克隆扩张并关联 tumour-reactivity

## 算法与生物学贡献
- 用 `MetaCell` 把单细胞表达数据 coarse-grain 成稳健 metacells，降低 dropout 和单细胞噪声对程序解释的影响。
- 在 CD8 metacells 上 de novo 提取 co-regulated gene modules：TCF7-like naive/memory、cytotoxic、dysfunction/checkpoint、proliferation 等。
- 通过 anchor-gene-correlated modules 计算 quantitative scores，例如以 `LAG3` 锚定 dysfunctional module、以 `FGFBP2` 锚定 cytotoxic module。
- 把 CD8 state 描述为 transitional-to-dysfunctional continuum，而不是互斥的离散状态。
- 用 TCR sharing 支持 transitional/dysfunctional compartments 与 complete cytotoxic cells 的关系边界。
- 强调 dysfunctional CD8 compartment 本身高度 proliferative，不宜简化为静态 terminal exhaustion。

## 文章中的算法/分析流程
### 1. MetaCell coarse-graining
- 输入：single-cell UMI expression matrix，过滤 mitochondrial genes、immunoglobulin genes、高丰度 lincRNA 和低可信转录本；过滤低 UMI 或高 mitochondrial fraction 的细胞。
- 特征选择：保留变异和表达量足够的基因用于 balanced similarity graph。
- 关键思想：把局部相似细胞聚成 metacells，再在 metacell 层估计表达富集、相似图和状态关系。
- 本文参数示例：T/NK 模型使用数百次 bootstrap，K-nearest graph 和 metacell splitting 以避免把异质细胞强行合并。
- 输出：metacell assignment、metacell similarity/projection、gene enrichment、module activity、cell-state annotation。

### 2. Gene module 与状态评分
- 作者不是只看 PDCD1/LAG3/HAVCR2 单 marker，而是在 metacell 层寻找与锚定基因相关的 gene sets。
- dysfunctional score 以 `LAG3` 模块为核心；cytotoxic score 以 `FGFBP2` 模块为核心；Treg/myeloid 等也使用类似 module scoring 思路。
- 这种做法对算法的启发是：T-cell state 更适合建模为多个连续 gene programs 的组合，而不是单一 exhaustion axis。

### 3. TCR sharing 与克隆动态
- TCR 序列从单细胞数据中解析后用于评估 clone size 和不同 T-cell groups 的 sharing propensity。
- 结果支持 dysfunctional cells 与 transitional cells 之间存在动态联系，而 complete cytotoxic cells 更像相对独立的 compartment。
- TCR 在本文中主要作为后验 lineage evidence，而不是直接进入 MetaCell 图构建；这正是后续 joint model 的空间。

### 4. 增殖、tumour-reactivity 与 patient context
- 文章显示早期 dysfunctional CD8 T cells 是高度 proliferative 的 immune compartment。
- dysfunctional signature intensity 与 tumour-reactivity 相关，提示 exhaustion-like phenotype 不应被简单视作无功能或终末失败。
- 不同患者中 dysfunctional compartment 的比例差异很大，说明算法必须处理 donor/tumour-level heterogeneity。

## 数据可用性
- 文章 DOI：https://doi.org/10.1016/j.cell.2018.11.043
- GEO processed data：`GSE123139`，链接 <https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE123139>
- raw EGA：`EGAS00001003363`
- 数据性质：25 名 melanoma patients；human melanoma tumour immune cells；另有若干 matched PBMC；MARS-seq/scRNA-seq + TCR extraction/index information
- GEO 摘要确认：processed scRNA-seq；raw sequences 通过 EGA controlled access
- MetaCell source：<https://bitbucket.org/tanaylab/metacell/src/default/>
- paper-specific scripts：原文 Data and Software Availability 指向 Tanay lab analysis page <http://compgenomics.weizmann.ac.il/tanay/?page_id=804>
- 论文网页：<https://www.cell.com/cell/fulltext/S0092-8674(18)31568-X>

## 代码可用性
- 代码/软件类型：主要使用 MetaCell R package；另有作者分析脚本页面。
- 代码输入：
  - filtered single-cell UMI matrix
  - cell-level metadata，如 patient、sample、tumour/PBMC、sorting index、TCR clonotype
  - marker gene sets 或从 metacell enrichment 中派生的 modules
- 代码输出：
  - metacell graph/projection
  - metacell-to-cell assignment
  - gene module scores and enrichment
  - T/NK/myeloid 等 immune state annotations
  - 后续可与 TCR clone size/sharing、proliferation fraction、patient composition 连接
- 模型结构与意义：
  - MetaCell 是 graph/coarse-graining framework，不是端到端监督模型。
  - 它通过构建相似图和稳健 cell aggregates 来提高 gene program 估计稳定性。
  - 在本文中，它的意义是把 melanoma TIL 的 CD8 dysfunction 表示为 continuous module manifold。

## 对新算法开发的启发
1. Module-based state modeling 比单 marker score 更适合 dysfunction continuum。
2. Cycling、tumour reactivity 与 exhaustion 需要联合而可分解的表示。
3. TCR clone sharing 可从后验验证进入模型训练。
4. 需要 donor-aware calibration，因为不同患者的 dysfunctional cell fraction 和 myeloid/T-cell context 差异很大。
5. 可以发展 `gene program + clonotype + antigen specificity` 的联合 latent model，避免把 TCR 只作为后验解释。

## 对新算法贡献程度
- 直接算法贡献：**中高**。本文依托 MetaCell，而非从零提出专用 TCR 模型；但它把 metacell/module 体系用于 T-cell dysfunction continuum，非常有方法代表性。
- 数据贡献：**高**。melanoma TIL、PBMC、TCR extraction、patient heterogeneity 组合非常适合做 T-cell state benchmark。
- 新算法空间：**很高**。特别适合发展 clone-aware dysfunction trajectory、proliferation-dysfunction disentanglement 和 tumour-reactivity prediction。

## 可作为我们 method 报告里的位置
建议放在“CD8 exhaustion/dysfunction state modeling”与“TCR-state coupling 的局限”两节之间。它说明仅用 cluster label 或 exhaustion marker 不够，下一代算法应当显式建模连续 gene modules、clone sharing、proliferation 和 patient context。

## 一句话结论
`052` 把 human melanoma CD8 dysfunction 写成动态、增殖和 clone-linked 的 compartment，是 exhaustion algorithms 的重要基准。
