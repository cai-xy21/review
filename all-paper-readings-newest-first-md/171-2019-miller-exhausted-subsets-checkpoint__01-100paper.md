# 《Subsets of exhausted CD8+ T cells differentially mediate tumor control and respond to checkpoint blockade》精读

## 论文信息

- 作者：Brian C. Miller、Debattama R. Sen、Ramy Al Abosy 等
- 期刊：*Nature Immunology*
- 年份：2019；20: 326–336
- DOI：10.1038/s41590-019-0312-6
- 原文：[Nature Immunology](https://doi.org/10.1038/s41590-019-0312-6)
- PubMed：[PMID 30778252](https://pubmed.ncbi.nlm.nih.gov/30778252/)
- 全文：[PMCID PMC6673650](https://pmc.ncbi.nlm.nih.gov/articles/PMC6673650/)
- 数据总入口：[GEO GSE122713](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE122713)；[BioProject PRJNA506053](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA506053)

## 一句话结论

在慢性 LCMV 与 B16-OVA 肿瘤中，TCF1/SLAMF6 高、TIM-3 低的 progenitor exhausted CD8 T cells 能长期维持并响应 PD-1 阻断，而高度细胞毒的 terminally exhausted cells 更短寿且不直接响应治疗。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 物种 | 小鼠 | 人黑色素瘤部分主要用于组织学/临床相关验证 |
| LCMV scRNA-seq | 9,194 个 gp33 tetramer+ CD8 T cells | Clone 13 感染第 28 天 |
| 肿瘤 scRNA-seq | 11,212 cells（day 10+20）；其中 day 20 为 4,313 cells | B16-OVA TIL，两个分析对象不要相加重复计算 |
| 单细胞状态 | 4 个主要耗竭相关 cluster | 进一步归纳为 progenitor 与 terminal 两大功能轴 |
| 表观组 | bulk ATAC-seq | 分选 progenitor/terminal TIL、LCMV Tex 与 Armstrong memory |
| 转录组 | scRNA-seq + bulk RNA-seq | 不含配对 scTCR-seq |
| 仓库结构 | SuperSeries + 4 个 SubSeries | GSE122675、GSE122712、GSE123235、GSE123236 |

## 1. 研究要解决的问题

PD-1 阻断究竟作用于全部耗竭 TIL，还是只作用于特定可再生亚群？肿瘤耗竭是否与慢性感染共享转录和染色质程序？

## 2. 方法框架

- LCMV Clone 13 慢性感染和 Armstrong 急性感染/记忆对照；
- B16-OVA 肿瘤及抗 PD-1 治疗；
- 10x scRNA-seq 解析异质性；
- SLAMF6/TCF1 与 TIM-3 分选后 bulk RNA-seq、ATAC-seq；
- adoptive transfer、肿瘤控制与再刺激实验验证功能；
- 人黑色素瘤组织检测 progenitor-like TIL 与治疗持续时间关系。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这是一套由**感染单细胞图谱、肿瘤单细胞图谱、分选群体转录组、染色质可及性和功能实验**组成的多层数据，而不是单一 scRNA 文件。单细胞层用于发现状态，bulk RNA/ATAC 用于稳定比较程序，转移实验用于检验状态潜能。

### 3.2 单细胞图谱规模

| 图谱 | 细胞数 × 基因数 | 生物情境 | 用途 |
|---|---:|---|---|
| LCMV Cl13 day 28 | 9,194 × 13,642 | gp33 tetramer+ CD8 T cells | 发现 4 个耗竭相关 cluster |
| B16-OVA day 10+20 | 11,212 × 14,496 | 肿瘤 CD8 TIL | 将感染状态投射到肿瘤 |
| B16-OVA day 20 | 4,313 × 13,880 | day 20 子集 | 聚焦成熟肿瘤状态；已包含于上一对象的相关分析中 |

质控排除检测基因少于 200 或线粒体转录本超过 10% 的细胞；表达少于 3 个细胞的基因也被删除。三个对象分别选择 1,653、1,234 和 914 个高变基因。

### 3.3 四个状态与两大功能轴

LCMV 图谱得到 4 个主要群体，均表达 PD-1/TOX 等耗竭特征；作者再用 Tcf7/Slamf6 与 Havcr2(Tim-3) 等标记把生物学主线归纳为 progenitor exhausted 与 terminally exhausted。前者较少、记忆/自我更新样、可响应 PD-1；后者占多数、细胞毒更强但持久性差。

### 3.4 GEO 包的组成

| accession | 内容 | 下载用途 |
|---|---|---|
| `GSE122713` | 72 个样本的 SuperSeries | 总入口；元数据与子系列导航 |
| `GSE122675` | B16-OVA CD8 TIL 10x scRNA-seq | 肿瘤单细胞矩阵/原始 reads |
| `GSE122712` | LCMV Cl13 gp33+ CD8 10x scRNA-seq | 感染单细胞矩阵/原始 reads |
| `GSE123235` | progenitor/terminal bulk RNA-seq | 状态差异表达 |
| `GSE123236` | progenitor/terminal ATAC-seq | 染色质可及性与 exhaustion enhancer |
| `PRJNA506053` | BioProject umbrella | 统一进入 SRA；约 94 Gbases、44 GB SRA 数据量级 |

SuperSeries 页面提供约 7.3 MB 的 narrowPeak TAR；scRNA 原始 FASTQ 应通过各 SubSeries 的 SRA Run Selector 下载，不能只下载 SuperSeries TAR 后误以为拿到了全部数据。

### 3.5 如何获取

#### 路线 A：快速复用单细胞图谱

分别进入 GSE122675 与 GSE122712，下载 processed count matrix 和 barcode/feature 文件；用 GEO 元数据保留感染/肿瘤、天数、tetramer/分选策略与生物重复。

#### 路线 B：重跑 Cell Ranger/比对

从 PRJNA505998（肿瘤）和 PRJNA506056（LCMV）进入 SRA Run Selector，生成 run table 后用 SRA Toolkit 获取 FASTQ。

#### 路线 C：复现表观与 bulk 分析

下载 GSE123235 的 bulk counts/FASTQ 和 GSE123236 的 ATAC peaks/FASTQ。ATAC 主比较为分选群体，不能与 scATAC 等同。

### 3.6 下载后先做什么

先核对 cell barcode 是否在不同 run 间重复，并将 `sample_id` 加到 barcode 前缀；差异表达和比例检验应以生物重复/小鼠为统计单位。重新整合时保留 day、模型和分选门，避免批次校正把真正的感染—肿瘤差异消掉。

## 4. 主要生物学发现

肿瘤和慢性感染共享广泛 exhaustion chromatin program，包括 TOX 与 Pdcd1 调控区域；但肿瘤也有代谢和细胞因子相关特异改变。progenitor exhausted 可生成 terminally exhausted，并承担 anti-PD-1 后的扩增来源。

## 5. 状态转换证据

证据来自转移与治疗实验，而非 UMAP 邻近：分选的 progenitor cells 可维持并产生 terminal cells；terminal cells 自我更新和治疗响应有限。该设计把“状态标志”提升为“状态潜能”。

## 6. 推荐图版

- Fig. 1：LCMV 四群与肿瘤映射；
- Fig. 2：转录/染色质共享 exhaustion program；
- Fig. 3–4：progenitor 与 terminal 的功能、转移和 PD-1 响应；
- 人黑色素瘤验证图：连接小鼠状态与临床持续应答。

## 7. 创新价值

1. 将肿瘤 Tex 异质性拆成可再生与终末功能群。
2. 联合 scRNA、bulk RNA、ATAC 和转移实验。
3. 指出 PD-1 blockade 的主要细胞靶群不是全部 PD-1+ TIL。

## 8. 局限性

1. 主体是小鼠模型。
2. 单细胞数据无配对 TCR，不能做克隆级谱系追踪。
3. bulk ATAC 平均了分选群体内部异质性。
4. 人数据主要为相关性验证，样本和治疗方案异质。

## 9. 对本章节的作用

适合承担“从分子图谱到可导航状态”的桥梁：先用单细胞识别状态，再用表观和转移实验验证哪些状态能被 checkpoint 扰动并产生后代。

## 10. 可直接用于综述的观点

> 肿瘤中的 progenitor-exhausted CD8 T cells 保留自我更新和向 terminally exhausted TIL 分化的能力，并构成 PD-1 阻断后的主要扩增来源，提示有效导航应保存可再生耗竭前体而非仅追求即时细胞毒状态（Nature Immunology 2019, Miller）。

## 11. 避免误读

- 不要把 4 个 cluster 简化成 4 条确定谱系。
- 不要把 bulk ATAC 写成 scATAC-seq。
- 不要把 terminally exhausted 的强细胞毒误写成“完全无功能”。
- 不要把小鼠 marker 阈值直接当成人 CAR-T 产品放行标准。

