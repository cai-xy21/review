# 021. Joint probabilistic modeling of single-cell multi-omic data with totalVI

## 基本信息
- 年份：2021
- 期刊：Nature Methods
- DOI：https://doi.org/10.1038/s41592-020-01050-x
- PMID：33589839
- 主题：CITE-seq；RNA-protein joint model；deep generative model；probabilistic denoising

## 为什么重要
totalVI 是 `RNA + surface protein` 单细胞多模态算法的核心方法论文。CITE-seq 让转录本与抗体衍生标签可以在同一细胞测到，但“同测”不等于“同模”；RNA UMI、ADT count、蛋白背景、批次与抗体 panel 不完整性会共同扭曲 T 细胞状态空间。totalVI 的价值是把这些生物学与技术因素写进一个联合生成模型，而不是把两种矩阵简单拼接后再做聚类。

## 数据与任务设计
- 新数据：`SLN-all` mouse spleen / lymph node CITE-seq，论文说明抗体 panel 超过 100 个 surface proteins；GEO accession 为 `GSE150599`
- benchmark 数据：10x Genomics `PBMC5k`、`PBMC10k` 与 `MALT` CITE-seq
- 物种/器官：
  - 新数据为小鼠 spleen 与 lymph node immune cells
  - 公开 benchmark 含人 PBMC 与 MALT immune CITE-seq
- 任务边界：paired cells 同时有 RNA counts 与 protein counts；重点是联合表示、去噪、批次整合、差异分析与 association analysis
- 与本项目的关系：它不是专门的 TCR 或人群队列方法，但对依赖蛋白标志物区分 T 细胞 naive/memory/activation/exhaustion-like states 的研究是基础设施

## 核心贡献
1. **把 CITE-seq 写成联合概率模型**：共享 latent state 驱动 RNA 与 surface protein 观测。
2. **显式处理蛋白背景**：ADT 背景可来自 ambient antibodies 与 nonspecific binding，不能默认每个蛋白 count 都是 foreground。
3. **一套模型支撑多种下游任务**：同一后验表示可用于 latent embedding、normalization/denoising、differential testing 与 RNA-protein association。
4. **跨 batch 与不同抗体 panel 的路径**：模型允许 covariates，官方实现也讨论 missing proteins 与 panel mismatch。

## 与 T 细胞-人群免疫力的关系
- T 细胞状态经常由 RNA 与表面蛋白联合定义：例如仅凭 RNA 可能难以稳定区分技术 dropout、弱表达 marker 与真实 phenotype shift。
- 对 population immune studies，totalVI 提供了“技术噪声建模先于个体差异解释”的范式；否则 donor effect、batch effect 与 protein background 很容易混成假的人群差异。
- 局限也很明确：totalVI 的标准输入不含 TCR sequence、clonotype graph、组织层 cell-cell context 或 donor-level outcome，因此它是后续 T-cell population model 的上游表征模块，而不是完整答案。

## 文章中的算法贡献
### 1. 输入与共享 latent state
- 输入是按同一细胞 barcode 对齐的 RNA UMI count matrix `X` 与 protein UMI count matrix `Y`。
- 还可输入 covariate/design matrix `S`，官方文档以 day、donor、batch 等观测变量为例。
- 每个细胞有低维 latent variable `z_n`，作为 RNA 与 protein 的共享细胞状态。

### 2. RNA 生成支路
- RNA 支路沿用 count generative modeling 思路：用 latent state 与 covariates 产生基因相对丰度，再用 library size 与 gene-specific overdispersion 生成 gene counts。
- 观测 RNA 使用 Negative Binomial 建模，目标不是把 log-normalized matrix 当高斯输入硬融合。
- 这使 library size、有限检测灵敏度与 batch-aware normalization 能落在模型内部处理。

### 3. Protein 生成支路
- protein 不是一个单峰 count 模型。totalVI 用背景/前景混合逻辑处理每个蛋白。
- `beta` 表示 protein background intensity；`alpha` 表示 foreground scale；`pi` 表示 background probability。
- 这一步对免疫 phenotype 很关键：若把 isotype-like background 与真实 low abundance marker 混淆，后续 T-cell subset 边界会被人为拉伸。

### 4. 变分推断与下游输出
- totalVI 用 amortized variational inference 学习神经网络参数与近似后验。
- 输出可包括：
  - posterior latent representation，用于 neighbors/UMAP/clustering/integration
  - denoised normalized RNA 与 protein expression
  - protein background-aware differential analysis
  - feature association/correlation estimates
- 因此它的工程意义不只是“再造一个 UMAP”，而是给同一概率语义下的多个分析读出。

## 相比已有方法的算法增量
- 相比分别分析 RNA 与 ADT：保留 paired-cell joint state。
- 相比简单 concatenation 或各模态 PCA 后拼接：技术噪声与 count 分布不再被忽略。
- 相比只做 batch correction：把 protein background、modality likelihood 与 downstream differential testing 统一到模型内。
- 对后续算法的影响：它把单细胞免疫多模态建模推进到可比较的 latent generative model 路线。

## 局限与新算法空间
1. **donor hierarchy 不够强**：covariate 可输入 donor，但 population-level outcome、重复采样、个体层不确定性还需更明确层级结构。
2. **TCR 缺席**：对 T 细胞最关键的 clonotype 与 receptor sequence 没有成为 latent prior 或 graph relation。
3. **组织生态缺席**：protein panel 与 RNA 共模，不等于建模 tissue niche、antigen exposure 与 cellular interaction。
4. **模态权重解释困难**：官方文档也提示低维表示中 RNA 与 protein 平衡不易直观解释。
5. **扩展方向**：`RNA + ADT + TCR + donor outcome` 的 uncertainty-aware multimodal model，或 clone-aware totalVI-like latent space，仍值得做。

## 数据可用性
- accession 级别：
  - 新数据 `SLN-all`：GEO `GSE150599`
  - 公共 benchmark：10x Genomics `PBMC5k`、`PBMC10k`、`MALT` CITE-seq 页面由论文 Data availability 明列
- 数据性质：
  - `SLN-all`：小鼠脾脏/淋巴结 immune CITE-seq，RNA 与 >100 surface proteins 成对测量
  - benchmark：人 PBMC 与 MALT RNA-protein paired immune datasets
- 处理后数据：论文说明 processed `SLN-all` 也放入 reproducibility GitHub
- 文章提供的代码：
  - 复现仓库：<https://github.com/YosefLab/totalVI_reproducibility>
  - Zenodo snapshot：https://doi.org/10.5281/zenodo.4330368
  - 参考实现：<https://github.com/scverse/scvi-tools>
- 代码输入：`AnnData`/矩阵级 paired gene counts、protein counts、batch/donor/panel covariates
- 代码输出：shared latent embedding、normalized/denoised RNA 和 protein、differential expression/abundance 结果、association estimates
- 模型结构与意义：shared latent variable + RNA Negative Binomial branch + protein background/foreground mixture branch + amortized variational inference；它把 CITE-seq 的 biology/technical decomposition 变成可复用模型
- 复用判断：**高**。数据、模型实现与论文复现代码都可定位；需要注意原论文新数据为小鼠 immune tissue，不是人群 T-cell cohort。

## 可放入 method report 的表述
totalVI 代表了单细胞免疫多模态算法从“联合测量”走向“联合概率建模”的阶段：它在 cell-level 将 RNA count、surface-protein count、protein background 与 batch effect 统一建模，但尚未把 donor hierarchy、TCR clonotype 与人群免疫结局纳入同一表示空间。

## 一句话结论
totalVI 是 RNA-protein 单细胞多模态建模的主干方法，适合作为后续开发 T-cell clone-aware、donor-aware multimodal algorithms 的直接起点。
