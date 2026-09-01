# 022. MOFA+: a statistical framework for comprehensive integration of multi-modal single-cell data

## 基本信息
- 年份：2020
- 期刊：Genome Biology
- DOI：https://doi.org/10.1186/s13059-020-02015-1
- PMID/PMCID：32393329 / PMC7212577
- 主题：multi-view factor analysis；multi-group integration；interpretable latent factors；single-cell multi-omics

## 为什么重要
MOFA+ 代表单细胞多模态算法里的可解释统计路线。它不以某一种 assay 为中心，也不要求把所有问题交给端到端深度网络；它把不同 views、不同 sample groups 的变异拆成 latent factors 与 feature weights，回答“哪些变化是共享的，哪些是模态特异或 group-specific 的”。这对人群免疫尤其关键，因为 donor、组织、疾病、刺激和技术平台同时变化时，单纯的混合 embedding 很难解释。

## 数据与研究设计
- 论文分析的是 benchmark / demonstration datasets，而不是一个新的 T-cell population cohort。
- 论文 Data availability 给出 GEO accessions：
  - `GSE87038`
  - `GSE97179`
  - `GSE121708`
- 文章还用 multi-group mouse gastrulation scRNA-seq 展示 group structure 建模；其中 time-course 示例含 `16,152` 个来自 E6.5、E7.0、E7.25 mouse embryos 的细胞，阶段各有两个 biological replicates。
- 模态覆盖文章讨论的单细胞 multi-omics 场景，包括 expression、DNA methylation、chromatin accessibility 等 feature matrices；MOFA+ 的接口本身按 `views x groups` 组织。

## 核心贡献
1. **把 multi-modal 与 multi-group 一起建模**：不仅区分数据模态，还区分 condition/sample group。
2. **因子级可解释性**：每个 factor 有 sample scores、feature weights 与 variance explained。
3. **稀疏性与缺失支持**：多视图数据常有 missing entries 或不是每个 group 都强信号，factor model 适合做稀疏分解。
4. **可扩展推断**：MOFA+ 以 variational inference 改进原 MOFA 在大规模单细胞数据上的可用性。

## 与 T 细胞-人群免疫力的关系
- 论文数据不以 T 细胞为主，但方法框架直接适合 T-cell population questions：RNA、ATAC、protein、repertoire-derived features、clinical covariates 可以被组织成不同 views。
- MOFA+ 的 factor-level 解释适合回答“一个免疫状态轴由哪些基因/染色质 feature 支撑”“该轴在不同 donor group 是否共享”。
- 它更像 exploratory decomposition / hypothesis generator，而不是 TCR specificity predictor 或 supervised immune-fitness model。

## 文章中的算法贡献
### 1. Multi-view factor model
- 输入是多个 feature matrices；每个 view 对应一类 assay 或 feature block。
- MOFA+ 学习低维 latent factors 表示样本/细胞主要变异。
- 每个 view 有 feature weights，从而可识别 factor 在 RNA、ATAC、methylation 等层面的贡献。

### 2. Multi-group extension
- group-aware 结构允许同一 factor 在多个 groups 中共享，也允许某些 factor 在特定 group 中更有解释力。
- 对 cohort 研究，这比把 healthy、disease、stimulated、age strata 全堆成一个无标签矩阵更透明。

### 3. Sparsity 与 variance decomposition
- 稀疏约束帮助筛选 factor-active views/features。
- variance explained 输出是 MOFA+ 的关键读数：不仅知道细胞靠近谁，还知道 variation 被哪个 factor、哪个 view、哪个 group 吸收。

### 4. 推断与工程化
- MOFA+ 用 computationally efficient variational inference 支撑大规模单细胞数据。
- `MOFA2` 软件提供 R/Python 生态入口，适合作为探索性 multi-omics baseline。

## 相比已有方法的算法增量
- 相比单模态 PCA/ICA：同时建模多个 molecular layers。
- 相比只做 batch correction：MOFA+ 的目标是解释 structured variation，不是只把数据对齐。
- 相比黑箱 joint embedding：feature weights 与 variance decomposition 更利于 method report 中说明生物学驱动因素。
- 相比 totalVI：MOFA+ 更通用、更解释性强；totalVI 对 paired RNA-protein count likelihood 与 protein background 建模更专门。

## 局限与新算法空间
1. **factor model 仍偏线性**：复杂 receptor sequence grammar、非线性 activation manifold 与 niche interaction 不会自动被捕捉。
2. **cell/donor hierarchy 需额外设计**：直接把细胞当 rows 可能把 cell abundance 与 donor independence 混淆。
3. **TCR 表示缺乏天然接口**：repertoire sequence 需先转成 numeric features、motifs 或 graph summaries。
4. **新算法机会**：donor-aware factor model、clone-aware sparse factors、factor-to-outcome calibration、与 generative model latent 的解释性桥接。

## 数据可用性
- 数据 accession：GEO `GSE87038`、`GSE97179`、`GSE121708`
- 数据性质：benchmark / demonstration single-cell datasets；论文同时展示多模态与 multi-group mouse developmental data，不是专门免疫 cohort
- 文章代码与软件：
  - `MOFA2`：<https://github.com/bioFAM/MOFA2>
  - 官方文档：<https://biofam.github.io/MOFA2/>
  - 论文 release snapshot：Zenodo record `3735162`
- 代码输入：按 views/groups 组织的矩阵，可含 missing values；实践中可从 RNA、ATAC、methylation、protein 或派生 feature matrices 构建
- 代码输出：latent factors、factor values、feature weights、variance explained、factor-view/group relevance 与下游可视化
- 模型结构与意义：Bayesian group factor analysis + sparsity constraints + variational inference；它把多模态免疫数据的“共享/特异变异来源”变成可解释对象
- 复用判断：**高**。软件成熟、benchmark accession 明确；用于 T-cell population study 时要补 donor-aware aggregation/统计检验。

## 可放入 method report 的表述
MOFA+ 说明多模态整合不应只追求一个混合后的 embedding。对人群免疫研究，解释 variation 来源本身就是算法目标；但 factor decomposition 仍需与 clonotype、donor hierarchy 和非线性 immune trajectories 进一步结合。

## 一句话结论
MOFA+ 是可解释 multi-view/multi-group 单细胞建模的标准引用，适合与 totalVI 并列写成“统计因子模型路线”和“count generative model 路线”的对照。
