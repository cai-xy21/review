# 《Multimodal Analysis of Composition and Spatial Architecture in Human Squamous Cell Carcinoma》精读

## 论文信息

- 作者：Andrew L. Ji、Adam J. Rubin、Kristofer Thrane 等
- 期刊：*Cell*
- 年份：2020；182: 497–514.e22
- DOI：10.1016/j.cell.2020.05.039
- 原文：[Cell](https://doi.org/10.1016/j.cell.2020.05.039)
- PubMed：[PMID 32579974](https://pubmed.ncbi.nlm.nih.gov/32579974/)
- 全文：[PMCID PMC7391009](https://pmc.ncbi.nlm.nih.gov/articles/PMC7391009/)
- 数据总入口：[GEO GSE144240](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE144240)
- 代码：[GitHub](https://github.com/ajrubinlab/MultimodalAnalysis_scSCC)

## 一句话结论

作者联合 48,164 个单细胞转录组、17,064 个空间转录组 spots 与 55,832 个 MIBI 细胞，发现 cSCC 特异 keratinocyte 状态位于肿瘤前缘的 fibrovascular niche，并揭示 Treg 与 CD8 T cells 在肿瘤间质中的空间共定位。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 人体队列 | 10 个 cSCC 与部位匹配正常皮肤 | 同一患者的 tumor/normal 是配对设计 |
| scRNA-seq | 48,164 个细胞 | 10x 3′ v2；另含 xenograft/cell-line 数据 |
| 空间转录组 | 17,064 spots | 4 个患者的经典 ST；另有 2 个患者 Visium |
| MIBI | 55,832 个分割细胞 | 单细胞蛋白成像，不在 GEO 表达矩阵中 |
| 肿瘤角质细胞状态 | 4 类 | basal、cycling、differentiating、tumor-specific keratinocyte (TSK) |
| GEO SuperSeries | GSE144240，105 个 records | 含 scRNA、WES、CRISPR screen、ST 四个 SubSeries |
| 关键公开文件 | scRNA counts 127.2 MB；ST RAW 222.1 MB | 下载前先按 modality 选子系列 |

## 1. 研究要解决的问题

组织解离后的 scRNA-seq 能看细胞异质性，却丢失肿瘤前缘和细胞邻域；空间测序保留位置但 spot 混合多细胞；MIBI 可到单细胞蛋白层却 marker 有限。论文用三者互补，重建 cSCC 的细胞组成、空间生态位与通信网络。

## 2. 方法框架

- 10 对 cSCC/匹配正常皮肤做 10x scRNA-seq；
- 4 个患者做 array-based Spatial Transcriptomics，后补 2 个患者 Visium；
- MIBI 多重离子束成像量化肿瘤、免疫与基质细胞邻域；
- WES 辅助识别肿瘤细胞；
- ligand–receptor 分析连接 TSK 与 fibroblast/endothelial/immune cells；
- xenograft 与体内 CRISPR screen 验证 TSK 富集基因的肿瘤依赖性。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这是一个由**配对人组织单细胞表达、空间表达、空间蛋白成像、肿瘤外显子、异种移植单细胞和 CRISPR 功能筛选**组成的多模态资源。不同模态并非都来自同一切片或同一细胞，整合是跨样本/跨分辨率推断。

### 3.2 人体图谱规模

| 层级 | 规模/组成 | 应如何理解 |
|---|---:|---|
| 患者 | 10 个 cSCC，配对 site-matched normal skin | 配对关系是主要临床设计 |
| scRNA | 48,164 transcriptomes | 涵盖 malignant/normal keratinocytes、T/B/NK、myeloid、fibroblast、endothelial 等 |
| ST | 17,064 spots | spot 级混合表达，用 scRNA signature 做定位/解释 |
| MIBI | 55,832 segmented cells | 多标记蛋白图像，用于单细胞邻域和共定位 |
| 角质细胞状态 | 4 个肿瘤亚群 | 三个重现正常表皮程序，一个为 TSK |

免疫空间结果的重点不是建立细粒度 T-cell atlas，而是观察 Treg 与 CD8 T cells 在区室化肿瘤间质中的共定位，以及免疫抑制信号与肿瘤前缘生态位的关系。

### 3.3 GEO 图谱组成

`GSE144240` 是总入口，包含 105 个 GEO sample records 与四个子系列：

| accession | records/内容 | 关键文件与用途 |
|---|---|---|
| `GSE144236` | 29 个 scRNA records；人 cSCC/normal + xenograft/cell line | `cSCC_counts.txt.gz` 127.2 MB；`patient_metadata_new.txt.gz` 648.8 KB；另有 CAL27/SCC13/XG_TME counts |
| `GSE144237` | 配对 cSCC/normal WES | 肿瘤变异和拷贝数辅助信息 |
| `GSE144238` | A431/CAL27/SCC13 in vivo CRISPR screen | TSK/整合素网络功能验证 |
| `GSE144239` | 16 个 ST records | `RAW.tar` 222.1 MB；Visium counts 10.5 MB；all counts 9.0 MB；barcode map 16.9 KB |

GSE144236 页面特别提示：旧版 `patient_metadata.txt.gz` 的 barcode 名称被打乱，必须使用更新的 `patient_metadata_new.txt.gz`。这是复用时的关键数据质量事项。

### 3.4 空间数据的样本组成

经典 ST 来自 P2、P5、P9、P10，各 3 个 replicate；后续 Visium records 来自 P4 与 P6，各 2 个 replicate。GSE144239 RAW TAR 含 CSV/JPG/JSON/MTX/PNG/TSV，可同时恢复表达矩阵、坐标、组织图像与 scale factors。

### 3.5 如何获取

#### 路线 A：只做单细胞重分析

从[GSE144236](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE144236)下载：

1. `GSE144236_cSCC_counts.txt.gz`；
2. `GSE144236_patient_metadata_new.txt.gz`；
3. 如需模型验证再下载 CAL27、SCC13 与 xenograft TME counts。

原始 reads 从 PRJNA603103 / SRP244706 的 SRA Run Selector 获取。

#### 路线 B：只做空间分析

从[GSE144239](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE144239)下载 RAW TAR。经典 ST 可用 `ST_all_counts` + `barcode_id`；Visium 优先直接使用 TAR 内 Space Ranger 风格的 matrix、spatial image 与 JSON。

#### 路线 C：完整多模态复现

按 GSE144240 的四个 SubSeries 分别下载；MIBI 图像/分割数据与 source data 需同时检查论文补充材料和代码仓库，因为 GEO SuperSeries 主体聚焦测序数据。

### 3.6 下载后先做什么

1. 用新 metadata 核对每个 barcode 的 patient、tumor/normal 与 replicate；
2. 绝不能只按 barcode 合并不同 10x runs，应加 sample 前缀；
3. ST 中区分经典 ST 与 Visium，两者 spot 尺寸/捕获流程不同；
4. 跨模态整合时保留 modality 和 patient 层级，避免把同一患者多个 spots 当成独立患者。

## 4. 主要图谱发现

TSK 细胞表达特异角质细胞程序，富集在侵袭前缘并与 fibroblast、endothelial cells 邻接，形成 fibrovascular niche。TSK 还是 ligand–receptor 网络的通信枢纽。肿瘤间质中出现 Treg–CD8 邻域和其他免疫抑制特征。

## 5. 与 T 细胞状态导航的关系

论文说明只知道 CD8/Treg 转录状态不够：同一细胞状态所处的前缘、间质区室和邻近细胞决定其可接触信号。未来 CAR-T/TIL 导航应把空间可达性与局部基质/血管信号纳入优化变量。

## 6. 推荐图版

- Fig. 1：多模态设计与 48,164-cell atlas；
- Fig. 3–4：TSK 的空间定位和 fibrovascular niche；
- Fig. 5：MIBI 免疫空间结构，适合 Treg–CD8 共定位；
- CRISPR 图：从状态关联走向功能依赖。

## 7. 创新价值

1. 在同一 cSCC 队列整合 scRNA、ST 和 MIBI。
2. 将肿瘤细胞状态放回侵袭前缘生态位。
3. 用 xenograft/CRISPR screen 验证图谱发现的功能网络。

## 8. 局限性

1. 空间样本少于 scRNA 队列，并非每位患者都有全部模态。
2. 经典 ST spot 非单细胞分辨率。
3. MIBI 受限于预选抗体 panel。
4. T 细胞不是本文最深度解析的谱系，缺乏 scTCR。

## 9. 对本章节的作用

该文适合“charting molecular landscape by single-cell and spatial omics”，强调状态必须与位置和邻域配对；也可作为“molecular drivers”章节的例子，因为空间通信网络随后由 CRISPR 做功能验证。

## 10. 可直接用于综述的观点

> cSCC 的单细胞状态只有在空间坐标中才显示其功能组织：TSK 细胞定位于侵袭前缘 fibrovascular niche，而 Treg 与 CD8 T cells 在肿瘤间质形成特定邻域，说明细胞治疗优化需同时导航状态与空间可达性（Cell 2020, Ji）。

## 11. 避免误读

- 不要把 17,064 spots 写成 17,064 个单细胞。
- 不要使用旧的 scrambled barcode metadata。
- 不要把所有 10 位患者写成都有 ST/MIBI。
- 不要把空间共定位直接解释为抑制因果。

