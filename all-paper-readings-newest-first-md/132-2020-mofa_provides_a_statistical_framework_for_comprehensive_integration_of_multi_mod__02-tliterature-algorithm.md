# Algorithm Report 022

## Paper
MOFA+: a statistical framework for comprehensive integration of multi-modal single-cell data

## 题录与资源
- DOI：https://doi.org/10.1186/s13059-020-02015-1
- 论文数据 accessions：GEO `GSE87038`、`GSE97179`、`GSE121708`
- 实现：<https://github.com/bioFAM/MOFA2>
- 文档：<https://biofam.github.io/MOFA2/>

## 算法定位
MOFA+ 是 unsupervised multi-view factor model。它的主要产物不是 cell type classifier，而是解释多模态、多 group 数据的 latent factors，以及这些 factors 在 views、groups、features 上的载荷和解释方差。

## 模型输入
- 多个 `samples/cells x features` matrices
- 每个矩阵属于一个 `view`，例如 RNA、ATAC、methylation、protein 或派生特征块
- 可再按 experimental groups / conditions 分组
- 允许 missing values，适合部分模态不齐的探索性整合

## 模型输出
- latent factor values
- per-view feature weights
- view/group-specific variance explained
- factor relevance、feature ranking 与低维 embedding
- 可供下游 cluster annotation、trajectory interpretation、covariate correlation 与 hypothesis generation 使用的 factor tables

## 核心模型结构
1. **Shared factors**：用低维 factors 表示主要 variation axes。
2. **View-specific weights**：每个 factor 在每个 modality 上有对应 feature loadings。
3. **Group-aware structure**：group 维度用于区分共享与 condition-specific variation。
4. **Sparsity**：限制 factors 只激活部分 views/features，提高解释性。
5. **Variational inference**：让原本面向较小 multi-omics 数据的因子模型更适合单细胞规模。

## 算法贡献拆解
- 把 `multi-modal` 与 `multi-group` 放进同一 factorization problem。
- 提供 variance decomposition，而不只提供融合后的 neighbor graph。
- 对不同测量层的 feature-level explanation 友好。
- 成为深度模型之外很重要的 interpretable baseline。

## 与 T 细胞算法的连接
- 可把 T-cell RNA、chromatin、protein、clone summary 或 cytokine signature 组织成 views。
- 可用 factors 对比 age strata、disease groups、tissue groups 或 stimulation groups。
- 但 receptor sequence 本身不是原生输入对象；若研究 TCR motif 或 clone transitions，需要先做特征化或另建模型。

## 对新算法贡献程度
- 直接算法创新：**高**
- 可解释性贡献：**很高**
- 面向 T-cell repertoire 的直接性：**中低**
- 面向人群 donor-level 统计的直接性：**中**

## 数据与复现
- 论文使用公开 GEO datasets 与 mouse developmental demonstration data
- accessions 已明确到 GEO 级
- `MOFA2` 软件与文档可直接复用
- 进入免疫 cohort 时需特别处理 donor independence、cell composition 与 sparse modality coverage

## 可继续开发的空间
1. donor-random-effect factor model
2. clone-conditioned factors for T-cell state programs
3. non-linear factor decoder while retaining feature interpretability
4. factor uncertainty 与 phenotype/outcome prediction 联合输出

## 最终判断
MOFA+ 是“解释多模态 variation”这一算法目标的核心引用。它不替代 TCR 特异性模型，也不替代 RNA-protein 专用 count model，但它是人群免疫 method report 中最适合说明可解释 multi-omics decomposition 的基线。
