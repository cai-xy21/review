# Algorithm Report 018

## Paper
Normalization and variance stabilization of single-cell RNA-seq data using regularized negative binomial regression

## 基本信息
- 年份/期刊：2019, Genome Biology
- DOI：https://doi.org/10.1186/s13059-019-1874-1
- PMID/PMCID：31870423 / PMC6927181
- 代码：sctransform <https://github.com/satijalab/sctransform>
- Seurat 接口：`SCTransform`

## 算法视角定位
SCTransform 是 UMI-based scRNA-seq 预处理的基础方法。它把 normalization 从 library-size scaling + log transform 改写为 regularized negative-binomial regression，并用 Pearson residuals 作为 variance-stabilized expression。对 T 细胞人群数据来说，这一步直接影响后续 variable genes、PCA、clustering、integration 和 rare state discovery。

## 数据与任务
- 物种/样本：论文使用多个公开 UMI scRNA-seq datasets；示例包括 10x human PBMC
- 代表规模：文中 PBMC 示例包含 `33,148` PBMC；教程示例也包括 10x `2,700` PBMC
- 器官/组织：外周血/PBMC 等公开 benchmark
- 数据性质：通用预处理方法论文，不是单一新 cohort accession
- 任务定义：消除 sequencing depth 等技术因素对表达方差的影响，同时保留真实生物异质性

## 核心算法结构
### 1. Gene-wise negative-binomial regression
- 对每个基因建模 UMI count。
- cell-level sequencing depth 作为 covariate。
- 目标是解释技术性 depth variation，而不是通过统一 size factor 简单缩放。

### 2. Regularization
- 低/中表达基因的 per-gene NB 参数估计容易过拟合。
- SCTransform 将参数估计按基因平均丰度做平滑/正则化。
- 这使 NB model 更稳定，避免低表达基因获得不合理的 dispersion estimate。

### 3. Pearson residual transformation
- 用拟合模型得到 expected count 与 variance。
- 计算 Pearson residual：观测值相对期望值的标准化偏差。
- residual matrix 成为 variance-stabilized representation，供 PCA、clustering、variable feature selection 使用。

### 4. Feature selection
- 基于 residual variance 识别 variable features。
- 这样 variable gene selection 不再强烈受测序深度和平均表达量影响。

## 输入、输出与模型意义
- 输入：UMI count matrix；可选 technical covariates，如 mitochondrial content、cell cycle score 等
- 输出：Pearson residual matrix、corrected counts/assay、variable feature ranking、模型参数
- 模型结构：regularized negative-binomial GLM + Pearson residual variance stabilization
- 方法意义：提供比 log-normalization 更统计化的 UMI normalization 入口

## 对新算法开发的贡献程度
- 直接算法创新：高
- 对下游流程影响：极高
- 对 T 细胞人群研究贡献：高
- 综合评估：**P1 级预处理基础算法**

## 局限与风险
1. 模型把 sequencing depth 作为技术协变量，但在免疫细胞中 RNA content 也可能与 activation state 相关。
2. 高度异质样本中，“大多数基因不强烈生物变化”的假设可能被挑战。
3. residuals 适合降维和聚类，但 differential expression 的统计解释需要小心。
4. 对 ADT/TCR/ATAC 等模态不能直接套用同一噪声模型。

## 新算法空间
1. Activation-aware normalization：区分 T-cell activation 导致的真实 RNA content 增加与技术 depth。
2. Joint QC-normalization model：把 ambient RNA、doublet probability、cell calling uncertainty 纳入 NB regression。
3. Donor-aware residuals：在 cohort 中建模 donor random effect 后再做 variance stabilization。
4. Multimodal residual framework：为 RNA、ADT、TCR-derived features 建立可比的 residual representation。

## 可纳入 method report 的一句话
SCTransform 的核心贡献是用 regularized NB regression 和 Pearson residuals 替代启发式 log-normalization，显著改变了单细胞 T-cell state 发现和跨样本整合的统计入口。
