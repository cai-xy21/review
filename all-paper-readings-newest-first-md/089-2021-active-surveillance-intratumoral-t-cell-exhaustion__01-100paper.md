# 《Active surveillance characterizes human intratumoral T cell exhaustion》精读

## 论文信息

- 作者：Ran You、Jordan Artichoker、Adam Fries 等
- 期刊：*Journal of Clinical Investigation*
- 年份：2021；131(18): e144353；发表于 2021 年 9 月 15 日
- DOI：10.1172/JCI144353
- 原文：[JCI](https://www.jci.org/articles/view/144353)
- PubMed：[PMID 34292884](https://pubmed.ncbi.nlm.nih.gov/34292884/)
- 开放全文：[PMC8439597](https://pmc.ncbi.nlm.nih.gov/articles/PMC8439597/)
- 转录组数据：[GEO GSE179975](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE179975)
- SRA：[SRP328019](https://www.ncbi.nlm.nih.gov/sra/?term=SRP328019)；BioProject：[PRJNA746001](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA746001)
- 补充数据与视频：[JCI supplemental data](https://www.jci.org/articles/view/144353/sd/1)

## 一句话结论

新鲜人肿瘤切片的双光子成像显示，耗竭表型较强的 CD8⁺ T 细胞并非静止，而具有更快的“巡逻”运动；论文公开了 19 份 CRC T 细胞 bulk RNA-seq（20 个 SRA runs、约 231 Gb），但没有公开完整的逐细胞轨迹、原始双光子栈或流式 FCS。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 人肿瘤成像队列 | 17 个新鲜肿瘤，5 类癌症 | 每个活检至少 3 个 ROI；轨迹数未作为公共表发布 |
| 癌种 | HNSC、CRC、妇科、肾、肝肿瘤 | 组织类型异质，样本量不足以做癌种分层结论 |
| 影像 | 双光子 3D time-lapse；补充 2 个 mp4 | 公开视频是示例，不是全队列原始数据 |
| 同组织表型 | 流式：CD38、CTLA-4、PD-1、Ki67 等 | 17 个活检按平均速度 <1 μm/min 划分 immotile |
| RNA-seq | 19 份 CRC 样本，Illumina HiSeq 4000 | bulk 分选 CD4⁺+CD8⁺ T 细胞，不是 scRNA-seq |
| SRA | 20 paired-end runs | 1 个样本拆为 2 runs；约 1.143 billion read pairs/spot records |
| 测序量 | 230,822,160,122 bases；SRA Lite 约 87,962 MB | 当前数据库统计，下载 FASTQ 后空间更大 |
| 处理矩阵 | 16,155 genes × 10 CD38-high；× 9 CD38-low | Figure 4 使用 top/bottom 33rd percentile，需重建筛选逻辑 |

## 1. 研究要解决的问题

“耗竭”常由抑制受体、转录和表观状态定义，但这些细胞在肿瘤中的真实行为并不清楚。论文问：耗竭 T 细胞是否真的行动迟缓；能否在不激活内源 T 细胞的前提下，对人肿瘤活检进行实时成像；运动状态能否与同一样本的流式和转录特征相连。

## 2. 方法框架

### 2.1 live-biopsy 成像

新鲜肿瘤在术后小于 5 h 内置于预热、充氧介质，切片后在加热灌流室中用双光子显微镜成像。作者筛选不引起可检测 Ca²⁺通量的抗体克隆，并尽量使用 Qdot 增强亮度和光稳定性，标记内源 CD8 T 细胞、CD14/HLA-DR 髓系及 EpCAM 肿瘤细胞。

每个活检随机采至少 3 个 ROI，追踪 T 细胞速度、位移、与 APC/肿瘤细胞接触时间和局部细胞密度。平均速度 <1 μm/min 的活检被归为 immotile，用于与流式表型对比。

### 2.2 小鼠校准

在 PyMT-ChOVA 乳腺肿瘤模型中，将 OT-I 细胞分别提前 4 d（新到达效应细胞）或 14 d（长期驻留/耗竭样）转入；并比较切片成像与同一模型的 intravital imaging。还用 B78 和 MC38 验证切片条件能保留与体内相近的运动速率。

### 2.3 RNA-seq

另一组 CRC 样本经组织消化、流式分选肿瘤浸润 CD4⁺/CD8⁺ T 细胞后进行 bulk RNA-seq。样本按 CD38 丰度代表耗竭程度；Figure 4 比较 CD38 top 33rd 与 bottom 33rd percentile，寻找运动/细胞骨架相关基因。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

本研究由三个互补但并非完全同一批样本的数据层构成：

1. **时空行为层**：人新鲜肿瘤切片中内源 T 细胞的 3D 位置—时间轨迹、局部肿瘤密度与接触事件；
2. **表型层**：同一活检拆分后的流式免疫表型；
3. **分子层**：19 份 CRC 分选 T 细胞 bulk RNA-seq；
4. **校准层**：鼠肿瘤 d4/d14 OT-I 的表型、活体/切片轨迹和同细胞 PD-1—速度关系。

“同一肿瘤成像 + 流式”形成真正的状态—行为连接；GEO RNA-seq 是独立 CRC 队列的群体分子支持，不能把每条 RNA 样本直接配到 17 个成像活检。

### 3.2 人成像与流式队列

| 层级 | 规模/内容 | 应如何理解 |
|---|---:|---|
| 新鲜肿瘤 | 17 | 5 类癌症；每例拆分做 live imaging 和 flow cytometry |
| 癌种 | HNSC、CRC、GYN、KID、HEP | 论文旨在跨癌种方法展示，不是每癌种充分统计 |
| ROI | 每例 ≥3 | 局部异质性是核心；ROI 不能当独立患者 |
| 运动阈值 | 样本平均 <1 μm/min 为 immotile | 是分析用二分，不是自然生物学边界 |
| 成像接触 | 可观察至少 90 min 的 T–APC–tumor conjugate | 示例不代表所有细胞接触时长 |

论文没有发布“总追踪细胞数、每 ROI 帧数、z-stack 数、轨迹长度分布”的机器可读清单；这些细节只能从图和补充材料获得，无法据公开库重建完整 motion dataset。

### 3.3 GEO/SRA 数据集详细组成

[GSE179975](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE179975) 的官方记录为 19 个样本：

- 14 个原发 CRC 肿瘤样本；
- 3 个转移灶样本（样本名含 `.T2`）；
- 2 个 juxta-tumoral reactive normal 样本（`.N1`）；
- 每个样本均为分选的 CD4⁺和 CD8⁺ T 细胞 bulk RNA；
- 平台 GPL20301，Illumina HiSeq 4000；paired-end，RunInfo 平均 read length 字段为 202，即约 2×101 bp。

数据库当前有 **20 个 SRR runs**，因为 `GSM5444358 / IPICRC064.T1` 对应两个 runs；因此 19 samples ≠ 19 runs。

按 NCBI RunInfo 汇总：

| 指标 | 数值 |
|---|---:|
| Samples | 19 |
| SRA runs | 20 |
| spots/read pairs records | 1,142,683,961 |
| bases | 230,822,160,122 |
| SRA Lite size | 87,962 MB，约 85.9 GiB |
| 单 run spots 范围 | 18,939,498–114,157,414 |

### 3.4 处理文件有什么

GEO `suppl/` 提供两个很小的 gzip TSV：

| 文件 | 压缩体积 | 实测维度 | 内容 |
|---|---:|---:|---|
| `GSE179975_CRC_CD38_HI_EHK8-10_tcell_counts_san_swaps.tsv.gz` | 398.9 KB | 16,155 genes × 10 samples | CD38-high 标签组的 count matrix |
| `GSE179975_CRC_CD38_LOW_EHK8-10_tcell_counts_san_swaps.tsv.gz` | 365.5 KB | 16,155 genes × 9 samples | CD38-low 标签组的 count matrix |

两个文件合计覆盖 19 个样本列。需要注意，主文 Figure 4 明确写 top/bottom 33rd percentile；因此重新做 DEG 时不能仅按两个文件名直接把 10 对 9 全部比较，而应查清作者的 CD38 评分、percentile cutoff 和最终纳入样本。

GEO series matrix 只有约 3.1 KB，主要是元数据，不是表达矩阵。原始 reads 在 SRA，不能从 GEO 的两个 TSV 反推 BAM/FASTQ。

### 3.5 如何下载

#### 路线 A：快速复用处理矩阵

```bash
wget -c https://ftp.ncbi.nlm.nih.gov/geo/series/GSE179nnn/GSE179975/suppl/GSE179975_CRC_CD38_HI_EHK8-10_tcell_counts_san_swaps.tsv.gz
wget -c https://ftp.ncbi.nlm.nih.gov/geo/series/GSE179nnn/GSE179975/suppl/GSE179975_CRC_CD38_LOW_EHK8-10_tcell_counts_san_swaps.tsv.gz
```

读取后应确认第一列 `gene_id`，并检查样本名中的 `.T1/.T2/.N1`。

#### 路线 B：下载原始 RNA-seq

在 [SRA Run Selector](https://www.ncbi.nlm.nih.gov/Traces/study/?acc=SRP328019) 导出 RunInfo/Accession List，再用 SRA Toolkit：

```bash
prefetch --option-file SRR_Acc_List.txt
fasterq-dump --split-files --threads 8 --outdir fastq SRR15110349
```

20 runs 总量很大，应预留远高于 86 GiB 的临时和 FASTQ 空间，并在合并同一 GSM 的多个 runs 前保留 run provenance。

#### 路线 C：影像与轨迹

JCI/PMC 只公开两个示例 mp4（PMC 页面约 0.9 MB 和 4.9 MB）及补充 PDF，没有全量 TIFF/OME-TIFF、Imaris track export 或 ROI 表。完整成像数据需向作者申请，并应同时请求 voxel size、frame interval、z-depth、漂移校正、track gap/length cutoff 和 ROI 掩膜。

### 3.6 下载后先做什么

RNA-seq 应先对 20 runs 做 FastQC/MultiQC，再按 GSM 合并或联合比对；差异分析以样本为单位。成像数据若取得，应先把像素坐标换算为 μm，按 biopsy→slice→ROI→cell 建立层级模型，不要把所有轨迹平铺后做普通 t test。

## 4. 主要发现

1. 小鼠肿瘤中 d14 长期驻留/耗竭样 OT-I 比 d4 新到达细胞运动更快。
2. 17 个人人肿瘤活检的 CD8 T 细胞速度存在显著患者和区域差异。
3. 局部 EpCAM⁺肿瘤密度与 T 细胞速度负相关；T 细胞或髓系密度、T–APC 接触时间不是稳定解释变量。
4. 平均速度较快的肿瘤中，CD8 T 细胞 CD38、CTLA-4、PD-1 和 Ki67 较高。
5. CD38-high CRC T 细胞上调 ENTPD1、LAG3、HAVCR2、TOX2，并富集细胞迁移、极性和微管相关基因；TNF 下调。
6. 同细胞小鼠实验显示 PD-1 强度与速度正相关，支持部分细胞内在联系。

## 5. 关键行为与分子 readouts

- 行为：平均速度、top 10% speed、track displacement、接触时间；
- 空间：EpCAM⁺肿瘤体积分数、CD8/HLA-DR/CD14 密度、ROI；
- 表型：CD38、CTLA-4、PD-1、Ki67；
- RNA：ENTPD1、LAG3、HAVCR2、TOX2、TNF，以及 PDLIM4、AFAP1L2、PLK4、KIF20A、MYO7A、MYO7B、CAV1。

## 6. 状态—功能—分子驱动如何连接

论文建立的是相关链：耗竭 marker 高 → 运动更快 → 运动/细胞骨架基因上调。小鼠 d4/d14 和同细胞 PD-1—速度分析增强了机制可信度，但仍不能证明某个基因直接驱动人 T 细胞巡逻，也不能确定快速运动改善还是削弱肿瘤杀伤。

## 7. 推荐图版

- **Fig. 1**：live-biopsy 方法与小鼠 d4/d14 校准。
- **Fig. 2**：区域肿瘤密度与 T 细胞运动，最适合“空间影响状态”。
- **Fig. 3**：17 个活检的运动—耗竭表型连接。
- **Fig. 4**：RNA 分子程序和同细胞 PD-1—速度。

如果只能选一张，选 Fig. 3；若强调多模态闭环，选 Fig. 2 + Fig. 4。

## 8. 创新价值

1. 用非刺激抗体直接观察人肿瘤内源 T 细胞，而非外加标记 T 细胞。
2. 同一活检连接动态行为、局部空间密度和流式状态。
3. 纠正“exhausted = immobile/inactive”的直觉。
4. 提供可下载 bulk RNA-seq 支持运动分子程序。

## 9. 局限性

1. 切片缺少血流、淋巴、神经和完整趋化因子场，不能完全代表体内。
2. 17 个活检跨 5 癌种，癌种内样本很小。
3. 速度阈值和 ROI 选择会影响二分类。
4. 流式与运动多为样本层关联，不是人单细胞同步 readout。
5. GEO 是 bulk T 细胞，CD4/CD8 混合比例可能混杂 DEG。
6. 全量影像、轨迹和 FCS 未公开，公开 RNA 不能复现核心动态分析。
7. 快速巡逻与真实杀伤、抗原特异性、治疗反应之间仍未建立因果。

## 10. 对本综述章节的作用

这篇是“live cell tracking”与“link state/function transitions with molecular drivers”的桥梁：它展示了如何把活细胞运动嵌入状态定义，但也提醒实时成像和终点 omics 尚未在同一细胞上闭环。

## 11. 可直接用于综述的观点

> 新鲜人肿瘤切片的双光子成像显示，耗竭标志较高的 CD8⁺ T 细胞仍保持快速巡逻，并伴随细胞骨架和迁移基因程序上调，说明“耗竭”是功能维度分离的动态状态，而非简单的全面停摆（JCI 2021, You）。

## 12. 数据复用建议

GSE179975 适合重做 CD38-high/low bulk RNA 差异和迁移基因集验证；若研究重点是实时追踪，必须申请原始 OME-TIFF/轨迹数据。最理想的后续设计是在同一切片先成像、再回收已追踪细胞做空间或单细胞分子测量。

## 13. 避免误读

- 不要把 GSE179975 写成 19 个单细胞 RNA-seq 样本。
- 不要把 19 samples 与 20 SRA runs 混为一谈。
- 不要把快运动等同于高杀伤或更好疗效。
- 不要声称公开库含完整人肿瘤轨迹；核心动态原始数据没有公共下载。
