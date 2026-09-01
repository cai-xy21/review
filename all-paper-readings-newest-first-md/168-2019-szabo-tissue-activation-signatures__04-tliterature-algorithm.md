# Algorithm Report 061

## Paper
Single-cell transcriptomics of human T cells reveals tissue and activation signatures in health and disease

## 算法视角定位
这篇文章不是提出新通用算法的 computational method paper，而是一个非常重要的 **human T-cell state reference**。它把 T 细胞状态拆成三个同时存在的轴：`lineage/CD4-CD8`、`tissue context` 和 `activation response`。对我们写 T 细胞-人群免疫力方法学文章来说，它的价值在于定义了“健康组织 T 细胞参考空间如何用于疾病投影”的任务。

## 数据模态与样本设计
- 模态：10x Chromium 3' scRNA-seq；流式/蛋白 marker 验证；外部 tumor T-cell scRNA-seq 投影分析。
- 物种/器官：`Homo sapiens`；外周血、肺、肺引流淋巴结、骨髓。
- 样本：2 名健康器官供者提供 lung/lymph node/bone marrow，2 名健康成人提供 blood。
- 实验条件：CD3+ T cells，resting 与 anti-CD3/CD28 activated 两种状态。
- 规模：文章摘要和正文报告分析超过 50,000 个 resting/activated human T cells。

## 关键算法问题
1. 如何在健康参考中同时分离 tissue effect、activation effect 和 CD4/CD8 lineage effect。
2. 如何把 blood T cells 与 tissue T cells 放进同一低维空间，判断血液是否能代表组织免疫状态。
3. 如何把疾病 T-cell scRNA-seq 投影到健康 activation reference 上，解释 tumor T cells 的功能状态。
4. 如何把 T-cell state annotation 从 marker-based cluster 描述推进到 reference mapping。

## 论文中的算法贡献
### 1) Tissue-aware T-cell reference map
文章对每个 donor 合并 resting/activated tissue T cells，使用 highly variable genes、unsupervised community detection 和 UMAP 建立二维参考空间。主要变异轴不是单一 tissue 或单一 activation，而是：
- activation state
- CD4/CD8 lineage
- tissue-specific enrichment, especially lung TRM and LN Treg

**算法意义**：这提示 T-cell atlas 不能只做全局 cluster annotation。更合理的模型应显式加入 tissue covariate，否则会把 tissue residency、activation 和 lineage differentiation 混为一个“状态”。

### 2) Blood-to-tissue projection
文章把 resting/activated blood T cells 投影到 tissue T-cell UMAP reference，并用 scmap 做替代验证。这个分析不是新算法，但给出了一个重要的 reference mapping 任务：

`query blood T cells -> healthy tissue activation reference -> nearest tissue/activation/lineage state`

**意义**：为后续开发 tissue-aware transfer learning、uncertainty-aware label transfer、out-of-reference T-cell state detection 提供原型。

### 3) Activation-state decomposition
文章把 CD8+ T cells 的 activation response 解析为 cytotoxic module 与 cytokine/chemokine module，同时在 CD4+ T cells 中识别 IFN-response intermediate state。这里的贡献是把 classical activation marker 细化成单细胞状态空间中的多个响应端点。

**方法学价值**：
- activation 不是一个二元标签，而是 cell-state manifold。
- disease-associated T cells 可以被解释为健康 activation state 的偏移、放大或异常组合。

### 4) Disease T-cell projection
文章将 breast/lung/skin/colon tumor T-cell datasets 投影到健康 T-cell reference，发现 tumor T cells 以 activated CD8 effector、Treg、resting CD4 等状态为主。

**算法意义**：这是后续 atlas transfer 的早期使用场景。它支持把健康参考作为 disease interpretation coordinate system，但也暴露出局限：reference 不一定覆盖 exhaustion、tumor-specific antigen response、therapy-induced states。

## 不是它解决得很好的问题
1. 没有 TCR-seq，因此无法把 clone expansion 与 activation/tissue state 直接连接。
2. donor 数很小，个体层方差、年龄、性别、暴露史无法稳健建模。
3. projection 主要是几何邻近和 marker interpretation，不是 probabilistic reference mapping。
4. 没有显式处理 disease query 的 out-of-distribution state。
5. 多组织只覆盖少量解剖位点，不能当作 pan-tissue T-cell atlas。

## 数据可用性评估
- DOI：https://doi.org/10.1038/s41467-019-12464-3
- PubMed/PMC：PMID 31619651；PMCID PMC6797728
- 数据 accession：GEO `GSE126030`
- GEO 链接：<https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE126030>
- 数据性质：人类 CD3+ T cells；blood/lung/lymph node/bone marrow；resting/activated；>50,000 cells。
- 样本设计：组织样本来自 2 名 organ donors，外周血来自 2 名 healthy volunteers；包含 resting 与 anti-CD3/CD28 activated 条件。
- 模态：10x Genomics 3' scRNA-seq。
- 代码：
  - marker selection、clustering、differential expression：<https://github.com/simslab/cluster_diffex2018>
  - scHPF：<https://github.com/simslab/scHPF>
  - UMAP projection analysis：<https://github.com/simslab/umap_projection>
- 复用性：高于初稿判断。GEO accession 和关键分析代码入口明确，适合作为 tissue/activation-aware T-cell reference；完整复现实验仍依赖作者注释、组织来源 metadata、外部 disease projection datasets 和激活条件的一致整理。

## 代码/模型结构
- 输入：cell-gene count matrix；metadata 包含 donor、tissue、resting/activated、lineage/subset annotation；projection 分析还需要 reference embedding 与 query expression matrix。
- 核心流程：`QC -> normalization -> HVG selection -> clustering/community detection -> differential marker selection -> UMAP -> scHPF/factor or module interpretation -> blood/tumor query projection -> state interpretation`
- 输出：T-cell subset/state labels、activation/tissue signatures、cluster/differential-expression tables、scHPF factors、query-to-reference projection result。
- 模型意义：这是 reference-map workflow，不是 trainable foundation model；作者代码覆盖 clustering/DE、factorization 和 projection 组件，适合被升级为 probabilistic T-cell reference model。

## 对新算法贡献程度评估
- 定义任务价值：高
- 数据资源价值：中高
- 直接算法创新：低到中
- 对后续方法启发：高

综合评估：**高价值 T-cell reference paper；直接算法创新有限，但对 tissue-aware and activation-conditioned T-cell modeling 很关键。**

## 可发展的新算法空间
### A. Tissue-aware T-cell atlas transfer
建立 `cell state + tissue prior + activation prior` 的联合映射模型，输出 label、uncertainty 和 out-of-reference score。

### B. Activation-conditioned latent space
把 activation response 当作连续 perturbation axis 建模，而不是 resting/activated 二分类。

### C. Blood-to-tissue immune inference
利用 paired 或 multi-tissue reference，从 blood T-cell states 推断 tissue-resident immune baseline，但必须做 uncertainty calibration。

### D. Clone-aware extension
在类似 reference 中加入 TCR/BCR，使 `same clone across tissue/activation states` 成为可建模结构。

## 适合纳入 method report 的表述
这篇文章说明，T-cell state 的基础坐标不能只由 cell type 决定，还必须同时考虑组织环境和激活条件。现有分析已经能构建健康 T-cell reference 并用于疾病投影，但仍缺少 donor-aware、clone-aware 和 uncertainty-aware 的统一算法。
