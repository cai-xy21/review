# 《Pan-cancer single-cell landscape of tumor-infiltrating T cells》精读

## 论文信息

- 第一作者：Liangtao Zheng、Shishang Qin、Wen Si（共同第一作者）
- 期刊：*Science*，2021；374(6574): abe6474
- DOI：10.1126/science.abe6474
- 原文：[Science](https://www.science.org/doi/10.1126/science.abe6474)
- PubMed：[PMID 34914499](https://pubmed.ncbi.nlm.nih.gov/34914499/)
- 处理数据：[GEO GSE156728](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE156728)
- BioProject：[PRJNA658913](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA658913)
- 代码：[Zenodo 5461803](https://doi.org/10.5281/zenodo.5461803)

## 一句话结论

研究整合 316 名供者、21 种癌症的 397,810 个 T 细胞，建立首批大规模泛癌 TIL 统一图谱，识别 17 类 CD8 与 24 类 CD4 状态，并显示 CD8 耗竭可沿多条、癌种偏好的状态路径形成。

## 1. 研究问题

单癌种研究无法区分“跨癌种保守状态”和“癌种特异状态”。作者要回答：TIL 状态有哪些共同坐标；CD8 T 细胞如何走向耗竭；肿瘤突变负荷、组织来源与临床特征如何塑造状态组成。

## 2. 数据护照

| 维度 | 规模/内容 | 解读 |
|---|---:|---|
| 高质量 T 细胞 | 397,810 | 主图谱细胞数 |
| 供者 | 316 | 不等于样本数；部分供者有肿瘤、邻癌或血液配对样本 |
| 癌症 | 21 种 | 泛癌比较的主体 |
| CD8 状态 | 17 类 | 含初始/记忆、效应、驻留、增殖、过渡与耗竭等 |
| CD4 状态 | 24 类 | 含 naive/memory、Tfh、Th17、Treg、细胞毒与增殖等 |
| 模态 | scRNA-seq；168,901 个细胞有 productive TRA–TRB | TCR 子集来自论文报告的 87 名供者、15 个癌种，不是全部 316 名供者/21 癌种 |
| 公开入口 | GSE156728 / PRJNA658913 / Zenodo 代码 | 处理矩阵与原始测序入口分开 |

## 3. 分析框架

1. 从多中心数据中提取 T 细胞，统一质控、整合与细分聚类。
2. 按癌种、组织和患者比较状态组成，避免只看合并 UMAP。
3. 用轨迹/转录程序分析 CD8 由过渡或效应状态走向耗竭的多条路径。
4. 在带 TCR 的子集中分析克隆扩增与状态共享，辅助判断状态联系。
5. 将 T 细胞组成与肿瘤突变负荷等患者特征关联，并用组成对患者进行分层。

## 4. 关键发现

### 4.1 TIL 具有共享状态，但比例高度癌种化

多数主要状态可跨癌种复现，但每个癌种占据的状态组合不同。图谱的价值不是证明“所有癌症一样”，而是提供共同标签后再量化差异。

### 4.2 CD8 耗竭不是单一路径

作者解析出多条指向耗竭的状态转换路径，不同癌种对路径有偏好。TOX、PRDM1 等耗竭相关转录因子较普遍，SOX4、FOXP3 等程序呈癌种偏向。

### 4.3 T 细胞组成可表征患者生态

部分状态与突变负荷、组织来源和临床特征相关；只使用肿瘤内 T 细胞组成也能形成具有临床特征差异的患者群。但这属于回顾性分层，不是已经验证的临床分类器。

## 5. TCR 分析应怎样理解

### 5.1 精确规模

| 口径 | 数量 | 解释 |
|---|---:|---|
| 图谱全部 T 细胞 | 397,810 | 来自 316 名供者、21 个癌种 |
| 有分析级配对 TCR 的细胞 | 168,901 | 至少一对 productive TCR α、β 链；约占总图谱 42.5% |
| TCR 供者/癌种 | 87 / 15 | 不能把 TCR 结论外推到全部癌种 |
| clonotype | 92,533 | 原文及作者发布 `cloneID` 口径；克隆定义在供者内使用 |
| clonal cells | 53.9% | 相同 TCR pair 至少出现在两个细胞中 |
| expanded clonotypes | 14,631 | 克隆大小至少 2 个细胞 |
| CD4/CD8（作者 RDS） | 74,222 / 94,679 | 处理后 TCR 表逐行实测 |
| 肿瘤/邻癌/外周血（作者 RDS） | 98,698 / 57,199 / 13,004 | `loc=T/N/P`；支持跨组织迁移与共享分析 |

作者公开的 `data/tcr/byCell/tcr.zhangLab.comb.flt.rds` 每行一个细胞，包含 `CDR3.A1/A2`、`CDR3.B1/B2`、V/J identifier、`cloneID`、`cloneSize`、供者、组织位置、癌种和转录状态等。当前发布 RDS 实测有 168,901 行和 92,533 个 `cloneID`；其 `patient` 字段有 86 个不同字符串，而正文报告 87 名供者，严谨复现时应进一步核对 Supplementary Table 的 ID 合并关系。

GEO 的 `GSE156728_10X_VDJ.merge.txt.gz` 是更接近 Cell Ranger 输出的 contig 长表：442,618 条 contig、172,995 个 cell-library barcode。按 `is_cell + high_confidence + full_length + productive` 且同时具有 TRA/TRB 的严格规则，可得到约 147,442 个 10x 配对 αβ 细胞；它不包含作者整合的全部 Smart-seq2 TCR，因此不应拿这个数字替代论文的 168,901。

### 5.2 原文如何使用 TCR

1. **克隆扩增**：统计 clone size，并用 STARTRAC expansion index 比较不同 CD4/CD8 metacluster 的扩增强度。
2. **组织迁移**：利用同一供者、同一 clonotype 在外周血、邻癌和肿瘤之间的共享计算 STARTRAC migration index。
3. **状态联系**：利用同一 clonotype 跨 metacluster 分布计算 transition index 和 pairwise transition index（pTrans）；`pTrans > 0.1` 用作高连接提示。
4. **支持 CD8 耗竭路径**：克隆共享支持 `IL7R+ Tm → ZNF683+ Trm → CXCL13+ terminal Tex` 和 `GZMK+ Tem → PDCD1+ Tex → CXCL13+ terminal Tex` 两条主要路线，并显示部分癌种存在 KIR+ NK-like T、Tc17 或 CD8 Treg 与 Tex 的连接。
5. **定义 potentially tumor-reactive T cells（pTRT）**：筛选肿瘤内至少 2 个细胞的扩增克隆，寻找其中 TCR signaling 或 proliferation gene set 显著升高的细胞/mini-cluster，再把共享同一 TCR 的细胞纳入 pTRT。结果指向四类 CD8 Tex，以及 CD4 的 `IFNG+ Tfh/Th1` 和 `TNFRSF9+ Treg`。

同一 clonotype 跨状态只能说明克隆联系，不能单独给出状态转换方向；方向主要来自 RNA velocity、Monocle/diffusion map 等转录轨迹证据。TCR 共享也不能证明具体抗原、HLA 限制性或真正的肿瘤反应性，因此 pTRT 中的 “potentially” 不能省略。

### 5.3 是否值得复用

- **可以直接用**：泛癌克隆扩增、血液—邻癌—肿瘤迁移、克隆跨状态共享、TCR–转录状态联合建模和方法 benchmark。
- **不适合直接声称**：抗原特异性、neoantigen 特异性、TCR–HLA 配对或临床可用 TCR；数据没有系统的实验抗原标签。
- 复用时以 `patient + cloneID` 为克隆单位，不能跨患者直接合并相同 CDR3；还应处理双 α/双 β 链、平台（10x/Smart-seq2）、癌种和组织来源不均衡。

## 6. 数据如何获取与复用

### 快速复用

- 在 [GSE156728](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE156728) 获取处理表达矩阵和样本文件。
- 在 [Zenodo 5461803](https://doi.org/10.5281/zenodo.5461803) 获取论文分析代码。
- Xue 2025 数据包提供再整理的 `adata_cd8_zheng.h5ad`，适合只做 CD8 映射比较：[Zenodo](https://doi.org/10.5281/zenodo.10472913)。

### 从原始数据重做

原始数据来自多个队列，部分受控；从 GSE156728/BioProject 和论文补充表追溯各研究 accession。统一重算时至少记录 `study、sample、patient、tissue、cancer、platform、treatment`，并以患者为统计重复。

## 7. 推荐图版

- **Fig. 1**：21 癌种、近 40 万 T 细胞的数据组成与总体图谱；章节开场首选。
- **Fig. 2–3**：CD8/CD4 精细状态和癌种组成；讲“共享坐标 + 癌种偏好”。
- **CD8 exhaustion trajectory 主图**：讲多路径耗竭；截取时保留路径图、关键 marker 与癌种分布三部分。
- **患者分层图**：只在强调临床相关时使用，并标明回顾性。

## 8. PPT 单页格式

**标题**：泛癌图谱显示 CD8 耗竭存在多条癌种偏好路径

**正文**：

1. 316 名供者、21 种癌症、397,810 个 T 细胞。
2. 统一识别 17 类 CD8 与 24 类 CD4 状态。
3. 不同癌种共享状态坐标，却偏好不同耗竭路径与状态组合。

**配图**：Fig. 1 总图 + CD8 exhaustion trajectory 局部。

**页脚引用**：Science 2021, Zheng。

## 9. 局限性与避免误读

- 跨研究整合不能完全消除平台、取样和组织解离偏差。
- 轨迹推断不是纵向谱系追踪；癌种偏好也可能受队列构成影响。
- 细胞数巨大，但患者数才是临床统计的有效重复层级。
- 与 Chu 2023 和 Xue 2025 存在来源数据重叠，综述中不能把三者细胞数相加成独立证据。

## 10. 可直接用于综述

> 泛癌单细胞整合在 397,810 个 T 细胞中建立了跨癌种共享的状态坐标，并揭示 CD8 耗竭可经由多条、具有癌种偏好的状态路径形成，说明“耗竭”应被建模为情境依赖的状态族而非单一终点（Science 2021, Zheng）。
