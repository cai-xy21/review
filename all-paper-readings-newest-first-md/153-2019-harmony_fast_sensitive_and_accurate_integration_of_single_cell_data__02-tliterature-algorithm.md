# Algorithm Report 017

## Paper
Fast, sensitive and accurate integration of single-cell data with Harmony

## 基本信息
- 年份/期刊：2019, Nature Methods
- DOI：https://doi.org/10.1038/s41592-019-0619-0
- PMID/PMCID：31740819 / PMC6884693
- 代码：Harmony <https://github.com/immunogenomics/harmony>
- 评估指标代码：LISI <https://github.com/immunogenomics/lisi>
- 复现脚本：<https://github.com/immunogenomics/harmony2019>

## 算法视角定位
Harmony 代表 embedding-level integration 路线：它不重建完整 count model，而是在 PCA/低维空间中快速校正 batch/donor/technology 等协变量。对大规模 PBMC/T-cell atlas 来说，它的优势是速度快、内存低、可同时处理多个 covariates，适合百万细胞级别的实用整合。

## 数据与任务
- 物种/样本：论文复用公开数据，包括 PBMC、pancreatic islet、mouse embryogenesis、spatial transcriptomics 等
- 免疫相关数据：跨技术 PBMC 数据用于展示 fine-grained subpopulation integration
- 规模：作者报告 Harmony 可在个人电脑上整合约 `10^6` cells
- 模态：主要是 scRNA-seq embedding，也展示 spatial/scRNA integration
- 数据可用性：论文说明所有数据来自公开来源，数据源列在 Supplementary Table 8；不是单一新 cohort accession

## 核心算法结构
### 1. 输入低维表示
- 用户先对表达矩阵做 PCA 或其他 embedding。
- Harmony 在 embedding 上运行，因此避免直接在高维 count matrix 上做复杂模型。
- 输入还包括 batch、donor、technology、study 等 metadata covariates。

### 2. Soft clustering
- Harmony 在 embedding 空间中对细胞做 soft k-means 风格聚类。
- 聚类目标同时考虑 cell similarity 和 batch diversity。
- 每个 cluster 被鼓励包含多批次/多数据集的相似细胞，而不是被单一 batch 主导。

### 3. Batch-aware correction
- 在每个 soft cluster 内估计 batch-specific offset。
- 将 offset 从 cell embedding 中减去，得到 corrected embedding。
- soft clustering 与 correction 交替迭代，直到收敛。

### 4. 多协变量处理
- Harmony 可以同时纳入多个 experimental/biological factors。
- 这对人群免疫研究重要，因为 donor、sample processing、platform、site、condition 往往同时存在。

## 输入、输出与模型意义
- 输入：PCA/low-dimensional embedding、cell metadata、需要校正的 covariate names、参数如 theta/lambda/sigma
- 输出：Harmony-corrected embedding；下游用于 UMAP/t-SNE、clustering、neighbor graph、label transfer
- 模型结构：soft clustering + mixture-of-experts-style batch correction in embedding space
- 方法意义：用轻量、可扩展的方式实现 shared cell-state embedding

## 对新算法开发的贡献程度
- 直接算法创新：高
- 可扩展工程价值：极高
- 对 T 细胞人群研究贡献：高
- 综合评估：**P0/P1 级实用整合方法**

## 局限与风险
1. Harmony 校正的是 embedding，不产生 corrected raw count matrix。
2. 如果 condition 与 batch/donor 高度混杂，可能过度校正真实 disease/vaccine/age signal。
3. 稀有 T-cell states 如果只存在于少数 donor 或条件中，可能被错误拉向主群体。
4. 下游 differential expression 仍应回到原始/合适归一化表达矩阵或 pseudobulk。

## 新算法空间
1. Rare-state preserving integration：显式保护少数 donor 中真实存在的 antigen-specific T-cell states。
2. Donor-aware Harmony extension：把 donor random effect 与 batch effect 分开建模。
3. Integration diagnostics：自动判断哪些 covariates 可以校正、哪些可能包含真实生物信号。
4. Multimodal lightweight correction：把 RNA embedding、ADT embedding、TCR feature embedding 联合做快速协变量校正。

## 可纳入 method report 的一句话
Harmony 的关键贡献是把单细胞整合转化为低维空间中的快速迭代校正问题，在大规模人群免疫图谱中极其实用，但对 donor/condition 混杂和稀有 T-cell state 过度校正仍需谨慎。
