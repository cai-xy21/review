# 《Clonal kinetics and single-cell transcriptional profiling of CAR-T cells in patients undergoing CD19 CAR-T immunotherapy》精读

## 论文信息

- 作者：Andy Sheih、Valentin Voillet、L.-A. Hanafi 等
- 期刊：*Nature Communications*
- 年份：2020；11: 219
- DOI：10.1038/s41467-019-13880-1
- 原文：[Nature Communications](https://doi.org/10.1038/s41467-019-13880-1)
- PubMed：[PMID 31924795](https://pubmed.ncbi.nlm.nih.gov/31924795/)
- 全文：[PMCID PMC6954177](https://pmc.ncbi.nlm.nih.gov/articles/PMC6954177/)
- scRNA/scVDJ：[GEO GSE125881](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE125881)
- integration-site sequencing：[SRA PRJNA589633](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA589633)
- 代码：[GitHub](https://github.com/ValentinVoillet/CAR-T)

## 一句话结论

对 4 名患者的 62,167 个纵向 CD8+ CAR-T 单细胞做 5′ scRNA/scTCR 分析表明，输注产品中的转录异质性入体后逐渐收敛；含 CCL4、CD27、IFNG 与细胞毒程序的克隆更可能早期扩增并长期持续。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 患者 | 4（CLL 2、NHL 2） | 小队列但每人纵向配对 |
| 细胞 | 62,167 个高质量 CD8+ CAR-T | QC 前另剔除 2,340 cells |
| 时间点 | IP + early + late + very late | early 7–14 d；late 26–30 d；very late 83–112 d，实际患者日数略有差异 |
| scRNA/VDJ records | 32 GSM | 4 人 × 4 时间点 × GEX/VDJ 两类 |
| IP 状态 | 4 个转录 cluster | activation、cytotoxicity、mitochondrial、cell-cycle programs |
| TCR 克隆追踪 | 4 人有 scVDJ；重点克隆动力学详析 NHL-6/7 | TCR 是内源克隆条形码，不是 CAR integration site |
| processed matrix | 109.3 MB gzip CSV | GEO 另有 2.0 MB TAR；raw reads 走 SRA |

## 1. 研究要解决的问题

输注产品中哪些 CAR-T 克隆会在体内扩增并持续？入体后转录状态如何随抗原负荷和时间变化？能否用内源 TCR 将产品状态连接到后续克隆命运？

## 2. 方法框架

- 从 infusion product 和多个血液时间点分选 CD8+CD4−EGFRt+ CAR-T；
- 10x Chromium 5′ GEX + V(D)J enrichment；
- t-SNE、MAST、GSEA、聚类、pseudotime/RNA velocity；
- TCRβ repertoire sequencing 与 single-cell αβ TCR 对接；
- lentiviral integration-site sequencing 为独立克隆性证据。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

核心是**同一患者从产品到体内多个时间点的 CAR-T 转录组 + 内源 TCR**。GEX 描述状态，TCR 将不同时间点的细胞归为同一 clonotype，integration site 则从载体插入位点提供另一种克隆追踪。三者不可混为同一 barcode。

### 3.2 纵向图谱规模

| 层级 | 规模/组成 | 应如何理解 |
|---|---:|---|
| 患者 | CLL-1、CLL-2、NHL-6、NHL-7 | 每人一个 IP 和 3 个 post-infusion 时点 |
| 时间 | IP；早期；晚期；极晚期 | GEO 实际日数为 d12/d21、d28–38、d83–112 |
| cells | 62,167 | 经过 `<200 genes`、`>20% mito`、`>40,000 UMI` 等 QC |
| IP clusters | 4 | cluster 2 偏 cytotoxic；cluster 4 为 S/G2M cycling；其余由 activation/mitochondrial programs 区分 |
| GEO records | 16 GEX + 16 VDJ | 不能把 32 records 当成 32 个生物样本 |

所有患者输注后 transcriptome 相对 IP 改变：早期/晚期 PRF1、GZMB、GZMK 等效应程序上升，极晚期下降；MKI67 随靶抗原清除而降低。抑制性受体数量上升并不等同于一致的 exhaustion signature 增强。

### 3.3 克隆图谱组成

作者定义早期相对频率上升的 IRF clonotypes 与下降的 DRF clonotypes。NHL-6 中 29/29 IRF clones 在 late 仍被检测、very late 仍为 29/29；DRF 相应仅 15/67 与 6/67。NHL-7 中 IRF 为 19/19 与 10/19，DRF 为 3/59 与 2/59。IRF 在 IP 时已富集 CCL4、CD27、IFNG 和 cytotoxic genes。

这些数字是“被采样到的 clonotype 持续性”，不是所有输注克隆的真实存活率；外周采血会漏采低频 clone。

### 3.4 GEO 文件组成

`GSE125881` 包含 32 个 records：每个患者/时间点分别有 `GE` 与 `VDJ`。主要下载项：

- `GSE125881_raw.expMatrix.csv.gz`：109.3 MB，合并表达矩阵；
- `GSE125881_RAW.tar`：2.0 MB，逐样本 CSV；
- SRA `SRP182902` / BioProject `PRJNA517816`：原始 GEX/VDJ reads；
- 论文 Source Data：TCR-seq 与图表底层数据；
- `PRJNA589633`：integration-site sequencing，独立于 GEO scRNA。

### 3.5 如何获取

#### 路线 A：快速复现表达图谱

下载 109.3 MB expression CSV 与 GEO sample metadata；把 patient、timepoint、GEX/VDJ 类型映射到 cell barcode。表达 CSV 本身未必包含全部临床/TCR 字段。

#### 路线 B：克隆—状态联合分析

下载 GEX 与对应 VDJ records，使用 Cell Ranger 输出的 contig annotation 建 αβ clonotype，再与同一 cell barcode 的 GEX 合并。建议保留 chain completeness、productive status 和 multiple-chain flags。

#### 路线 C：严格重跑

从 SRA Run Selector 获取 GEX/VDJ FASTQ；integration-site reads 从 PRJNA589633 单独下载。代码仓库用于复现分析逻辑。

### 3.6 下载后先做什么

先检查 barcode 是否带 sample suffix，并给每个 barcode 加 `patient_timepoint` 前缀；再核对 TCR chain pairing。统计克隆频率时按每个样本总 CAR-T 数归一化，并对 sampling depth 做敏感性分析。

## 4. 状态与时间变化

输注产品异质性最高，入体后状态收敛到激活/效应程序，随后随靶抗原减少而降低。IP 的 cluster 2 越来越富集于后期仍持续的 clonotypes，提示产品中早期细胞毒/激活准备状态可预示体内命运。

## 5. TCR 能说明什么

同一 αβ TCR 跨时间支持克隆连续性；它不能证明某个转录状态单向转化为另一个状态，也不能给出 CAR 插入位点。integration-site 数据是补充但并非每个 scRNA cell 一一对应。

## 6. 推荐图版

- Fig. 5：IP 到 very-late 的转录变化；
- Fig. 6：IP 四个 cluster；
- Fig. 7：IRF/DRF 与克隆持续性，是本综述最有价值图。

## 7. 创新价值

1. 早期纵向联合 CAR-T scRNA 与 TCR 克隆追踪。
2. 把产品状态连接到数月后的体内克隆命运。
3. 显示“耗竭”不能仅由抑制受体共表达判定。

## 8. 局限性

1. 仅 4 人，克隆详析重点仅 2 名 NHL 患者。
2. 只分选 CD8+ CAR-T，缺乏 CD4/host immune context。
3. 血液采样不能代表组织分布。
4. 轨迹推断未得到清晰发育关系。

## 9. 对本章节的作用

这是“link cell state/function transitions with molecular drivers”的核心文献：利用内源 TCR 把同一克隆的产品状态和体内命运相连，为制造阶段的状态富集提供直接线索。

## 10. 可直接用于综述的观点

> 纵向 scRNA/scTCR 显示，CAR-T 输注产品中的 CCL4/CD27/IFNG 与细胞毒准备程序富集于随后扩增并长期持续的克隆，说明制造产品的单细胞状态可用于预测而非仅事后描述体内命运（Nature Communications 2020, Sheih）。

## 11. 避免误读

- 不要把 32 GSM 写成 32 名患者。
- 不要把内源 TCR 与 CAR integration site 混为一谈。
- 不要把“未在血中采到”直接解释为克隆消失。
- 不要由 pseudotime/UMAP 推断确定方向。

