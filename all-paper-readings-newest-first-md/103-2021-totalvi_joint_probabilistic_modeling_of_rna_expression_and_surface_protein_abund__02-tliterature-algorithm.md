# Algorithm Report 021

## Paper
Joint probabilistic modeling of single-cell multi-omic data with totalVI

## 题录与资源
- DOI：https://doi.org/10.1038/s41592-020-01050-x
- 新数据 accession：GEO `GSE150599` (`SLN-all`)
- 复现代码：<https://github.com/YosefLab/totalVI_reproducibility>
- 参考实现：<https://github.com/scverse/scvi-tools>

## 算法定位
totalVI 是 paired CITE-seq 的 cell-level deep generative model。它要解决的不是“如何把两个 embedding 画到一张图”，而是如何把 RNA count、protein count、protein background、library size 与 batch covariates 放入一致的概率语义，再从后验中读出 embedding、denoising 与 differential analysis。

## 模型输入
- `X`：`N x G` RNA UMI count matrix
- `Y`：`N x T` ADT/protein UMI count matrix，与 `X` 按细胞配对
- `S`：可选 design/covariate matrix，常见字段包括 batch、day、donor 或 panel-related covariates
- 实现层通常使用 `AnnData`，需要保留 raw counts 与 protein counts

## 模型结构
### Shared latent layer
- 每个细胞有 latent state `z_n`
- `z_n` 同时驱动 RNA 与 protein 解码器
- covariates 进入生成与推断过程以吸收观测技术差异

### RNA branch
- decoder 预测 gene expression scale
- RNA library size 与 gene-specific dispersion 被建模
- 观测基因 counts 使用 Negative Binomial likelihood

### Protein branch
- 为每个蛋白区分 foreground 与 background
- `beta` 表示 background intensity
- `alpha` 表示 foreground scaling
- `pi` 表示落入 background 的概率
- 观测 protein counts 使用 mixture-aware count likelihood

### Inference
- 使用 amortized variational inference 学习模型参数与 approximate posterior
- 训练后的 posterior mean/sample 可作为 latent representation

## 代码输入与输出
### 输入
- paired RNA/protein counts
- per-cell batch/donor covariates
- antibody panel 与缺失 protein 设置

### 输出
- `X_totalvi` 风格 shared latent representation
- normalized/denoised RNA expression
- denoised protein foreground expression 与 background-aware quantities
- differential expression/abundance statistics
- RNA-protein association/correlation estimates

## 算法贡献拆解
1. **Likelihood 选型正确**：count data 不被粗糙高斯化。
2. **蛋白背景显式化**：ADT 非特异性背景从 phenotype signal 中拆出。
3. **后验复用**：同一模型服务 embedding、denoising、integration、DE。
4. **多 batch 场景可落地**：batch covariates 和 missing proteins 让 CITE-seq cohort 更可分析。

## 与 T 细胞算法的连接
- 适合作为 T-cell RNA/ADT phenotype space 的上游表示。
- 对基于 surface proteins 界定 activated/memory/cytotoxic/exhaustion-like states 的任务比 RNA-only workflow 更自然。
- 标准 totalVI 没有 receptor branch；TCR clonotype 目前只能后接 metadata、graph 或另建 decoder。

## 对新算法贡献程度
- 直接算法创新：**高**
- 免疫/T 细胞可迁移性：**高**
- 人群 donor-level 直接建模：**中**
- 作为我们方法论文的核心引用优先级：**P0**

## 可继续开发的空间
1. totalVI latent 加入 clonotype graph 或 sequence encoder
2. 在 cell latent 之上加 donor-level random effect / outcome head
3. 面向跨抗体 panel、跨组织、跨疾病的 uncertainty calibration
4. 与 longitudinal exposure 或 vaccination response 联合建模

## 数据与复现判断
- `SLN-all` 是 mouse spleen/lymph-node CITE-seq，新数据 accession 明确
- 论文还使用 10x 人 PBMC 与 MALT benchmark
- reproducibility repository、Zenodo snapshot 与 `scvi-tools` 均可定位
- 复现实验适合先从 official `scvi-tools` tutorial 与 public benchmark 起步，再映射到 T-cell cohort

## 最终判断
totalVI 是本批文献中最强的 cell-level multimodal algorithm paper。它的缺口正好指向我们可写的新问题：人群免疫研究需要在 totalVI 式多模态去噪之上继续加入 donor、TCR 与 immune outcome。
