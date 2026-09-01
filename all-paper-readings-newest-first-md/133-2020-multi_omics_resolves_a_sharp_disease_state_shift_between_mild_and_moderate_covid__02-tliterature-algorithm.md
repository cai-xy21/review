# Algorithm Report 074

## Paper
Multi-Omics Resolves a Sharp Disease-State Shift between Mild and Moderate COVID-19

## 算法视角定位
这篇文章是 COVID-19 multi-omics disease-severity mapping 的代表。它的强项不是提出新算法，而是构建了一个覆盖临床、plasma proteomics/metabolomics、single-cell RNA/CITE-seq、TCR-seq、single-cell secretome 和 EHR 的 longitudinal multi-omics cohort，并提出 mild-to-moderate 之间存在 sharp disease-state shift。

## 数据模态与样本设计
- 模态：clinical labs/EHR、plasma proteomics、metabolomics、PBMC scRNA-seq/CITE-seq、single-cell TCR-seq、single-cell secretome。
- 物种/器官：`Homo sapiens` peripheral blood/PBMC/plasma。
- 样本：139 COVID-19 patients，覆盖 mild/moderate/severe；258 healthy controls 用于不同模态匹配；许多患者在诊断后一周内 serial blood draws。
- 单细胞：blood scRNA-seq 数据在 ArrayExpress `E-MTAB-9357`。
- 额外数据：metabolomic/proteomic supplemental items in Mendeley Data。

## 关键算法问题
1. 如何在多模态 longitudinal cohort 中定义 disease severity trajectory。
2. 如何把 patient-level WOS severity 与 cell-level states、TCR sharing、plasma molecules 和 clinical labs 对齐。
3. 如何识别 mild-to-moderate transition，而不是只比较 healthy vs severe。
4. 如何避免不同模态缺失、不同采样时间和不同健康对照集合带来的偏差。

## 论文中的算法贡献
### 1) Severity as ordinal/trajectory variable
文章使用 WHO Ordinal Scale (WOS) 表示采样时疾病严重程度，并支持两种分析视角：
- 每次 blood draw 作为当前 severity state。
- 每个 patient 的 T1/T2 作为 disease trajectory。

**算法意义**：把 COVID-19 从 case-control 问题扩展为 severity state transition problem。

### 2) Cross-omic stepwise transition analysis
文章比较 healthy-to-mild、mild-to-moderate、moderate-to-severe 的 metabolites/proteins/cell states，发现 mild-to-moderate 之间存在 major shift，伴随 inflammatory cytokines 上升、lipid/amino-acid/xenobiotic metabolism 下降。

**方法学价值**：提示 disease progression 的关键不是线性加重，而可能是 nonlinear regime shift。对算法来说可转化为 change-point detection / disease phase transition modeling。

### 3) Single-cell immune state integration
文章在 PBMC 单细胞层识别 activated adaptive cells、proliferative/exhausted CD4 T cells、clonally expanded cytotoxic CD4 T cells 等随 severity 变化的状态，并结合 TCR sharing patterns 解释不同 fate。

**算法意义**：把 T-cell state, clonal sharing 和 clinical severity 放在同一疾病轴上，但仍主要是统计整合而非联合生成模型。

### 4) Network/correlation integration
文章进行 inter-omic Spearman correlation、clinical covariate adjustment、probabilistic PCA、module/severity correlation 等分析。

**局限**：这类相关网络适合发现 candidate modules，但难以区分因果、治疗影响和病程阶段。

## 不是它解决得很好的问题
1. 多模态缺失和不同健康对照集合可能影响跨模态可比性。
2. 细胞状态和 plasma omics 之间主要是 correlation/integration，不是因果模型。
3. TCR 信息被用于解释 clone sharing，但没有与 RNA/protein/severity 端到端联合。
4. 临床治疗、时间、基础疾病会与 severity confound。

## 数据可用性评估
- DOI：https://doi.org/10.1016/j.cell.2020.10.037
- scRNA-seq：ArrayExpress `E-MTAB-9357`
- Proteomics/metabolomics supplemental：Mendeley Data <https://doi.org/10.17632/tzydswhhb5.5>
- 可确认数据性质：human blood/PBMC/plasma；139 COVID-19 patients with serial draws; healthy matched controls across modalities。
- 代码：未在可见 Data and Code Availability 中定位到独立完整 GitHub；主要数据与 supplement 可公开获取。
- 复用性：高用于 multi-omics severity benchmark；完整重跑整合分析需较多 supplement/metadata 对齐。

## 代码/模型结构
- 输入：EHR/clinical labs、WOS labels、plasma protein/metabolite abundance、single-cell RNA/ADT/TCR/secretome profiles。
- 核心流程：`modality-specific QC -> severity-aligned differential analysis -> PCA/module detection -> cross-omic correlation network -> single-cell state/TCR interpretation -> disease-shift synthesis`
- 输出：severity-associated molecules/states、mild-to-moderate transition modules、single-cell immune subpopulations、cross-omic network features。
- 模型意义：定义了 infection severity phase-transition benchmark。

## 对新算法贡献程度评估
- 定义任务价值：很高
- 数据资源价值：很高
- 直接算法创新：中
- 对后续方法启发：很高

综合评估：**高价值 multi-omics cohort；直接算法创新中等，但为 disease-state transition modeling 提供强任务场景。**

## 可发展的新算法空间
### A. Multi-omic disease change-point model
学习 mild-to-moderate 的非线性 phase transition，并输出每个模态对 transition 的贡献。

### B. Longitudinal donor-state model
将 serial draws 建成 hidden-state model，区分病程进展、治疗干预和个体基础免疫差异。

### C. TCR-state-severity joint model
把 clonal expansion/sharing 与 RNA/ADT state、severity outcome 联合建模。

### D. Missing-modality robust integration
针对真实 clinical multi-omics 数据中模态缺失和不均匀采样，开发 partial-observation representation learning。

## 适合纳入 method report 的表述
这篇文章把 COVID-19 免疫差异从二分类比较推进到 multi-omics disease-state shift。它提示我们，新算法不应只做细胞注释，而要能从 donor-level 多模态数据中识别非线性病程转折点。
