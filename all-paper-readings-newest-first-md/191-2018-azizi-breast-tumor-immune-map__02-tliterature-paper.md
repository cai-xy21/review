# 026. Single-cell Map of Diverse Immune Phenotypes in the Breast Tumor Microenvironment

## 基本信息
- 年份：2018
- 期刊：Cell
- DOI：https://doi.org/10.1016/j.cell.2018.05.060
- PMID/PMCID：29961579 / PMC6348010
- 主题：breast tumor immune atlas；T-cell trajectories；TCR utilization；SEQC；Biscuit

## 为什么重要
这篇论文不是纯 T-cell algorithm paper，但它有两层值得写进 method report 的价值。第一，它把 T 细胞放回肿瘤免疫生态中，说明 phenotype 不只是内在转录程序，也受 tissue microenvironment 与 receptor usage 共同塑造。第二，它在图谱构建过程中明确提出 `SEQC` preprocessing pipeline 与 `Biscuit` Bayesian clustering/normalization，为复杂肿瘤单细胞数据提供了实际算法环节。

## 数据与研究设计
- 主 atlas：约 `45,000`/文中细化为 `47,016` 个 CD45-positive immune cells
- 患者：8 个 treatment-naive primary breast carcinomas，覆盖不同 breast cancer subtypes
- 配对组织：matched normal breast tissue、peripheral blood、lymph node
- 追加 T-cell 数据：约 `27,000` 个 T cells 的 paired single-cell RNA 与 TCR sequencing
- 物种/器官：`Homo sapiens`；breast tumor microenvironment、normal breast、blood、lymph node
- 主要问题：
  - 肿瘤局部是否扩展了连续 immune phenotypes
  - T-cell activation/differentiation 是否更接近 continuous trajectories
  - TCR utilization 与局部环境如何共同关联 T-cell phenotype

## 核心贡献
1. **组织情境中的 T-cell state**：T 细胞不是孤立 cluster，而是 tumor ecosystem 中连续变化的表型体积。
2. **肿瘤免疫图谱**：联合 lymphoid 与 myeloid immune states，便于讨论跨细胞类型 coupling。
3. **算法工件**：
   - `SEQC` 负责 raw single-cell sequencing preprocessing
   - `Biscuit` 用 Bayesian clustering、normalization 与 imputation 稳定跨文库/跨样本状态识别
4. **paired RNA/TCR 连接**：TCR usage 被接回 phenotype diversity，提前暴露了 clone/state 联合算法需求。

## 与 T 细胞-人群免疫力的关系
- 本文关注肿瘤局部免疫，而不是健康人群 immune baseline；但它提醒我们 population T-cell algorithms 不能只建模 isolated lymphocyte transcriptomes。
- 人与人之间 T-cell state 差异可能来自 clone history、tissue context、ligand environment 与 measurement depth 的叠加。
- 因此它适合支撑 `microenvironment-aware` 与 `clone-state-context` 算法机会，而不适合作为全身免疫力评分的直接训练集。

## 文章中的算法贡献
### 1. SEQC preprocessing
- SEQC 面向单细胞测序 raw data 的工程问题，论文把它作为 preprocessing pipeline 提出。
- 对后续算法的意义是先把 alignment/counting/QC 形成可靠输入，否则 tumor samples 的 library quality 与 depth 差异会直接污染 state comparisons。
- 在代码复用上，SEQC 是 raw reads 到 count-level object 的上游工具，而不是 downstream latent model。

### 2. Biscuit Bayesian normalization/clustering
- 论文用 Biscuit 处理不同 library molecule counts 带来的可比性问题。
- Biscuit 联合做 clustering、normalization 与 imputation，并利用 mean 与 covariance patterns 区分 cell populations。
- 文中强调 T-cell clusters 的差异不只在平均表达，也在 co-expression/covariance structure；这比只比较 marker means 更接近状态程序分析。

### 3. Continuous phenotype analysis
- atlas 层先识别 immune clusters，再考察 tumor 中 phenotype expansion 与 activation/differentiation continuum。
- 对 T cells，论文的算法启发是状态边界可能是连续谱而非离散 subtype，cluster 数量本身不应替代 trajectory/phenotypic volume 建模。

### 4. RNA/TCR coupling
- paired scRNA/TCR 使 TCR utilization 与 T-cell phenotype diversity 能被联合查看。
- 该连接主要仍是分析层回填，不是一个端到端 receptor-conditioned latent model；这正是后续方法空间。

## 相比已有方法的算法增量
- 相比只做 marker-based tumor infiltrate 描述：给出高分辨 immune atlas 与 continuous state framing。
- 相比忽略 library count differences 的 naive normalization：Biscuit 试图把 normalization 与 clustering 统一。
- 相比 transcriptome-only T-cell atlas：加入 paired TCR assay，强调 receptor usage 与 local context。

## 局限与新算法空间
1. **直接算法创新集中在前处理/聚类层**：论文主体仍是资源与 biological interpretation。
2. **患者数有限**：8 tumors 对真正 population-level generalization 不够。
3. **TCR 信息未成为模型结构**：尚未联合编码 sequence、clone expansion 与 phenotype transition。
4. **可做方向**：
   - tumor niche-aware T-cell representation
   - cell-cell graph 与 clone graph 共同约束
   - continuous phenotype volume 的跨 donor comparison
   - context-conditioned exhaustion/activation trajectory

## 数据可用性
- GEO：
  - `GSE114727`
  - `GSE114725`
- NCBI BioProject：
  - `PRJNA472383`：3-prime RNA sequencing atlas
  - `PRJNA472381`：5-prime RNA sequencing and TCR sequencing
- HCA Data Explorer 也收录该项目，可作为矩阵级资源入口
- 数据性质：
  - 人 breast tumor immune atlas：8 tumors，加 matched normal breast、blood、lymph node
  - 追加 paired RNA/TCR T-cell cohort：约 27,000 T cells
- 文章提供的代码：
  - `SEQC`：<https://github.com/ambrosejcarr/seqc.git>
  - `Biscuit`：<https://github.com/sandhya212/BISCUIT_SingleCell_IMM_ICML_2016>
- 代码输入：
  - `SEQC`：single-cell sequencing raw reads / run-level files，生成 count/QC 层输入
  - `Biscuit`：single-cell expression count matrix
- 代码输出：
  - `SEQC`：preprocessed molecule/count matrices 与 QC artifacts
  - `Biscuit`：Bayesian cluster assignments、normalized/imputed expression 与状态结构
- 模型结构与意义：`SEQC` 保证 raw-to-count 输入质量；`Biscuit` 用 Bayesian normalization/clustering 处理 library scale 与 co-expression structure；两者服务于 tumor immune atlas 中跨样本 phenotype discovery
- 复用判断：**高数据价值，中高算法复用价值**。数据 accessions、代码与 TCR extension 均明确，但它不是专为现代 donor-aware multi-omics model 设计的 benchmark。

## 可放入 method report 的表述
Azizi et al. 表明 T-cell phenotype 应在 tissue immune ecosystem 中理解；其 `SEQC + Biscuit + paired RNA/TCR` 分析链在 2018 年已把 preprocessing、Bayesian normalization 与 receptor-context coupling 放到同一 tumor atlas 场景，但后续仍需要更统一的 clone-state-context model。

## 一句话结论
这篇论文对新算法的价值在于给出一个真实而复杂的 T-cell context problem：既有状态连续性、局部生态，又有 TCR 使用信息，不能靠单一 cluster label 解决。
