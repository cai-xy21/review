# 《Changes in Bone Marrow Tumor and Immune Cells Correlate with Durability of Remissions Following BCMA CAR T Therapy in Myeloma》精读

## 论文信息

- 作者：Kavita M. Dhodapkar、Adam D. Cohen、Aditi Kaushal 等
- 期刊：*Blood Cancer Discovery*
- 年份：2022；3: 490–501
- DOI：[10.1158/2643-3230.BCD-22-0018](https://doi.org/10.1158/2643-3230.BCD-22-0018)
- PubMed：[PMID 36026513](https://pubmed.ncbi.nlm.nih.gov/36026513/)；[PMC9627239](https://pmc.ncbi.nlm.nih.gov/articles/PMC9627239/)
- 数据：[GEO GSE210079](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE210079)；BioProject `PRJNA863621`

## 一句话结论

11 名多发性骨髓瘤患者的骨髓多组学显示，BCMA-CAR T 缓解持久性不仅取决于肿瘤清除，还与治疗前 TCR 多样性、TCF1⁺记忆样 T 细胞、CLEC9A⁺树突细胞及 BAFF⁺PD-L1⁺髓系生态相关。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 患者 | 11 名 RRMM | 长 PFS n=4，短 PFS n=7 |
| 骨髓样本 | 28 份用于高维分析 | 不同 assay/时间点可用数不同 |
| GEO 样本记录 | 24 | pre、day 28、3 月、>3 月的单细胞多组学子集 |
| 单细胞 | 约 72,467 个 | 公共矩阵的常用索引统计；复分析应从 H5 实算 |
| 模态 | 10x GEX + 55-ADT + TCR | 骨髓全生态，不只 CAR⁺ T |
| 聚类 | 63 clusters | 含肿瘤、T/NK、髓系/DC、B 和祖细胞 |
| GEO 处理后包 | `GSE210079_RAW.tar`，约 1.0 GB | H5 文件；原始 reads 在 SRA |

## 1. 研究要解决的问题

BCMA-CAR T 可快速降低骨髓肿瘤负荷，但为何部分患者缓解短暂？作者把治疗前后肿瘤、T 细胞和髓系微环境放在同一骨髓单细胞/CyTOF/免疫组库框架中，寻找与 PFS 持久性相关的生态特征。

## 2. 方法框架

- 依据 PFS <180 天与 >180 天分为短、长缓解组；
- 多时间点骨髓采样；
- 10x 单细胞 GEX + 55 个 ADT + TCR；
- 高维流式/质谱流式与肿瘤负荷评估；
- 比较肿瘤清除、TCR 克隆结构、T/NK 状态和髓系/DC 构成。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

本数据集的分析单位是**骨髓生态系统**，而不是只分选 CAR⁺ T：

1. **肿瘤层**：浆细胞/骨髓瘤细胞比例与残留；
2. **T/NK 层**：CD4/CD8、TCF1⁺记忆样、耗竭与克隆扩增；
3. **髓系/DC 层**：单核/巨噬细胞、CLEC9A⁺ DC、BAFF/PD-L1 程序；
4. **B/祖细胞层**：治疗后骨髓重建背景；
5. **时间层**：治疗前、约 day 28、3 个月及更晚；
6. **临床层**：PFS 长短和肿瘤响应。

### 3.2 多大规模、图谱由什么组成

| 层级 | 规模/内容 | 应如何理解 |
|---|---:|---|
| 患者 | 11 | RRMM，长 PFS 4、短 PFS 7 |
| 高维骨髓标本 | 28 | 跨患者/时间点；不是 28 人 |
| GEO 单细胞样本 | 24 | 每个 accession 对应一个骨髓样本时间点 |
| 单细胞规模 | 约 72,467 | 第三方重分析索引对 GSE210079 的统计；下载后应以矩阵 barcode 数复核 |
| 细胞簇 | 63 | 进一步归入肿瘤、T、NK、髓系/DC、B、祖细胞等大类 |
| ADT | 55 个表面蛋白 | 与转录和 TCR 在同一 barcode 关联 |

论文报告 day 28 左右肿瘤细胞平均比例由治疗前约 35% 降至约 0.5%，但两组后续免疫生态不同。状态频率比较应使用每位患者的比例或 pseudobulk，而不是把细胞当独立重复。

### 3.3 GEO GSE210079 有什么

- 24 个 sample accessions；
- BioProject：`PRJNA863621`；
- 10x gene expression、55-plex antibody-derived tags 和 TCR；
- 测序平台：NovaSeq 6000；
- 处理后合集：`GSE210079_RAW.tar`，约 **1.0 GB**，内部为样本级 H5；
- 原始 reads：SRA，24 个 experiments；
- BioProject 当前汇总约 **0.40 TB / 1,463 Gb** 原始序列，而 GEO supplementary 约 **1,025 MB**。

0.40 TB 与 1.0 GB 分别代表原始读段与过滤后矩阵，两者不冲突。H5 是否同时含 Gene Expression 与 Antibody Capture feature，应以 `feature_type` 字段核查；TCR contig 文件需从相应 VDJ/SRA 或补充对象确认。

### 3.4 如何获取：按目的选择

- **快速重画图谱**：下载 `GSE210079_RAW.tar`，逐 H5 读入并用 GEO sample title 赋予患者/时间点。
- **重新比对和做环境 RNA/双细胞 QC**：通过 BioProject/SRA 下载 FASTQ；原始体量约 0.40 TB，需要服务器存储。
- **做 TCR—状态分析**：确认每个 barcode 的 VDJ contig 与 GEX 对齐，按患者定义 clonotype。
- **做临床分组复现**：从论文补充表获取短/长 PFS 标签；GEO 文件名本身不足以恢复完整临床终点。

### 3.5 下载后先做什么

1. 列出 24 个 H5 的 barcode 数，实算总细胞数；
2. 建立 `patient × timepoint × assay` 表并核对 28 与 24 两种样本口径；
3. 读取 feature types，分开 RNA 和 ADT；
4. 分患者计算 63 个簇的比例、TCR diversity 和 clonotype expansion；
5. 对长 PFS 仅 4 人的比较报告效应量与不确定性，避免细胞级伪重复。

## 4. 主要发现

短 PFS 与治疗前低 TCR 多样性、超扩增的耗竭样 T 克隆及 BAFF⁺PD-L1⁺髓系状态相关；长 PFS 更伴随 CLEC9A⁺树突细胞、CD27⁺TCF1⁺ T 细胞和较高克隆多样性。

## 5. 与“状态导航”最相关的分析

研究将优化对象从 CAR T 本身扩展到骨髓生态：若只操控 T 细胞内在状态，而忽略髓系抑制与抗原呈递环境，可能无法获得持久缓解。

## 6. 推荐图版

- 队列与多时间点骨髓图谱总览。
- 63 簇组成及长/短 PFS 对比。
- TCR diversity/TCF1⁺ T 与 BAFF⁺PD-L1⁺髓系关联图。

## 7. 创新价值

在 BCMA-CAR T 场景中把肿瘤、免疫细胞、表面蛋白和 TCR 放入同一骨髓纵向框架，强调持久性是生态系统性质。

## 8. 局限性

仅 11 人、分组极不平衡；时间点缺失；多种预处理和疾病负荷混杂；相关性不足以确定髓系或 TCF1 程序的因果方向。

## 9. 对本章节的作用

适合“charting molecular landscape”向“optimize conditions”过渡：目标不只是制造某种 T 细胞，还包括塑造支持该状态的骨髓条件。

## 10. 可直接用于综述的观点

> BCMA-CAR T 后缓解持久性与治疗前骨髓免疫生态相关：多样的 TCR、CD27⁺TCF1⁺ T 细胞及 CLEC9A⁺ DC 倾向于长缓解，而超扩增耗竭克隆和 BAFF⁺PD-L1⁺髓系状态倾向于短缓解（Blood Cancer Discovery 2022, Dhodapkar）。

## 11. 避免误读

- 不要把 24 个 GEO 样本写成 24 名患者。
- 不要把 63 clusters 都称为 CAR T states。
- 72,467 为公共数据索引统计，正式复分析应从下载对象实算并报告版本。
