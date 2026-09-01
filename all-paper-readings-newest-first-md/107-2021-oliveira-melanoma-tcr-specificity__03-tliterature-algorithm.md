# Algorithm Report 058

## Paper
Phenotype, specificity and avidity of antitumour CD8 T cells in melanoma

## 算法视角定位
`058` 是 specificity-aware tumour T-cell modeling 的高价值 paper。它把 melanoma CD8 TIL 的 phenotype、exact TCR clonotype、functional antigen specificity、TCR avidity、blood persistence 和部分 CITE-seq context 接起来，适合说明仅靠 exhausted-like transcriptome 不能可靠区分 tumour-reactive 与 bystander cells。

## 题录与数据
- 年份：2021
- 期刊：Nature
- DOI：https://doi.org/10.1038/s41586-021-03704-y
- 物种/器官：`Homo sapiens`；melanoma CD8 TILs and serial peripheral blood
- discovery context：article figure text describes TCRs from four melanoma patients for blood/tumour specificity tracking
- modalities：scRNA-seq, scTCR-seq and CITE-seq; antigen reactivity/deorphanization; TCR avidity assays; bulk blood TCR dynamics
- dbGaP：`phs001451.v3.p1` (study ID 26121)
- analysis code：https://github.com/kstromhaug/oliveira-stromhaug-melanoma-tcrs-phenotypes

## 详细算法贡献
### 1. Phenotype-specificity linkage
- The paper links intratumoural CD8 expression states with experimentally supported TCR specificity。
- It contrasts melanoma-reactive exhausted states with viral/bystander non-exhausted memory-like states。

### 2. Avidity as extra supervision
- TCR recognition quality is quantified via avidity and related to target abundance and peptide-HLA binding context。
- This turns “antigen-specific” into richer labels: specificity category plus response strength。

### 3. Cross-compartment clone dynamics
- Blood persistence of TIL-derived clonotypes is related to intratumoural exhaustion and checkpoint-response context。
- It motivates dynamics models that combine tumour phenotype and blood repertoire observation.

## 代码专项
- Nature code statement lists the public analysis repo and standard tools including Seurat, Harmony, SingleR and Scanpy。
- Repository inputs include 10x TCR processing objects, per-patient clonotype grouping and single-cell TIL objects; files include `10x_TCR_processing.R`, patient processing Rmds and combined TIL analyses。
- Outputs include clonotype group assignments, combined TIL phenotype analyses, comparisons with prior Sade-Feldman/Yost references and manuscript statistics。
- Meaning: public code is study analysis code for phenotype/clonotype linkage, not a general antigen-prediction package.

## 对新算法贡献程度
- 直接 general algorithm：**中等偏低**
- specificity/avidity supervision value：**极高**
- public code/data value：**高**
- 综合判断：**P1 specificity-aware CD8 TIL benchmark**

## 数据可用性评估
- Nature data statement: scRNA-seq, scTCR-seq and CITE-seq via dbGaP `phs001451.v3.p1`
- Other data available from corresponding author on reasonable request
- Code repo linked above
- Controlled human data means raw reuse requires dbGaP access

## 新算法空间
1. Predict tumour specificity/avidity from sequence, phenotype and antigen context with calibrated uncertainty。
2. Distinguish bystander memory, tumour-reactive exhausted and tumour/control-cross-reactive cells。
3. Integrate tumour and blood clone persistence for longitudinal monitoring。
4. Learn from partial functional labels where most clonotypes remain untested。

## 最终判断
`058` is a strong benchmark for the gap between transcriptomic state and true antigen function. It should sit near `053` in the method report as a core motivation for specificity-aware algorithms.
