# Algorithm Report 076

## Paper
COVID-19 immune features revealed by a large-scale single-cell transcriptome atlas

## 算法视角定位
这篇 Cell 论文是 COVID-19 large-scale single-cell atlas 的核心资源。它分析 196 个 individuals/controls 的 284 个样本，整合 1,462,702 个细胞，覆盖 PBMC、BALF、sputum、pleural fluid 等。算法贡献主要是 **大规模多组织 single-cell atlas construction + clinical-feature association + viral RNA-positive cell analysis + ligand-receptor/cytokine score analysis**。

## 数据模态与样本设计
- 模态：scRNA-seq，部分 5' V(D)J/TCR/BCR-seq，cytokine measurements，IHC validation。
- 物种/器官：`Homo sapiens`；PBMC、BALF、sputum、PFMC/pleural fluid。
- 样本：284 human samples from 196 COVID-19 patients and controls。
- 样本构成：249 PBMC-related samples，35 respiratory-system samples，包括 12 BALF、22 sputum、1 PFMC。
- 规模：QC 后 1,462,702 cells；major lineages 包含 B/plasma B、myeloid、NK、epithelial、CD4 T、CD8 T；进一步分成 64 clusters，其中 T-cell clusters 约 28 个。

## 关键算法问题
1. 如何在百万级 multi-center COVID single-cell 数据中做统一 QC、annotation 和 cluster-level interpretation。
2. 如何把 clinical features, severity, age, sex, disease stage 与 cell subtype abundance/expression 关联。
3. 如何在 scRNA reads 中检测 SARS-CoV-2 viral RNA-positive cells，并比较 viral-positive vs viral-negative cells。
4. 如何在 PBMC 与 respiratory samples 中比较 hyper-inflammatory cell types 和 ligand-receptor interactions。

## 论文中的算法贡献
### 1) Million-cell atlas integration
文章用 kallisto/bustools 对 scRNA-seq reads 进行比对和计数，经过 UMI/gene/mitochondrial/erythrocyte filtering 后保留 1.46M cells，并用 marker genes 注释 6 major cell types 与 64 clusters。

**算法意义**：这是大规模 disease atlas 的资源型贡献。它为后续大模型、metacell、reference mapping 和 severity prediction 提供训练/测试数据。

### 2) Observed/expected enrichment score
文章使用 observed-to-expected cell number ratio (`R_O/E`) 来衡量 cell clusters 对 tissue 或 patient group 的偏好。

**方法学价值**：这是 composition association 的直观统计方法，适合 atlas-level display；但它未显式建模 donor random effects 和 compositional dependency。

### 3) Clinical-feature association
文章将 cell subtype proportion/expression 与 age、sex、severity、disease stage 等临床变量关联，识别不同临床特征对应的 immune subtype changes。

**算法意义**：定义了 `cell subtype x clinical covariate` 的 population-immunity mapping task。

### 4) Viral RNA-positive single-cell analysis
文章构建包含 SARS-CoV-2 genome 的 custom reference，用 kallisto/bustools 检测 viral RNA-positive cells，并比较 viral-positive and viral-negative cells 的 transcriptomic changes。

**方法学价值**：
- 把 host cell state 和 pathogen reads 放在同一 single-cell object。
- 适合开发 host-pathogen joint expression model。

### 5) Cytokine/inflammatory scoring and interaction analysis
文章用 cytokine score/inflammatory score 识别 hyper-inflammatory cell subtypes，尤其是 monocyte/macrophage/neutrophil-related populations，并使用 ligand-receptor 分析探索 lung/peripheral blood interactions。

**局限**：score 和 ligand-receptor 仍是 expression-derived association，不等于 causal signaling。

## 不是它解决得很好的问题
1. 多中心、多平台、多组织数据整合易受 batch/tissue/confound 影响。
2. cell-level 样本巨大，但 clinical inference 仍应以 donor/sample 为单位。
3. TCR/BCR 信息虽存在，但未成为统一 clone-state-severity model。
4. viral RNA detection 受 ambient RNA、mapping 和 capture bias 影响，需要严格建模。
5. ligand-receptor 分析缺少空间邻近和功能验证约束。

## 数据可用性评估
- DOI：https://doi.org/10.1016/j.cell.2021.01.053
- Erratum DOI：https://doi.org/10.1016/j.cell.2021.10.023
- Processed gene expression：GEO `GSE158055`
- Raw data：GSA-Human `HRA001149`
- Raw data link：<https://ngdc.cncb.ac.cn/gsa-human/browse/HRA001149>
- Visualization portal：<http://covid19.cancer-pku.cn>
- Supplemental data：Mendeley Data <https://doi.org/10.17632/dvp4y5ttd5.1>
- 可确认数据性质：human PBMC and respiratory samples; 196 individuals; 284 samples; 1.46M cells; COVID severity/stage labels。
- 代码：未在 Data Availability 中定位到独立完整 GitHub；方法描述足够复现主要流程，但完整 pipeline 复现需自行实现。
- 复用性：很高。processed/raw accession 均明确，规模适合作为 algorithm benchmark。

## 代码/模型结构
- 输入：raw FASTQ/scRNA reads、human GRCh38 + SARS-CoV-2 custom reference、clinical/sample metadata、V(D)J where available。
- 核心流程：`kallisto/bustools quantification -> QC filtering -> major lineage annotation -> cluster annotation -> R_O/E tissue/group enrichment -> clinical association -> viral RNA-positive detection -> cytokine/inflammatory scoring -> ligand-receptor analysis`
- 输出：large-scale COVID atlas, annotated cell clusters, clinical-feature-associated immune states, viral-positive cell profiles, interaction/cytokine modules。
- 模型意义：百万级 infection atlas benchmark，适合后续 donor-aware, tissue-aware, pathogen-aware representation learning。

## 对新算法贡献程度评估
- 定义任务价值：很高
- 数据资源价值：很高
- 直接算法创新：中
- 对后续方法启发：很高

综合评估：**高价值 large-scale disease atlas；直接算法创新主要在整合分析和 host-pathogen single-cell task definition，而非新通用模型。**

## 可发展的新算法空间
### A. Donor-aware large-atlas association model
替代简单 R_O/E 和 cell-level DEG，用 mixed model/compositional model 估计 severity/age/sex/stage 对 cell states 的影响。

### B. Host-pathogen joint model
同时建模 viral RNA reads、ambient contamination、host cell state 和 tissue compartment。

### C. Scalable metacell/foundation representation
把 1.46M cells 压缩成 donor-aware metacells，用于跨队列 transfer 和 disease-state prediction。

### D. Clone-state-severity model
利用 V(D)J 子集，把 TCR/BCR expansion 与 transcriptomic state、severity、stage 联合建模。

## 适合纳入 method report 的表述
这篇文章提供了 COVID-19 人群免疫单细胞算法的最大资源之一。它的百万级、多组织、多阶段数据非常适合作为新算法 benchmark，但现有分析仍以 atlas annotation、group enrichment 和 module score 为主，缺少 donor-aware and pathogen-aware joint model。
