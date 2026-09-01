# Algorithm Report 031

## Paper
Single-cell transcriptomics of blood reveals a natural killer cell subset depletion in tuberculosis

## 题录与数据
- 年份/期刊：2020, EBioMedicine
- DOI：https://doi.org/10.1016/j.ebiom.2020.102686
- PMID/PMCID：32114394 / PMC7047188
- 数据：human PBMC 10x scRNA-seq；HC `n=2`、LTBI `n=2`、active TB `n=3`
- cell scale：`68,369` captured cells，QC 后 `62,628` analyzed cells
- validation：flow cytometry HC/TB cohort `81/50` 与 HC/LTBI/TB cohort `39/27/37`

## 算法视角定位
这是 **single-cell disease composition discovery** 工作，不是新 computational method。对“单细胞组学 × T-cell population immunity”的意义在于，它把 disease group、subset annotation、cell proportion 与 validation cohort 串成了可重复的问题模板。

## 输入、处理与输出
### 输入
- 10x PBMC scRNA reads/counts
- donor/group labels：HC、LTBI、active TB
- flow cytometry validation measurements

### 处理链
1. `Cell Ranger` 对齐与 count matrix 生成
2. Seurat QC、normalization、unsupervised clustering、marker detection
3. t-SNE 展示主群和细分 subsets
4. 在 HC/LTBI/TB 之间比较 subset frequency 与 marker programs
5. 通过 flow cytometry 在更大 donor cohort 复核候选 subset

### 输出
- PBMC cell/subset annotations，含 T、B、myeloid 主群及 `29` 个细分 subsets
- group-level abundance differences
- TB-associated candidate cell phenotype and validation evidence

## 详细算法贡献
### 1. 把 infection state 写成 cell-composition task
论文没有训练 disease classifier，而是把主动脉络放在 immune subset proportion shift。其隐含任务定义是：
`cells x genes + donor disease state -> cell map -> donor/group subset abundance profile`

### 2. 细胞发现和 cohort 验证分层
单细胞 discovery cohort donor 很少，作者用 flow validation 扩展 donor coverage。这一点比普通 atlas 更贴近人群免疫算法设计，因为后续算法不应只优化 cell-level separability，还应优化 donor-level transferability。

### 3. T-cell algorithm relevance 是间接的
T-cell compartment 是 PBMC map 的主要组成，但本文没有 TCR、clonal graph、trajectory 或 multimodal joint model。它可用于评估 annotation/composition robustness，不适合做 TCR-state benchmark。

## 现有算法不足
- discovery cohort donor 数太小，缺少显式 hierarchical/pseudobulk uncertainty
- subset frequency comparison 容易受 QC、cell recovery 与 sample composition shift 影响
- 无 repertoire、protein 或 epigenomic modality，不能区分 antigen-driven T-cell expansion 与一般 inflammation shift
- 原始测序 accession 与作者代码本轮未定位到，二次复现依赖正文参数与已发表图表

## 对新算法贡献程度
- 任务定义：中
- 数据资源：中低
- 直接算法创新：低
- donor-aware 方法动机：高

## 可做的新算法方向
1. **Donor-aware abundance model**：以 donor 为 replicate，联合估计 cell proportion shift 与不确定性。
2. **Discovery-to-validation transfer**：把 scRNA marker programs 转成 flow/CITE-compatible low-dimensional panels。
3. **Infection-state baseline score**：让 T/NK/myeloid coordinated shifts 形成 immune-state representation，而不是单 subset threshold。

## 数据与代码可用性
- 物种/器官：`Homo sapiens`；peripheral blood / PBMC
- 标准 accession：本轮在原文公开页面与常用库检索中未定位到
- 代码仓库：未提供作者专用仓库
- 可复用输入/输出判断：可复用的是论文定义的 PBMC disease-composition task；不是即取即用 benchmark package

## 可纳入 method report 的一句话
TB PBMC atlas work shows that scRNA discovery can expose infection-associated immune-composition shifts, but the algorithmic bottleneck is donor-aware inference and transfer to larger validation cohorts rather than cell-level clustering alone.
