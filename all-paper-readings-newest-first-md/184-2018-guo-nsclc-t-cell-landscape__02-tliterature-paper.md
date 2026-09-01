# 050. Global characterization of T cells in non-small-cell lung cancer by single-cell sequencing

## 基本信息
- 年份：2018
- 期刊：Nature Medicine
- DOI：https://doi.org/10.1038/s41591-018-0045-3
- 主题：NSCLC T-cell atlas；TCR lineage tracking；pre-exhaustion；Treg heterogeneity；patient stratification

## 为什么保留
这篇把 NSCLC T-cell single-cell analysis 从 cluster 描述推进到 lineage context 和 prognosis association。对方法报告，它是“已有算法做到哪里”的好例子：expression + TCR lineage 已能提出 clinically relevant features，但还没有形成统一的 donor-aware outcome model。

## 数据与研究设计
- 供者：14 名 treatment-naive NSCLC patients
- 细胞：12,346 个 T cells
- 组织：tumor、adjacent normal lung tissue、peripheral blood
- 主要 compartments：CD8 T cells、CD4 helper-like cells、CD4 regulatory T cells
- 主技术：deep single-cell RNA-seq；配套 TCR typing/lineage context

## 主要贡献
1. 构建 NSCLC tumor-related T-cell landscape。
2. 把 TCR lineage context 与 expression states 联合解释 inter-tissue effector cells。
3. 区分 pre-exhausted-like 与 exhausted CD8 tumor states，并将其比例与 LUAD prognosis 联系。
4. 发现 tumor Treg 内 activated state heterogeneity，并提出包含 IL1R2 的 signature。

## 与 T 细胞和人群免疫力的关系
- 直接研究 T-cell functional state、lineage and tumor adaptation。
- tumor、adjacent tissue 与 blood 的设计有助于分开 systemic immunity 与 local tumor remodeling。
- signature-to-prognosis 说明 cell-level state 可以向 patient-level readout 过渡。

## 算法与分析视角
- expression clustering and signature extraction 定义 cell states。
- TCR-based lineage tracking 提供 clone/history context。
- independent survival cohort analysis 将 single-cell derived state ratios/signatures 接到临床相关 readout。
- 主要仍是分析管线与派生特征，并非一个公开模型包。

## 数据可用性
- GEO：https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE99254
- GEO accession：`GSE99254`
- BioProject：`PRJNA387726`
- raw EGA accession：`EGAS00001002430`
- GEO processed files：12,346-cell count and TPM matrices；11,769-cell centered matrix
- Nature supplement：patient metadata、single-cell TCR typing、cluster signatures、exhaustion/Treg gene tables
- 作者代码：本轮未定位到专用仓库

## 对新算法开发的启发
1. lineage-aware patient scoring
2. cross-tissue TCR migration/state model
3. antigen-specific tumor T-cell representation
4. uncertainty-aware transfer of exhaustion axes across tumors

## 可信度与边界
- 可信度：高
- 强项：T-cell focused、14 patients、processed/raw accessions、TCR and prognosis context
- 边界：code gap；patient-level predictor not fully modeled；antigen specificity not directly resolved

## 一句话结论
`050` 是 NSCLC tumor T-cell state-lineage-prognosis 连接的代表文献，也是后续 donor-aware clone-state algorithm 很好的动机来源。
