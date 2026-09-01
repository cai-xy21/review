# 《PD-1-cis IL-2R agonism yields better effectors from stem-like CD8+ T cells》精读

## 论文信息

- **作者**：Laura Codarri Deak、Valeria Nicolini、Masao Hashimoto 等
- **期刊与年份**：*Nature*, 2022；610: 161–172
- **DOI**：10.1038/s41586-022-05192-0
- **本地原文**：[PDF](<D:/research/review/perturbation33references/23-PD-1-cis IL-2R agonism yields better effectors from stem-like CD8+ T cells.pdf>)
- **单细胞多组学数据**：[ArrayExpress/BioStudies E-MTAB-11773](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-11773)
- **慢性感染 bulk RNA-seq**：[GEO GSE208556](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE208556)
- **代码**：[GitHub](https://github.com/bedapub/PD1-IL2v_in-vivo_TILs_Panc02_publication)

## 一句话结论

把无 CD25 结合能力的 IL-2 变体通过抗 PD-1 臂定向到同一 PD-1⁺ T 细胞，可在肿瘤和慢性感染中把 TCF1⁺ 干样 CD8⁺ T 细胞导向细胞毒性更强、耗竭程度较低的“better effector”状态，同时减少对 Treg 的非选择性刺激。

## 数据护照

| 模块 | 规模/组成 | 获取位置 |
|---|---|---|
| Panc02 单细胞多组学 | 20 个小鼠肿瘤样本，5 个处理组，每组 4 个；GEX + TCR + 11-plex CITE-seq，共 60 assays | E-MTAB-11773 / ENA ERP138186 |
| 实际整合分析 | 两批各 10 个样本；因总体质量低和细胞数过低排除 2 个样本，余 18 个进入整合 | E-MTAB-11773 protocol/metadata |
| 处理组 | vehicle、muFAP-IL2v、muPD-1、muPD-1 + muFAP-IL2v、muPD1-IL2v；各 4 | SDRF / sample metadata |
| 慢性 LCMV bulk RNA-seq | 20 个文库；不同治疗/细胞状态组合 | GSE208556 / PRJNA860101 |
| 处理后矩阵 | `.h5ad` 约 2.04 GB；Matrix Market 矩阵约 2.05 GB；逐细胞 metadata 约 28.2 MB | E-MTAB-11773 Files |

## 1. 研究问题

PD-1 阻断的持续疗效依赖 PD-1⁺TCF1⁺ 干样 CD8⁺ T 细胞扩增并生成效应后代。常规 IL-2 或偏向 IL-2Rβγ 的 IL-2 变体既难以选择性作用于这群细胞，又可能刺激高表达 CD25 的 Treg。作者因此问：能否利用 PD-1 本身作为“停靠位点”，让 IL-2R 激动只发生在同一 PD-1⁺ T 细胞的顺式空间中？

## 2. 实验设计与方法框架

作者构建 PD1-IL2v：抗 PD-1 抗体融合不结合 CD25、偏向 IL-2Rβγ 的 IL-2 变体。研究依次验证顺式信号、Treg 竞争、Panc02 和 RIP-Tag5 等肿瘤模型、慢性 LCMV，并用 scRNA-seq、TCR-seq、CITE-seq 和 bulk RNA-seq刻画细胞状态。

核心比较不是“有无 IL-2”而是：同一 IL-2v 由 PD-1 顺式靶向，还是由 FAP 等肿瘤基质靶向/游离给药。这样才能把受体空间几何与一般药物暴露区分开。

## 3. 数据规模与图谱组成

### 3.1 E-MTAB-11773 到底是什么

这是 Panc02-Fluc 皮下胰腺肿瘤模型的肿瘤浸润 CD8⁺ T 细胞单细胞多组学数据。20 个独立小鼠样本分成两批处理，每批 10 个。每个生物样本同时建立：

1. 10x 5′ 单细胞基因表达文库；
2. 10x TCR 富集文库，用 CDR3 序列定义克隆型；
3. feature barcode/CITE-seq 文库，抗体面板包括 PD-1、TIM-3、LAG-3、TIGIT、CD127、CD44、CD62L、CXCR3、CXCR5、CD28、CD39 等 11 个表面蛋白。

因此页面中的 **60 assays = 20 个生物样本 × 3 类文库**，不是 60 只小鼠。每个样本上机前计划装载 10,000 个活细胞；GEX 目标约 20,000 reads/cell，TCR 与 feature barcode 各约 5,000 reads/cell。

### 3.2 处理组与分析纳入情况

| 处理 | 剂量 | 生物样本数 |
|---|---:|---:|
| Vehicle | — | 4 |
| muFAP-IL2v | 1.5 mg/kg | 4 |
| muPD-1 | 10 mg/kg | 4 |
| muPD-1 + muFAP-IL2v | 10 + 1.5 mg/kg | 4 |
| muPD1-IL2v | 0.5 mg/kg | 4 |
| **合计** |  | **20** |

数据库 protocol 明确记录：一个样本因总体质量低、另一个因细胞数极低而排除，故论文整合图谱来自 **18 个合格样本**。不要把样本页面上的 20 与最终分析样本数混用。

统一图谱将 CD8⁺ TIL 分为 naive、stem-like、memory/effector、better effector/cytotoxic 和 exhausted 等主要状态，并把转录状态与表面蛋白、克隆扩增连接起来。数据库没有在首页给出最终过滤后总细胞数，引用时应优先报告样本和 assay 数，而不要从“每样本计划装载 10,000”反推 20 万个有效细胞。

### 3.3 公开文件组成

| 文件 | 大小 | 用途 |
|---|---:|---|
| `sw_besca_24_final.annotated.h5ad` | 约 2.04 GB | 注释后的 AnnData，复用首选 |
| `matrix.mtx` | 约 2.05 GB | 稀疏表达矩阵 |
| `metadata_ext.tsv` | 约 28.2 MB | 逐细胞样本、状态、蛋白/TCR等元数据 |
| `barcodes.tsv` | 约 3.6 MB | 细胞条形码 |
| `genes.tsv` | 约 1.38 MB | 基因列表 |
| `PD1IL2V_ablist_cellranger.csv` | 约 1.1 KB | CITE-seq 抗体标签定义 |
| `E-MTAB-11773.sdrf.txt` | 约 193 KB | 样本—assay—原始文件映射 |

原始 reads 由 ENA study `ERP138186` 托管。BioStudies 的 SDRF 是批量下载时最重要的索引：先用它把 60 个 assays 还原到 20 个生物样本，再进入 ENA 获取 FASTQ。

### 3.4 GSE208556：20 个慢性 LCMV bulk RNA-seq 文库

GEO 页面列出 20 个小鼠文库，使用 NovaSeq 6000。样本包括 naive、untreated、抗 PD-L1、PD1-IL2v 以及联合治疗等组别；提供 `GSE208556_combined_counts.csv.gz`（约 1.2 MB），原始 reads 在 SRA/BioProject `PRJNA860101`。

这 20 个 bulk 文库与 Panc02 单细胞 20 个样本不是同一批材料：前者是慢性病毒感染中的分选 CD8⁺ T 细胞，后者是肿瘤 TIL 多组学。

### 3.5 推荐下载方式

1. 快速复用：从 BioStudies 下载 `.h5ad`、`metadata_ext.tsv` 和抗体列表。
2. 重新构建表达矩阵：下载 `matrix.mtx`、`barcodes.tsv`、`genes.tsv`。
3. 重跑 Cell Ranger：先下载 SDRF，再按 ENA run accession 批量下载 GEX、VDJ 与 feature-barcode FASTQ。
4. 复现慢性感染转录分析：先下载 GEO count matrix；只有需要重比对时再进入 SRA。

下载后首先核对 `.obs` 中的 sample、batch、treatment、cell type、TCR clone 字段，并排除数据库 protocol 指出的两个低质样本，避免把归档样本直接当成分析样本。

## 4. 主要结果

PD1-IL2v 在同一 PD-1⁺ T 细胞上形成 PD-1/IL-2Rβγ 顺式桥接，增强 STAT5 信号。它扩增 TCF1⁺ 干样细胞及其细胞毒性后代，并减少终末耗竭构成；在多个肿瘤模型和慢性 LCMV 中优于非靶向 IL-2v 或简单联合给药。

## 5. 机制理解

PD-1 在这里既是抑制受体，也是药物定位标签。通过空间共定位，IL-2v 在局部高有效浓度下激活 IL-2Rβγ；由于不依赖 CD25，可避开 Treg 的受体优势。关键并非持续保留所有 TCF1⁺ 细胞，而是让其产生功能更好的效应后代。

## 6. 推荐重点阅读的图

- 顺式/反式 STAT5 实验：直接证明药物的空间机制。
- Panc02 单细胞 UMAP、状态比例和 TCR 克隆扩增图：连接治疗、状态和克隆。
- 慢性 LCMV 的转录与表型比较：说明机制不局限于肿瘤。
- 多肿瘤模型疗效和生存图：展示转化广度。

## 7. 创新性

把检查点受体从单纯阻断靶点转化为细胞因子顺式递送锚点；并以 GEX + CITE-seq + TCR 三模态证明药物改变的是克隆扩增与分化路径，而非只增加总 T 细胞数。

## 8. 局限性

主要证据来自小鼠模型；不同模型的 PD-1 密度、抗原负荷和 Treg 结构不等同于人肿瘤。单细胞分析仅一个终止时间点，难以直接判定状态转换方向。两个低质样本被排除后各组是否仍完全平衡需从 metadata 重新核查。

## 9. 在综述中的定位

适合作为“受体空间工程重定向干样 CD8⁺ T 细胞命运”的代表，与常规 IL-2 mutein、IL-2/抗体复合物和细胞自主 cytokine circuit 对照。

## 10. 可直接写入综述的表述

> PD1-IL2v 通过在同一 PD-1⁺ T 细胞上顺式桥接 IL-2Rβγ，使 TCF1⁺ 干样 CD8⁺ T 细胞生成细胞毒性更强、耗竭程度较低的效应后代，并避免依赖 CD25 的 Treg 偏向扩增。

## 11. 数据复用建议

以 sample 而非 cell 为统计重复；同时建模 batch 和 treatment。可比较各状态的克隆扩增、克隆跨状态共享及 CITE-seq 蛋白—RNA一致性。对治疗组比例的检验应在每只小鼠层面汇总，不能把数万个细胞当独立重复。

## 12. 转化与安全性关注

需要评估 PD-1 高表达的非肿瘤抗原特异 T 细胞、慢性感染特异 T 细胞和外周活化 T 细胞是否被同步激活；同时监测 cytokine toxicity、血管内皮影响、过度克隆扩增及自身免疫。

## 13. 避免误读

- 60 assays 不是 60 个生物样本，而是 20 个样本的三种文库。
- 归档 20 个样本中有 2 个未进入整合分析。
- PD1-IL2v 促进“better effector”生成，不等于所有细胞长期保持干样。
- TCR 克隆共享支持谱系联系，但不能单独证明分化方向或抗原特异性。

