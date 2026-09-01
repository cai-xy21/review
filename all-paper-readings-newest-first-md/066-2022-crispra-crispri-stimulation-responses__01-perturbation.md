# 《CRISPR activation and interference screens decode stimulation responses in primary human T cells》精读

## 论文信息

- **作者**：Ralf Schmidt、Zachary Steinhart、Madeline Layeghi 等
- **期刊与年份**：*Science*, 2022；375: eabj4008
- **DOI**：10.1126/science.abj4008
- **本地原文**：[PDF](<D:/research/review/perturbation33references/32-CRISPR activation and interference screens decode stimulation responses in primary human T cells.pdf>)
- **主 CRISPR SuperSeries**：[GEO GSE174292](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE174292)
- **Perturb-seq**：[GEO GSE190604](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE190604)
- **补充筛选**：[GEO GSE190846](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE190846)
- **代码与处理数据**：[Zenodo 5784651](https://zenodo.org/records/5784651)

## 一句话结论

作者建立可在原代人 T 细胞中规模化运行的 CRISPRa/CRISPRi 平台，通过全基因组增益与减益筛选识别 IL-2、IFN-γ 等刺激应答调控器，并用 Perturb-seq 显示不同转录因子激活可把 T 细胞推入具有不同细胞因子组合的状态。

## 数据护照

| 数据集 | 规模 | 组成 |
|---|---:|---|
| GSE174255（GSE174292 子系列） | 52 个样本 | 4 plasmid input + 48 个 CRISPRa/i、2 donors、2 library halves、IL2/IFNG high/low/unsorted |
| GSE174284（GSE174292 子系列） | 16 个 bulk RNA-seq | 4 donors × sgControl/sgFOXQ1 × no-stim/stim |
| GSE174292 合计 | 68 个 GSM | 52 pooled screen + 16 bulk RNA |
| GSE190604 | 16 个测序记录 | 4 wells × stim/no-stim × guide/mRNA；同一 8 个生物 wells 的双文库 |
| Perturb-seq 处理矩阵 | 约 103,805 cells、36,601 features（公开矩阵规模） | MTX 950.1 MB + guide calls |
| GSE190846 | 16 个补充 CRISPR 样本 | donors 15/16；IFNG/IL2/TNFa high-low 等 |
| Zenodo | 3.1 GB + 26.2 MB + README | Perturb-seq 处理对象、全基因组筛选结果和代码 |

## 1. 研究问题

T 细胞刺激后的 cytokine output 对免疫治疗至关重要，但传统 KO 只能发现“必要因子”，难以发现“足以驱动”的增益节点。作者希望在原代人 T 细胞中同时实现 CRISPRa 与 CRISPRi，以互补解析刺激依赖的细胞因子调控网络。

## 2. 实验设计与方法框架

作者优化 CRISPRa/i machinery 的慢病毒递送，使用 Calabrese（CRISPRa）和 Dolcetto（CRISPRi）两半 library，在人 T 细胞中按 IL-2/IFN-γ high、low 和 unsorted 分选。随后阵列验证和 secretome profiling，并以 CRISPRa Perturb-seq 比较静息与再刺激状态，重点分析 FOXQ1、BATF、PRDM1、MAF 等命中。

## 3. 数据规模与图谱组成

### 3.1 GSE174255：52 个 pooled screen 样本

结构可精确分解为：

- plasmid input：CRISPRa Calabrese Set A/B + CRISPRi Dolcetto Set A/B，共 4；
- 细胞筛选：2 种模式（a/i）× 2 个 library halves（A/B）× 2 位供体 × 2 个 cytokine（IFNG/IL2）× 3 个 bins（high/low/unsorted）= 48；
- 总计：**4 + 48 = 52**。

该设计把 library half、donor、cytokine 和 FACS bin 完整展开。比较 high vs low 时应以相同 mode、half 和 donor 配对，unsorted 用于评估生长/representation bias。

### 3.2 GSE174284：16 个 FOXQ1 bulk RNA-seq 文库

样本为 **4 donors × 2 guides/conditions（sgControl、sgFOXQ1）× 2 stimulation states = 16**。提供 `GSE174284_gene_counts_raw.txt.gz` 和 SRA reads。它验证 FOXQ1 激活如何在静息/刺激下重塑转录，不是全基因组筛选 read count。

GSE174292 是以上两个子系列的 SuperSeries，共 68 GSM。

### 3.3 GSE190604：CRISPRa Perturb-seq

GEO 列出 16 个记录：

- no-stim 4 wells + stim 4 wells，共 8 个生物/上机 wells；
- 每个 well 分别有 guide-capture 与 mRNA library，因此 8 × 2 = 16 个记录。

公开处理文件：

| 文件 | 大小 | 用途 |
|---|---:|---|
| `GSE190604_matrix.mtx.gz` | 约 950.1 MB | feature-barcode 稀疏矩阵 |
| `features.tsv.gz` | 约 326.8 KB | 基因/feature 列表 |
| `barcodes.tsv.gz` | 约 419.4 KB | 细胞条形码 |
| `cellranger-guidecalls-aggregated-unfiltered.txt.gz` | 约 976.4 KB | 逐细胞 guide assignment |

公开矩阵约 103,805 个细胞、36,601 个 features；这是归档矩阵规模，具体 QC 后 singlet 数应按代码重新计算。后续研究报告约 61k single-guide training cells和 28,707 multiplet cells，是其自身过滤结果，不应替代本文原始 QC 数。

### 3.4 GSE190846：16 个补充 cytokine screen 样本

两位供体（15、16）各 8 个样本：IFNG high/low 各 2 个重复（4），IL2 high/low（2），TNFa high/low（2），合计每供体 8、总计 16。提供 `GSE190846_supp_CD4_CRISPR_screens_read_counts.tsv.gz`（约 3.8 MB）和 SRA amplicon reads。

### 3.5 Zenodo 5784651

| 文件 | 大小 | 内容 |
|---|---:|---|
| `CRISPRa-Perturb-seq.zip` | 约 3.1 GB | 处理后的 Perturb-seq 数据与分析代码 |
| `Genome-wide-screens.zip` | 约 26.2 MB | 全基因组筛选结果/代码 |
| `README.txt` | 926 B | 文件说明 |

论文 PDF 参考文献处的 DOI 排版容易漏掉末位；可用的记录是 **10.5281/zenodo.5784651**。

### 3.6 推荐下载方式

1. 复现命中排名：优先下载 Zenodo `Genome-wide-screens.zip` 和 GEO read-count 表。
2. 单细胞重分析：取 Zenodo 3.1 GB 包或 GEO MTX + guide calls；用 barcode 精确连接。
3. 重跑原始 reads：分别进入 PRJNA729110、PRJNA787633、PRJNA788572 等 BioProject/SRA。
4. FOXQ1：用 16 个 bulk count 文库做 donor 配对与 stimulation interaction。

## 4. 主要结果

CRISPRa 与 CRISPRi 找到互补调控器，部分命中只在刺激后影响 cytokine。阵列验证显示单个转录因子激活可改变多细胞因子组合。Perturb-seq 将扰动映射到不同激活状态，揭示 FOXQ1 等非经典因子可重编程 T 细胞刺激应答。

## 5. 机制理解

cytokine regulation 不是单一线性通路，而是“基线可及性 × 刺激诱导转录因子 × 反馈网络”的组合。CRISPRa发现足以开启程序的节点，CRISPRi发现维持程序所必需的节点，两者交集更接近稳健工程靶点。

## 6. 推荐重点阅读的图

- CRISPRa/i 平台与 IL2/IFNG high-low screen。
- gain- vs loss-of-function 命中比较。
- arrayed secretome 图。
- 103k-cell Perturb-seq 状态与扰动网络。
- FOXQ1 bulk RNA/功能验证。

## 7. 创新性

在原代人 T 细胞实现全基因组 CRISPRa 和 CRISPRi，并把 pooled cytokine phenotype screen 与单细胞扰动转录组连接。

## 8. 局限性

主要供体数有限，screen 对 donor-specific effect 的估计不足。高/低分选受蛋白检测阈值影响。CRISPRa 过表达幅度可能超出生理范围；Perturb-seq 的多 guide/multiplet 和刺激批次需要严格处理。

## 9. 在综述中的定位

适合作为“原代人 T 细胞双向功能基因筛选平台”的方法学标杆，并可与 KO-only、ORF screen和 in vivo screen 对比。

## 10. 可直接写入综述的表述

> 原代人 T 细胞 CRISPRa/CRISPRi 全基因组筛选以增益和减益互补地解析 IL-2、IFN-γ 调控网络，并通过约 10.4 万细胞的 Perturb-seq 揭示刺激依赖的可工程化细胞状态。

## 11. 数据复用建议

先在 52 个主 screen 样本中按 mode/library half/donor 做配对 high-low，再与 16 个补充 screen 交叉。Perturb-seq 按 well 和 stimulation pseudobulk，guide singlet 为主分析，multiplet 单独处理。FOXQ1 16 bulk 文库用于验证单细胞推断。

## 12. 转化与安全性关注

CRISPRa 靶点用于细胞工程时需评估持续表达、非目标 cytokine、细胞因子释放综合征和插入载体风险；更可行的方案可能是短暂 mRNA/小分子调节，而非永久激活。

## 13. 避免误读

- GSE174292 的 68 个 GSM 包含 52 screen + 16 bulk，不是 68 位供体。
- GSE190604 的 16 记录来自 8 个 wells 的 guide/mRNA 双文库。
- 103,805 是公开矩阵细胞量级，过滤后有效细胞数取决于 QC。
- Zenodo DOI 为 5784651，不是缺末位的 578465。

