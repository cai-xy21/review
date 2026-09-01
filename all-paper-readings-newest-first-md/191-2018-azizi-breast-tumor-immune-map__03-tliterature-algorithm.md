# Algorithm Report 026

## Paper
Single-cell Map of Diverse Immune Phenotypes in the Breast Tumor Microenvironment

## 题录与数据
- DOI：https://doi.org/10.1016/j.cell.2018.05.060
- GEO：`GSE114727`、`GSE114725`
- BioProjects：`PRJNA472383` (3-prime RNA atlas)、`PRJNA472381` (5-prime RNA + TCR)
- 数据规模：8 human breast tumors，约 45k immune atlas cells；追加约 27k paired RNA/TCR T cells

## 算法定位
这是 resource/biology paper 中带明确 computational contribution 的条目。直接算法工件是 `SEQC` 与 `Biscuit`；更大的方法学价值是定义 `T-cell state + TCR usage + local immune ecosystem` 的真实问题。

## 直接算法工件
### SEQC
- 输入：raw single-cell sequencing run data
- 输出：count/QC 层可分析输入
- 角色：preprocessing pipeline，降低 raw data engineering 对 atlas analysis 的不确定性
- 代码：<https://github.com/ambrosejcarr/seqc.git>

### Biscuit
- 输入：single-cell expression count matrix
- 输出：cluster assignments、normalized/imputed expression、state structure
- 角色：Bayesian clustering + normalization + imputation
- 论文强调点：cell populations 可由 mean expression 与 covariance/co-expression patterns 共同区分
- 代码：<https://github.com/sandhya212/BISCUIT_SingleCell_IMM_ICML_2016>

## 分析链
1. raw sequencing preprocessing and QC
2. Bayesian normalization/clustering across tumor immune samples
3. atlas annotation of lymphoid/myeloid immune states
4. continuous T-cell activation/differentiation phenotype analysis
5. paired RNA/TCR analysis connecting receptor utilization and phenotype diversity

## 对新算法贡献程度
- 直接算法创新：**中**
- 高价值数据/任务定义：**高**
- T-cell context relevance：**高**
- population-level generalization：**中**

## 对方法论文最有用的观点
- T-cell state 不是孤立点，local immune ecosystem 是 condition variable。
- T-cell phenotype 可能呈 continuous expansion，硬聚类只是起点。
- TCR usage 与 transcriptome phenotype 的关系值得模型化，而不只是 metadata 回填。

## 局限
- cohort 规模不足以直接做 broad population immunity model
- `SEQC` 与 `Biscuit` 不是现代 receptor-aware multimodal foundation model
- TCR/state/context 尚未端到端联合

## 新算法空间
1. microenvironment-aware T-cell latent model
2. clone graph + cell-cell interaction graph co-regularization
3. donor-comparable continuous phenotype volume metrics
4. context-conditioned tumor T-cell trajectory inference

## 最终判断
026 应在报告里写成“图谱论文如何暴露算法问题”的代表，而不是误写成纯算法 benchmark。它提供的数据和 context framing 对后续新模型立项很有价值。
