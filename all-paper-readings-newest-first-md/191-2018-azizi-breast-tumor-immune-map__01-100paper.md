# 《Single-cell map of diverse immune phenotypes in the breast tumor microenvironment》精读

## 论文信息

- **作者**：Azizi E, Carr AJ, Plitas G, et al.
- **期刊与年份**：Cell, 2018
- **DOI**：[10.1016/j.cell.2018.05.060](https://doi.org/10.1016/j.cell.2018.05.060)
- **原文**：[Cell](https://www.cell.com/cell/fulltext/S0092-8674(18)30723-2)
- **PubMed**：[PMID 29961579](https://pubmed.ncbi.nlm.nih.gov/29961579/)
- **开放全文**：[PMC6348010](https://pmc.ncbi.nlm.nih.gov/articles/PMC6348010/)
- **GEO SuperSeries**：[GSE114727](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE114727)
- **inDrop 主图谱**：[GSE114725](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE114725)
- **10x RNA+TCR**：[GSE114724](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE114724)
- **流式/CyTOF**：[FlowRepository FR-FCM-ZYJP](https://flowrepository.org/id/FR-FCM-ZYJP)
- **本报告核对日期**：2026-08-23

## 一句话结论

本文以 8 例初治乳腺癌的 47,016 个 CD45+ 单细胞为核心，联合另一批 27,000 个带配对 TCR 的 T 细胞和 6,556 个 CyTOF T 细胞，显示乳腺肿瘤免疫状态主要沿连续的激活/分化轴变化，而非由少数离散亚型完全解释；TCR 使用是 T 细胞表型多样性的一个重要来源。

## 数据护照

| 项目 | 内容 |
|---|---|
| 主队列 | 8 例未治疗原发乳腺癌，覆盖 ER/PR、HER2+、TNBC 等亚型 |
| 配对组织 | 肿瘤、邻近正常乳腺、外周血、淋巴结，具体组合依患者而异 |
| 主技术 | inDrop 3′ droplet scRNA-seq；47,016 个 CD45+ 细胞 |
| TCR 队列 | 另外 3 例肿瘤（BC9–BC11）的约 27,000 个 CD3+ T 细胞；10x 5′ GEX + 配对 V(D)J |
| 蛋白验证 | 另外 3 例肿瘤（BC12–BC14）的 6,556 个 T 细胞；CyTOF |
| 图谱规模 | 主 atlas 83 个免疫 cluster；覆盖 T、B、NK、髓系等 |
| 开放数据 | GSE114725（inDrop）、GSE114724（10x RNA/TCR）、GSE114727（总入口） |
| 软件 | [SEQC](https://github.com/ambrosejcarr/seqc)、[BISCUIT](https://github.com/sandhya212/BISCUIT_SingleCell_IMM_ICML_2016) |

## 1. 研究问题

乳腺肿瘤微环境的免疫多样性由离散细胞类型、连续激活状态还是克隆差异主导？同一患者肿瘤与血液/正常组织的状态分布如何不同？转录组、配对 TCR 与蛋白表型是否给出一致结论？

## 2. 方法框架

1. 用 inDrop 对主队列 CD45+ 细胞进行高通量转录组测序。
2. 采用层级聚类/BISCUIT 等方法解析 83 个免疫群，并建立连续激活和分化坐标。
3. 在独立 10x 队列中同时测量 T 细胞转录组与配对 TCR，检验克隆对状态多样性的贡献。
4. 以独立 CyTOF 队列验证关键蛋白和状态关系。
5. 比较不同患者、组织来源和乳腺癌亚型的免疫组成。

## 3. 数据规模与图谱组成

### 3.1 三层数据，而不是一个 80,000 细胞的同质队列

| 数据层 | 病例 | 细胞数 | 技术 | 主要用途 |
|---|---:|---:|---|---|
| 主免疫图谱 | BC1–BC8，共 8 例 | 47,016 CD45+ | inDrop scRNA-seq | 83 clusters、跨组织/患者免疫状态 |
| 配对克隆图谱 | BC9–BC11，共 3 例 | 约 27,000 CD3+ | 10x 5′ RNA + V(D)J | TCR 克隆与 T 细胞状态 |
| 蛋白验证 | BC12–BC14，共 3 例 | 6,556 T cells | CyTOF | 验证转录状态对应的蛋白表型 |

这三批患者互不等价，不能简单相加后声称“14 例均有 scRNA+TCR+CyTOF”。论文摘要把主图谱四舍五入称为约 45,000 个细胞，但数据集和正文给出 47,016。

### 3.2 主 atlas 的组成

主图谱覆盖 T 细胞、B 细胞、NK、单核/巨噬细胞、树突状细胞等免疫群，最终解析为 **83 个 cluster**。T 细胞内部并非只有固定的 canonical 亚型；例如多个 effector-memory T cluster 沿激活/分化连续轴排列。髓系细胞也不符合简单 M1/M2 二分，说明离散标签会损失状态信息。

### 3.3 GEO 数据包和文件大小

**GSE114727** 是 SuperSeries，总入口包含两个子系列；其 `RAW.tar` 约 **238.91 MB**，但实际下载时建议直接进入子系列，避免重复或混淆。

**GSE114725：inDrop 主图谱**

- 约 56 个 GEO 样本/文库记录。
- `GSE114725_RAW.tar`：约 **83.25 MB**，分样本原始计数/处理文件集合。
- `GSE114725_rna_raw.csv.gz`：约 **45.53 MB**，主图谱 raw expression/count 表。
- `GSE114725_rna_imputed.csv.gz`：约 **709.42 MB**，插补后的表达表；适合复现作者特定可视化，不宜直接用于差异检验。

**GSE114724：10x 配对 RNA/TCR 队列**

- 约 10 个 GEO 样本/文库记录。
- `GSE114724_RAW.tar`：约 **155.66 MB**，包含按文库组织的补充文件。
- `GSE114724_rna_raw.tsv.gz`：约 **55.22 MB**。
- `GSE114724_rna_imputed.tsv.gz`：约 **57.24 MB**。
- TCR 信息随 RAW/样本补充文件提供，下载后应逐文件检查 clonotype、barcode 与 RNA barcode 的映射，不要只下载 series-level RNA 表就假定已经拿到配对 TCR。

**FR-FCM-ZYJP** 保存 CyTOF/流式数据，适合在 FlowRepository 页面按实验下载 FCS 及元数据。

### 3.4 按分析目的下载

| 目的 | 首选入口 | 注意事项 |
|---|---|---|
| 重建主免疫 atlas | GSE114725 `rna_raw` | 优先 raw；插补矩阵用于可视化而非统计检验 |
| 研究 TCR—状态关系 | GSE114724 RAW 与 RNA | 必须校验同一 barcode 的 RNA/TCR 对应 |
| 复现连续状态图 | raw + 作者方法，或 imputed 表 | imputed 值不能当作独立真实观测 |
| 蛋白层验证 | FR-FCM-ZYJP | 需使用 FCS/CyTOF 分析软件或 R 包 |
| 一次下载整个项目 | GSE114727 | SuperSeries 便于发现，不一定是最清晰的分析入口 |

### 3.5 下载与读取示例

```powershell
Invoke-WebRequest `
  -Uri "https://ftp.ncbi.nlm.nih.gov/geo/series/GSE114nnn/GSE114725/suppl/GSE114725_rna_raw.csv.gz" `
  -OutFile "GSE114725_rna_raw.csv.gz"
```

```python
import pandas as pd
rna = pd.read_csv("GSE114725_rna_raw.csv.gz", index_col=0, compression="gzip")
print(rna.shape)
```

大文件建议用支持断点续传的 `curl.exe -L -C -`。首次读取应查看前几行确认矩阵方向和元数据列；不要默认 CSV/TSV 两个子系列格式完全一致。

## 4. T 细胞状态的定量表达

作者强调 T 细胞状态呈连续变化：同一经典谱系标签内仍存在不同激活、效应和分化程度。BISCUIT 等模型用于处理细胞间技术差异和多模态状态，而 TCR 克隆信息提供了一个自然“重复实验”：同克隆细胞可处于相近或分散状态，从而估计克隆身份与微环境各自的贡献。

## 5. TCR 与表型多样性

独立 10x 5′ 队列能获得配对 V(D)J，分辨率明显高于仅从普通 3′ RNA 数据中猜测 TCR。论文认为 TCR 使用对表型差异有贡献，但同一克隆也可存在状态变动，说明抗原受体并非唯一决定因素，局部环境与分化历史同样重要。

## 6. 主要生物学发现

- 肿瘤免疫细胞具有高度患者特异性，同时共享连续的激活/分化轴。
- T 细胞可解析出多种 effector-memory 和激活状态，离散簇只是连续景观的局部切片。
- TCR 克隆身份可解释部分 T 细胞表型多样性。
- 肿瘤巨噬细胞状态不支持简单 M1/M2 二分。
- CyTOF 在蛋白层面支持关键转录状态，但三种技术来自不同患者批次。

## 7. 对细胞状态导航的启示

如果目标是工程化调节 T 细胞，仅按“cluster label”选择靶点可能过于粗糙。本文提示应把状态看作多维连续坐标，同时测量克隆、组织环境和蛋白表型。理想的导航系统需要实时识别细胞在连续轴上的位置，而不是等待其进入一个人为划定的终末簇。

## 8. 推荐精读图

- 主 atlas 总览与 83 clusters 的图。
- T 细胞连续激活/分化结构图。
- 10x TCR 克隆与转录状态的关联图。
- CyTOF 蛋白验证图。

## 9. 方法学创新

1. 将大规模 droplet atlas、独立配对 TCR 队列和独立 CyTOF 验证并列。
2. 用连续状态模型挑战过度离散化的免疫细胞分类。
3. 将克隆身份作为解释表型变异的定量因素。

## 10. 局限性

- 主图谱、TCR 队列和 CyTOF 队列来自不同患者，不能进行逐细胞三模态配准。
- 不同乳腺癌亚型的样本量很小，亚型差异需谨慎。
- 插补矩阵可能放大相关结构，不宜用于正式差异统计。
- 横断面克隆—状态关联不能证明状态转换方向。
- 论文重点是免疫图谱，不是细胞治疗制造条件的系统优化实验。

## 11. 在综述架构中的位置

适合用于“**quantitatively characterizing cell phenotypes, functions and molecular markers**”，突出离散簇与连续状态轴的互补；也可在“**link cell state/function transitions with molecular drivers**”中说明 TCR、微环境和分化历史共同决定状态。

## 12. 可直接写入综述的表述

> Azizi 等以 47,016 个乳腺肿瘤相关 CD45+ 细胞建立 83-cluster 免疫图谱，并在独立的约 27,000 个 10x 5′ T 细胞及 6,556 个 CyTOF T 细胞中连接转录状态、配对 TCR 与蛋白表型，显示 T 细胞激活和分化更接近连续景观而非少数互斥亚型。

## 13. 避免误读

- 主图谱技术是 inDrop，不是 index sorting。
- 47,016、约 27,000 和 6,556 来自三批不同患者和技术，不能称为每个细胞都有三模态。
- GSE114727 是总入口；具体分析通常应选 GSE114725 或 GSE114724。
- 插补矩阵是模型产物，不是额外测得的分子计数。
