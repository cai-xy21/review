# Algorithm Report 071

## Paper
Global absence and targeting of protective immune states in severe COVID-19

## 算法视角定位
这篇文章是 COVID-19 severe-versus-mild immune-state framing 的代表。它使用 whole-blood-preserving single-cell RNA-seq，把 neutrophils、monocytes、platelets、lymphocytes 与 serum functional assay 放在一起分析。算法贡献不在于新模型，而在于把疾病严重程度定义为 **跨细胞类型协调的 interferon-stimulated gene (ISG) state 是否存在**。

## 数据模态与样本设计
- 模态：fresh whole-blood scRNA-seq, serum antibody/IFN functional assays, viral load, antibody titers, clinical metadata。
- 物种/器官：`Homo sapiens` peripheral whole blood；保留 granulocyte/neutrophil 信息。
- discovery cohort：8 mild/moderate COVID-19 patients, 6 severe COVID-19 patients。
- validation/expanded scRNA design：GEO 描述包含 COVID- mild 6、COVID- severe 5、COVID+ mild 11、COVID+ severe 10、healthy controls 14。
- 关键设计：whole-blood preserving protocol，而非 PBMC-only，避免丢失 neutrophil/platelet-relevant states。

## 关键算法问题
1. 如何在 whole-blood scRNA-seq 中跨 major immune lineages 定义 disease severity state。
2. 如何判断 severe disease 不是单一细胞群改变，而是系统性缺失 protective ISG-expressing state。
3. 如何把 serum functional perturbation 与 single-cell state 连接起来。
4. 如何避免 PBMC-only 数据低估 granulocyte/platelet 对人群免疫状态的贡献。

## 论文中的算法贡献
### 1) Whole-blood state preservation as analysis design
文章从全血直接裂红处理并上 10x，核心意义是把 neutrophils 和 platelets 纳入 single-cell immune profiling。

**算法意义**：这不是计算算法，但改变了可观测状态空间。COVID-19 severity 的关键差异如果落在 granulocyte/platelet 或 serum-induced programs 上，PBMC-only atlas 会产生系统性缺失。

### 2) Cross-cell-type ISG coordination score
文章在多个 cell populations 中观察 mild disease 的 coordinated ISG expression，而 severe disease 系统性缺失这些 ISG-expressing cells。

**方法学价值**：
- disease severity 不只表现为某个 cluster abundance 变化，而是跨细胞类型共享 program 的 presence/absence。
- 对新算法来说，这对应 `multicellular program detection` 或 `coordinated state module inference`。

### 3) Serum-to-state functional link
文章进一步用 severe patient serum/IgG 处理 PBMC，显示 severe serum can block IFN-induced ISG state through Fc receptor/CD32-related mechanisms。

**算法意义**：这为 causal perturbation interpretation 提供实验锚点。单细胞状态不只是相关 biomarker，而可被 serum factors 诱导或抑制。

### 4) Disease-state classification framing
文章使用 mild/moderate vs severe clinical labels，结合 UMAP、batch correction、population frequency、DE/ISG module 等常规分析，构建 severe COVID-19 的 immune-state definition。

**局限**：没有提出可复用 classifier 或 probabilistic severity model；但定义了应当预测的 phenotype。

## 不是它解决得很好的问题
1. scRNA cohort 相对不大，donor-level generalization 需要外部队列验证。
2. 没有 TCR/BCR clonotype 层面的 adaptive immune modeling。
3. cross-cell ISG coordination 主要是模块/表达统计，不是正式的 multicellular latent variable model。
4. serum perturbation 与体内机制之间仍需更系统的 causal modeling。

## 数据可用性评估
- DOI：https://doi.org/10.1038/s41586-021-03234-7
- Correction：https://doi.org/10.1038/s41586-021-03718-6
- Processed feature-barcode matrices：GEO `GSE163668`
- Raw FASTQ：SRA `SRP299788`
- GEO page：<https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE163668>
- Code：<https://github.com/UCSF-DSCOLAB/combes_et_al_COVID_2020>
- 可确认数据性质：human whole blood scRNA-seq from healthy controls, COVID-negative respiratory illness, COVID-positive mild/severe patients；外加 serum perturbation/antibody functional assays。
- 复用性：高。GEO/SRA/code 均可定位。

## 代码/模型结构
- 输入：Cell Ranger feature-barcode matrices、sample metadata、clinical severity labels、serum perturbation assay results。
- 核心流程：`whole-blood scRNA QC -> batch correction -> major-cell annotation -> ISG module analysis -> severity-associated frequency/expression comparison -> serum perturbation interpretation`
- 输出：cell type/state annotations、ISG-high protective states、severity-associated abundance/expression tables、figures and clinical metadata integration。
- 模型意义：提供 multicellular disease-state module benchmark，可升级为 donor-level immune-state classifier。

## 对新算法贡献程度评估
- 定义任务价值：高
- 数据资源价值：高
- 直接算法创新：中低
- 对后续方法启发：高

综合评估：**强 disease-state framing paper；直接算法创新有限，但对 whole-blood and multicellular program modeling 很重要。**

## 可发展的新算法空间
### A. Multicellular program model
学习跨 cell types 协同变化的 latent immune programs，而非分别做每个 cluster 的 DEG。

### B. Serum-perturbation causal model
把 ex vivo perturbation 数据与 in vivo scRNA state 结合，推断 serum factors 对 immune-state transition 的影响。

### C. Whole-blood atlas integration
开发适配 neutrophil/platelet-rich whole-blood scRNA 的 QC、ambient RNA、doublet 和 reference-mapping 方法。

### D. Severity classifier with uncertainty
使用 donor-level hierarchical model 预测 severe progression，同时报告细胞组成、ISG state 和 serum factors 的不确定性贡献。

## 适合纳入 method report 的表述
这篇文章说明 COVID-19 severe phenotype 可以表现为跨血液免疫细胞的 protective ISG state 缺失，而不是单一细胞类型异常。它为人群免疫力算法提出了 multicellular state coordination 和 perturbation-grounded interpretation 两个关键方向。
