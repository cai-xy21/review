# 049. Landscape of Infiltrating T Cells in Liver Cancer Revealed by Single-Cell Sequencing

## 基本信息
- 年份：2017
- 期刊：Cell
- DOI：https://doi.org/10.1016/j.cell.2017.05.035
- 主题：HCC TIL atlas；T-cell trajectory；exhaustion；Treg programs；TCR reconstruction

## 为什么保留
这是早期 human tumor T-cell single-cell 经典文献。它把血液、肿瘤与邻近正常组织中的 T cells 放在同一表达与 TCR context 中比较，是后续 exhaustion、clonality 和 tumor adaptation 建模的基础场景。

## 数据与研究设计
- 供者：6 名 hepatocellular carcinoma patients
- 细胞：5,063 个 single T cells
- 组织：peripheral blood、tumor、adjacent normal liver tissue，GEO 还记录 joint-area sample categories
- T-cell compartments：CD8 T cells、CD4 helper-like cells、CD4 CD25high regulatory T cells
- 平台：Smart-seq2 为主；一个 patient sample 使用 Tang2010 protocol

## 主要贡献
1. 提供 HCC infiltrating T-cell state landscape。
2. 揭示 tumor CD8 dysfunctional/exhausted programs 与 Treg-specific programs。
3. 用 trajectory analysis 表达 CD8/CD4 T-cell state continua。
4. 从 single-cell RNA reads 中恢复 TCR context，形成早期 clone-state coupling resource。

## 算法与分析视角
- transcriptome clustering 与 marker/program extraction 定义 T-cell states。
- trajectory analysis 以 Monocle 2.0 为主，补充材料对比了其他 trajectory tools。
- TraCeR 从 full-length scRNA reads 重建 TCR，连接 clone relation 与 state annotation。
- 没有端到端 sequence-state model，也没有单独作者模型仓库。

## 数据可用性
- GEO：https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE98638
- GEO accession：`GSE98638`
- BioProject：`PRJNA385799`
- raw EGA accession：`EGAS00001002072`
- GEO processed files：5,063-cell count and TPM matrices、centered matrices、cell metadata
- 作者代码：本轮未定位到专用仓库

## 对新算法开发的启发
1. clone-aware tumor T-cell trajectory
2. blood-normal-tumor domain disentanglement
3. trajectory topology uncertainty
4. donor-aware transfer of tumor signatures

## 可信度与边界
- 可信度：高
- 强项：T-cell focused、processed and raw accessions、trajectory and TCR context
- 边界：6 donors；早期 full-length workflow；clone information mostly post hoc

## 一句话结论
`049` 把 tumor T-cell atlas、trajectory 和 TCR context 连到一起，是后续 clone-aware exhaustion algorithms 的早期起点。
