# Algorithm Report 082

## Paper
scvi-tools: a library for deep probabilistic analysis of single-cell omics data

## 题录与实现
- DOI：https://doi.org/10.1038/s41587-021-01206-w
- PMID：35132262
- Code：<https://github.com/scverse/scvi-tools>
- Docs：<https://docs.scvi-tools.org>

## 算法视角定位
scvi-tools 是 deep probabilistic single-cell modeling library。它的重要性不在一个单一数据集，而在把 scVI、scANVI、totalVI、PeakVI 等模型标准化，使大规模 atlas integration、batch correction、multimodal denoising、label transfer 和 uncertainty-aware inference 可以复用。

## 输入与输出
- 输入：AnnData；raw RNA counts；batch/sample covariates；可选 protein ADT counts、ATAC peak matrix、labels、spatial coordinates。
- 输出：latent embedding、posterior normalized expression、denoised protein abundance、cell type labels、DE results、query mapping、trained model。

## 核心算法贡献
### 1. VAE-based count modeling
以 latent variable 表示细胞状态，用 decoder 生成 observed counts，并通过 negative-binomial family likelihood 处理 UMI count noise。相比 PCA/CCA/nearest-neighbor 方法，它直接面对 count distribution。

### 2. conditional batch-aware latent representation
batch、donor、protocol 等 covariates 可进入 decoder 或模型结构，从而学习 biological latent state 与技术效应的分离表示。

### 3. semi-supervised annotation
scANVI 在 latent representation 中加入 label supervision，用于 reference mapping 与 query label transfer，适合免疫 atlas 中 T cell subset 注释。

### 4. multimodal probabilistic models
- totalVI：RNA NB branch + protein foreground/background mixture。
- PeakVI：scATAC binary/count accessibility latent model。
这些模型为 RNA/ADT/ATAC 统一建模提供基础。

## 新算法贡献程度
- 直接模型创新：高。
- 工程和生态贡献：很高。
- 免疫人群建模启发：很高。

## 局限与机会
- 标准模型主要停留在 cell-level latent variable，donor-level immune fitness/outcome 需要另建层级模型。
- TCR/BCR sequence、HLA、antigen specificity 还没有作为原生模态进入 generative process。
- 可发展 `cell latent + clone graph + donor random effect + outcome head` 的免疫专用模型。

## 数据可用性评估
- 新实验 accession：不适用；本文是软件库/模型综述式论文。
- 代码：GitHub fully open。
- 复用性：极高；适合作为我们方法论文的 baseline 和实现底座。
