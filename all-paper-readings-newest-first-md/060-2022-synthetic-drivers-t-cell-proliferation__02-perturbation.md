# 《A genome-scale screen for synthetic drivers of T cell proliferation》精读

## 论文信息

- **作者**：Mateusz Legut, Zoran Gajic, Maria Guarino 等
- **期刊与年份**：*Nature*, 2022
- **DOI**：10.1038/s41586-022-04494-7
- **本地原文**：[PDF](<D:/research/review/perturbation33references/20-A genome-scale screen for synthetic drivers of T cell proliferation.pdf>)
- **核心数据入口**：[GEO GSE193736](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE193736)

## 一句话结论

全基因组 ORF gain-of-function 筛选发现多种可人为驱动原代人 T 细胞增殖的因子，其中 LTBR 等命中可在不依赖经典 TCR 共刺激的情况下重塑状态，为合成 T 细胞工程提供候选模块。

## 数据护照

| 模块 | 规模/组成 | GEO记录数 |
|---|---|---:|
| 全基因组 ORF 增殖筛选 | 约 12,000 个 ORF、约 6 barcode/基因；3 位供体；CD4/CD8 | 7 |
| OverCITE-seq | 约 30 个候选 ORF；1 位供体；5 种模态文库 | 5 |
| bulk RNA-seq | CD4/CD8 × LTBR/tNGFR × 静息/刺激 × 3 重复 | 24 |
| ATAC-seq | CD8 × LTBR/tNGFR × 静息/刺激 × 2 重复 | 8 |
| **GSE193736 合计** |  | **44** |

## 1. 研究问题

多数 CRISPR 筛选寻找删除后改善表型的负调控因子，而可直接装入工程细胞的 gain-of-function 模块同样重要。本文问：在人原代 T 细胞中，哪些全长 ORF 的过表达足以驱动增殖，并产生何种状态和调控机制？

## 2. 实验设计与方法框架

作者构建约 12,000 个全长人 ORF 的条形码慢病毒文库，在原代 CD4/CD8 T 细胞中以 CFSE 稀释筛选增殖命中。随后用 OverCITE-seq 同时读取转录组、表面蛋白、ORF、样本标签和 TCR，并对 LTBR 进行 bulk RNA-seq、ATAC-seq与功能验证。

## 3. 数据规模与图谱组成

### 3.1 全基因组 ORF 筛选

- 文库约 **12,000 个全长人 ORF**，平均约 **6 个独立条形码/基因**。
- 使用 **3 位健康供体**的 CD4 与 CD8 T 细胞；起始规模至少约 5×10^8 PBMC，转导率约 20–30%。
- GEO 的 7 个 pooled screen 记录为：质粒文库，以及 CD4/CD8 的 pre-stimulation、pre-sort、CFSE-low 输出等聚合样本。
- 供体在部分提交样本中被汇总，因此“7 个 GEO 记录”既不是 7 位供体，也不是完整列出每位供体的独立文库。

### 3.2 OverCITE-seq 多模态图谱

作者选择约 **30 个候选 ORF**，在 **1 位健康供体**中比较静息与 CD3/CD28 刺激条件，约 **30,000 个细胞被上机/加载**。GEO 将同一批细胞的不同模态拆成 5 个文库记录：

1. GEX：单细胞基因表达；
2. ADT：表面抗体标签；
3. ORF：细胞内 ORF/条形码身份；
4. HTO：样本哈希标签；
5. TCR：TCR 序列。

测序目标约为 GEX >25,000 reads/cell，其他文库 >5,000 reads/cell。30,000 是加载细胞数，不是最终质控通过的细胞数；若 GEO/论文未明确给出最终过滤总数，不应自行把二者等同。5 个记录是 5 种 assay 文库，不是 5 个供体。

### 3.3 LTBR bulk RNA-seq：24 个样本

设计为：

- CD4 与 CD8 两种亚群；
- LTBR ORF 与 tNGFR 对照两种构建；
- 静息与 CD3/CD28 刺激两种状态；
- 每个组合 3 个生物学重复。

因此 **2 × 2 × 2 × 3 = 24** 个 bulk RNA-seq样本。

### 3.4 LTBR ATAC-seq：8 个样本

ATAC 聚焦 CD8 T 细胞：LTBR/tNGFR × 静息/刺激 × 2 重复，即 **2 × 2 × 2 = 8** 个文库。它用于连接 LTBR 诱导的转录变化与染色质开放性。

### 3.5 GSE193736 的文件组成

44 个记录严格闭合为 **7 pooled screen + 5 OverCITE 模态 + 24 bulk RNA + 8 ATAC = 44**。GEO 提供的处理文件包括：

- ORF screen count 文件，约 2.0 MB；
- bulk RNA count matrix，约 698 KB；
- ATAC count matrix，约 1.8 MB；
- `RAW.tar`，约 13.1 MB，含单细胞多模态等 CSV 文件；
- 原始数据位于 SRA / PRJNA797432。

DICE、GTEx v8 与 SCP424 是论文用于背景比较的既往公共数据，不是 GSE193736 的新测序样本。

### 3.6 推荐下载方式

1. 先从 GEO 下载四类处理文件和 sample annotation；筛选命中复核通常无需先下载全部 FASTQ。
2. OverCITE-seq 必须同时保存 GEX/ADT/ORF/HTO/TCR 的 barcode 映射；不能把 5 个库当作独立细胞批次。
3. 若重新处理，通过 SRA Run Selector 按 `screen`、`scRNA/multimodal`、`RNA-seq`、`ATAC-seq` 分批导出 accession list。
4. `prefetch` 后用 `fasterq-dump --split-files`；单细胞 Feature Barcode 文库需保留 read 结构并用相容的 Cell Ranger/自定义流程处理。
5. 下载 DICE、GTEx、SCP424 时单独记录版本与来源，只做外部参照。

### 3.7 下载后的分析方案

筛选层以 ORF/barcode 为单位估计 CFSE-low 富集并在供体/亚群间求稳健命中；OverCITE 层先用 HTO 去混样、ORF 标签赋予扰动，再联合 GEX/ADT/TCR；bulk/ATAC 层用 `LTBR × stimulation × cell_type` 的交互设计。细胞级检验需防止把同一供体的细胞当作大量独立生物学重复。

## 4. 主要结果

筛选识别出可驱动 T 细胞增殖的多类合成因子，LTBR 是重点命中。多模态与 bulk/ATAC 数据显示，不同 ORF 形成不同表面、转录和调控状态；LTBR 可激活非经典 NF-κB 等程序并提高细胞扩增/功能潜力。

## 5. 机制理解

gain-of-function ORF 可直接接入受体、信号或转录网络，使 T 细胞绕过部分外源刺激限制。LTBR 通过持续或配体非依赖的下游信号改变增殖与存活程序，但工程强度和背景依赖性决定其效应。

## 6. 推荐重点阅读的图

- 全基因组 ORF 筛选设计、barcode 富集和命中重复性图。
- OverCITE-seq 的多模态 UMAP/扰动状态图。
- LTBR bulk RNA/ATAC 的 NF-κB 与染色质结果。
- LTBR 工程细胞的扩增和功能验证图。

## 7. 创新性

建立原代人 T 细胞全基因组 gain-of-function 筛选，并用单细胞多模态直接读取扰动身份和表型，是从“找可删的制动器”转向“找可安装的驱动器”的方法学跃迁。

## 8. 局限性

OverCITE 仅 1 位供体；pooled screen 的供体聚合限制严格的 donor-level 推断；ORF 过表达水平可能非生理；强增殖驱动可能牺牲分化质量或安全性。最终过滤细胞数不能由加载数代替。

## 9. 在综述中的定位

适合作为合成免疫学 GOF 筛选和多模态扰动图谱的代表，与 KO 筛选互补。

## 10. 可直接写入综述的表述

> 全基因组 ORF 筛选与 OverCITE-seq 联用揭示了可直接装入原代 T 细胞的合成增殖驱动模块，并以 LTBR 为例连接增殖表型、转录状态和染色质调控。

## 11. 数据复用建议

可先重算 barcode 层命中并评估 CD4/CD8 一致性，再把约 30 个 ORF 的 GEX/ADT 状态投影到公开 T 细胞图谱。LTBR 的 bulk/ATAC 数据适合构建机制签名，但单细胞效应的生物学重复仍是供体而非细胞。

## 12. 转化与安全性关注

任何持续增殖驱动器都需重点评估细胞因子非依赖增殖、克隆优势、插入风险、过度激活和肿瘤形成潜力，并设计可控表达或安全开关。

## 13. 避免误读

- **44 = 7 筛选记录 + 5 多模态文库 + 24 bulk RNA + 8 ATAC。**
- 5 个 OverCITE 记录是同批细胞的 5 种模态，不是 5 位供体。
- 30,000 是加载细胞数，不等于最终质控细胞数。
- DICE、GTEx v8、SCP424 是外部复用数据，不是本文新样本。
