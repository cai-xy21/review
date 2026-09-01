# 082. scvi-tools: a library for deep probabilistic analysis of single-cell omics data

## 基本信息
- 年份：2022
- 期刊：Nature Biotechnology
- DOI：https://doi.org/10.1038/s41587-021-01206-w
- PMID：35132262
- 代码：<https://github.com/scverse/scvi-tools>
- 文档：<https://docs.scvi-tools.org>
- 主题：deep generative model；probabilistic single-cell analysis；scVI/scANVI/totalVI/PeakVI；batch correction；reference mapping；multimodal integration

## 为什么重要
scvi-tools 是单细胞深度概率模型的通用软件库，把 scVI、scANVI、totalVI、PeakVI、DestVI 等模型统一到 Python API 中。对 T 细胞与人群免疫力方向，它不是一篇 T 细胞生物学论文，但它给出了跨 donor、跨 batch、跨组织、跨模态建模的核心算法工具箱，是后续 donor-aware、multimodal、uncertainty-aware immune representation learning 的基础。

## 数据与研究设计
- 数据性质：方法/软件库论文；整合多个已发表模型和 benchmark，而不是单一新队列。
- 物种/器官：工具不限定；示例和 benchmark 常覆盖 human PBMC、CITE-seq、spatial、ATAC 等公开数据。
- 数据可用性：模型训练使用的 benchmark 来自各原始论文公开数据；scvi-tools 本文主要开放代码、文档和教程。
- 与本综述相关的数据类型：scRNA-seq count matrix、CITE-seq RNA+ADT、scATAC peak matrix、spatial transcriptomics、reference/query atlas。

## 核心亮点
1. 以概率生成模型统一 normalization、batch correction、imputation/denoising、latent representation、differential expression 和 reference mapping。
2. 对 raw counts 建模，而不是只在标准化矩阵上做几何校正。
3. 对多模态数据提供 totalVI、PeakVI 等模型，能处理 RNA/protein 或 ATAC 的不同噪声结构。
4. 输出 latent posterior 和不确定性，可为个体免疫表型预测提供更稳健特征。

## 文章中的算法/分析流程
### 1. scVI family 的基本框架
- 输入是基因表达 count matrix 和 batch/donor/condition covariates。
- 模型通常使用 variational autoencoder：encoder 从 observed counts 推断 latent variable；decoder 根据 latent state 和 batch covariates 生成 count distribution。
- scRNA 表达常使用 negative binomial 或 zero-inflated negative binomial likelihood。
- 输出包括低维 latent embedding、batch-corrected representation、normalized expression、DE 统计和 query-to-reference mapping。

### 2. semi-supervised 和 reference mapping
- scANVI 在 scVI latent space 上加入 label information，可做 semi-supervised cell type annotation。
- 对免疫图谱而言，它适合把新 cohort 的 T cell subsets 映射到 reference atlas，同时保留 batch/donor variation 的建模能力。

### 3. multimodal extensions
- totalVI：joint modeling of RNA counts and surface protein ADT counts，蛋白分支显式建模 background/foreground mixture。
- PeakVI：对 scATAC peak accessibility 做 probabilistic latent variable modeling。
- 这些模块使 RNA、蛋白、ATAC 可以在 likelihood-aware 框架下建模，但标准 scvi-tools 仍没有原生把 TCR sequence graph 纳入同一 generative process。

## 代码输入、输出、模型结构和意义
- 输入：AnnData object；raw count matrix；batch/sample covariates；可选 protein count matrix、peak matrix、labels。
- 输出：latent representation、denoised/normalized expression、batch-corrected neighbor graph、cell type prediction、differential expression、posterior uncertainty、trained model weights。
- 模型结构：VAE/conditional VAE/semi-supervised VAE；不同模态采用各自 likelihood。
- 意义：把单细胞数据整合从 deterministic embedding correction 推向 probabilistic representation learning。

## 与 T 细胞—人群免疫力的关系
- T cell state 往往受到 donor、batch、organ、disease severity、activation history 共同影响；scvi-tools 提供建模这些 covariates 的基础。
- 适合做 PBMC/组织 T cell atlas integration、CITE-seq immune state denoising、reference mapping 和 differential abundance/DE 的特征层前处理。
- 对人群免疫力建模的缺口在于：donor-level outcome、TCR/BCR sequence、HLA genotype、exposure history 尚未被标准模型统一吸收。

## 对新算法贡献程度
- 直接算法贡献：高。虽然 scvi-tools 是软件库论文，但其封装的是一系列概率生成模型。
- 数据资源价值：中。本文本身不是新数据资源，价值在代码和模型实现。
- 对新算法启发：很高。它为 donor-aware immune foundation model 提供架构基础。

## 数据可用性
- 新实验数据：无单一新 accession；benchmark 数据来自各模型原论文公开资源。
- 代码仓库：<https://github.com/scverse/scvi-tools>
- 文档/教程：<https://docs.scvi-tools.org>
- 软件输入/输出：见上文。
- 复现性评价：代码和文档非常成熟；论文级 benchmark 的完全复现需回到各原始数据集。

## 一句话结论
scvi-tools 是单细胞概率建模的基础库，适合作为本文方法报告中“已有算法”章节的核心 P0 条目；真正的新空间在于把 donor hierarchy、TCR/BCR sequence 和 immune outcome 纳入这类生成模型。
