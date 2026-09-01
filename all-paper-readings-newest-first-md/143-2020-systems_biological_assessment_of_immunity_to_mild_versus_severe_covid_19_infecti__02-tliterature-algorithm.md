# Algorithm Report 073

## Paper
Systems biological assessment of immunity to mild versus severe COVID-19 infection in humans

## 算法视角定位
这篇 Science 论文是 COVID-19 systems immunology 的早期核心文献。它把 cytokines、serology、bulk transcriptomics、CITE-seq 和 immune-cell phenotyping 放在同一 severity comparison 框架中。对我们的方法学报告，它的价值在于展示 **single-cell CITE-seq 如何作为 systems immunology 中的机制分辨层**，而不是单独的 atlas。

## 数据模态与样本设计
- 模态：CITE-seq PBMC RNA+surface protein、bulk PBMC transcriptomics、plasma cytokines、serology、clinical metadata。
- 物种/器官：`Homo sapiens` peripheral blood/PBMC。
- CITE-seq：Atlanta cohort 12 age-matched subjects，其中 5 healthy controls、7 COVID-19 patients；DC-enriched PBMC；36 DNA-barcoded antibodies。
- CITE-seq 规模：初始 >63,000 cells，QC/annotation 后 57,669 high-quality transcriptomes。
- bulk RNA-seq：健康与 COVID-19 PBMC extended samples，用于验证 CITE-seq-derived ISG signature。

## 关键算法问题
1. 如何把 single-cell RNA、surface protein、bulk expression 和 soluble immune mediators 关联到 mild/severe clinical labels。
2. 如何从 CITE-seq 中提取 cell-type-specific interferon response，并验证到 bulk cohort。
3. 如何判断 COVID-19 severity 是 antiviral response、inflammatory cytokine 和 plasmablast/Tfh/monocyte state 的系统组合。
4. 如何在样本数较小的 CITE-seq 中避免过度解释 cell-level pseudo-replication。

## 论文中的算法贡献
### 1) CITE-seq as mechanistic layer in systems immunology
文章对 PBMC 做 CITE-seq，通过 UMAP、graph clustering、marker annotation 识别主要 immune populations，并比较 COVID-19 与 healthy 的 ISG expression。

**算法意义**：它不是提出 CITE-seq 方法本身，而是展示 RNA+ADT single-cell 数据如何嵌入更大的 systems biology design。

### 2) ISG signature transfer from CITE-seq to bulk RNA-seq
文章从 CITE-seq cell clusters 中提取 interferon-stimulated gene signature，然后在 bulk transcriptomics extended cohort 中验证，并用 PVCA 分析 covariates 对 signature variance 的解释。

**方法学价值**：
- `single-cell discovery -> bulk cohort validation` 是人群免疫研究中很实用的算法链。
- 可以避免只在小 CITE-seq cohort 中定义结论。

### 3) Severity-associated immune module interpretation
文章联合 soluble cytokines、antibody response、plasmablast/Tfh activation、monocyte/DC response 和 ISG signals，建立 mild vs severe COVID-19 的 systems-level immune profile。

**算法意义**：这类数据适合做 donor-level multi-view factor model 或 severity prediction，但本文主要采用统计比较与模块解释，没有形成统一预测模型。

## 不是它解决得很好的问题
1. CITE-seq cohort 只有 12 人，cell 数多但 donor 数有限。
2. 36 ADT panel 提供 targeted protein layer，但不是全蛋白组。
3. 没有 TCR/BCR single-cell receptor sequence 与 clone-state coupling。
4. systems integration 主要是分层解释，不是端到端 multi-omic model。

## 数据可用性评估
- DOI：https://doi.org/10.1126/science.abc6261
- CITE-seq GEO：`GSE155673`
- Bulk transcriptomics GEO：`GSE152418`
- BioProject/OmicsDI 聚合：`PRJNA655740`
- 可确认数据性质：human PBMC/CITE-seq from healthy and COVID-19 subjects; bulk PBMC transcriptomics and plasma immune measurements。
- 代码：文章公开页未定位到独立完整 GitHub；主要可通过 GEO 和 supplement 复现分析对象。
- 复用性：中高。数据 accession 明确，但代码复现性不如有完整 repository 的条目。

## 代码/模型结构
- 输入：CITE-seq RNA count matrix、ADT count matrix、bulk RNA-seq expression、cytokines/serology、clinical severity labels。
- 核心流程：`CITE-seq QC -> UMAP/clustering/annotation -> ISG module extraction -> cell-type-level comparison -> bulk RNA validation -> covariate variance analysis -> systems interpretation`
- 输出：cell-state annotations、CITE-seq ISG signature、bulk validation heatmaps/statistics、severity-associated immune modules。
- 模型意义：构成 `single-cell discovery + cohort-level validation` 的标准工作流。

## 对新算法贡献程度评估
- 定义任务价值：高
- 数据资源价值：中高
- 直接算法创新：低到中
- 对后续方法启发：高

综合评估：**systems-immunology resource paper；算法贡献主要是跨模态/跨尺度分析范式，而非新算法。**

## 可发展的新算法空间
### A. Donor-level multi-view severity model
把 CITE-seq cell composition、RNA module、ADT protein、bulk RNA 和 cytokines 用 multi-view latent factors 统一建模。

### B. Small-donor CITE-seq hierarchical inference
针对 donor 少、cell 多的设计，显式区分 cell-level variance 和 donor-level evidence。

### C. Signature transportability model
学习 single-cell-derived signatures 在 bulk、flow、clinical lab assays 之间的可迁移性。

## 适合纳入 method report 的表述
这篇文章展示了单细胞 CITE-seq 在人群免疫研究中最稳妥的使用方式之一：先用单细胞解析机制，再通过 bulk/serology/cytokine cohort 验证 donor-level 结论。它也说明现有研究仍缺少把这些模态端到端合并的 donor-aware severity model。
