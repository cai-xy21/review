# 《Hematopoietic Progenitor Kinase 1 (HPK1) Mediates T Cell Dysfunction and Is a Druggable Target for T Cell-Based Immunotherapies》精读

## 论文信息

- 作者：Jingwen Si、Xiangjun Shi、Shuhao Sun 等
- 期刊：*Cancer Cell*
- 年份：2020；38: 551–566.e11
- DOI：10.1016/j.ccell.2020.08.001
- 原文：[Cancer Cell](https://doi.org/10.1016/j.ccell.2020.08.001)
- PubMed：[PMID 32860752](https://pubmed.ncbi.nlm.nih.gov/32860752/)
- ATAC-seq：[GEO GSE150635](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE150635)
- RNA-seq：[GEO GSE156204](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE156204)
- 原始图数据：[Mendeley Data 10.17632/g774vrm3pk.1](https://doi.org/10.17632/g774vrm3pk.1)

## 一句话结论

HPK1/MAP4K1 通过 TCR 近端信号与 NF-κB–Blimp1 轴促进肿瘤浸润 T 细胞功能障碍；敲除 MAP4K1、抑制其激酶活性或用 PROTAC 降解 HPK1 均可增强 T 细胞和 CAR-T 抗肿瘤效应。

## 数据护照（先看这一表）

| 数据集 | 内容 | 样本/文件 | 分析提醒 |
|---|---|---|---|
| GSE150635 | B16 肿瘤特异性 T 细胞 WT vs HPK1 KO ATAC-seq | 5 个样本：WT 3、KO 2；processed bigWig RAW TAR 约 574.4 MB；SRA SRP261799 | 不平衡小样本，KO 只有 2 个生物重复 |
| GSE156204 | 第 21 天 B16 肿瘤浸润 T 细胞 WT vs HPK1 KO RNA-seq | 4 个样本：WT 2、KO 2；processed XLSX 约 1.0 MB；SRA SRP277417 | 极小 n，差异表达和富集需谨慎 |
| Mendeley Data | 论文 Original data/source data | version 1，DOI `g774vrm3pk.1` | 下载前检查文件清单；它不是 GEO 原始 FASTQ 的替代品 |
| 既往表达 signature | GSE9650、GSE24026、GSE27670、GSE5960 等用于 GSEA/比较 | 外部公共数据 | 属于复用 signature，不是本文新实验 |
| 功能数据 | MAP4K1 KO CAR-T、抑制剂、PROTAC、人多发性骨髓瘤模型 | 正文/补充/Mendeley source data | 不应写成全部在 GEO 中 |

## 1. 研究要解决的问题

HPK1 是造血细胞富集的 TCR 信号负调控激酶。本文问两个问题：HPK1 是否参与肿瘤环境中的 T 细胞耗竭/功能障碍；以及它能否通过基因编辑、激酶抑制或蛋白降解转化为 ACT 靶点。

## 2. 实验与分析框架

1. 分析人肿瘤和多发性骨髓瘤样本中 HPK1 与抑制受体/预后的关系。
2. 在 Map4k1 KO 小鼠 B16 模型中评估肿瘤生长、TIL 表型和功能。
3. 用 RNA-seq、ATAC-seq、IP-MS 和报告系统解析 NF-κB–Blimp1 机制。
4. 构建 MAP4K1 KO CD19/BCMA CAR-T，与 PDCD1 KO 比较。
5. 开发 HPK1 小分子抑制剂和 PROTAC，验证可药物性。

## 3. 数据内容详解

### 3.1 GSE150635：ATAC-seq

GEO 记录 5 个小鼠样本，WT 3 个、Map4k1 KO 2 个。处理后文件为每样本 bigWig 汇总 TAR，适合浏览关键基因座和重画平均 signal；若要统一 peak calling 和 differential accessibility，需要从 SRP261799 下载原始 reads。

### 3.2 GSE156204：RNA-seq

4 个样本来自第 21 天 B16 肿瘤浸润 T 细胞，WT 和 KO 各 2 个。GEO 提供约 1 MB 的 XLSX 处理后表。该设计可支持方向性和通路级复核，但对单基因显著性、离群值和批次非常敏感。

### 3.3 Mendeley 原始图数据

论文另以 Mendeley Data DOI 提供 Original data。它更可能对应图中定量/source data、成像或实验表格；应在页面逐文件查看后按需要下载。GEO 负责测序数据，Mendeley 负责论文 source data，两者功能不同。

## 4. 数据下载方式

### 4.1 GEO 处理后文件

```text
https://ftp.ncbi.nlm.nih.gov/geo/series/GSE150nnn/GSE150635/suppl/
https://ftp.ncbi.nlm.nih.gov/geo/series/GSE156nnn/GSE156204/suppl/
```

ATAC bigWig TAR 约 574.4 MB；RNA processed XLSX 约 1.0 MB。先下载 GEO SOFT/Series Matrix 元数据，建立 GSM—基因型—重复对应表。

### 4.2 原始 reads

GSE150635 对应 BioProject PRJNA633050 / SRP261799，GSE156204 对应 PRJNA657044 / SRP277417：

```bash
prefetch --option-file SraAccList.txt
fasterq-dump --split-files --threads 8 SRRxxxxxxx
```

### 4.3 Mendeley

打开 DOI 页面选择 version 1 并逐文件下载。引用时同时记录数据集 DOI 和版本，避免后续版本变化导致 source data 不一致。

## 5. 主要发现

1. HPK1 高表达与 T 细胞抑制受体、功能障碍和不良肿瘤免疫状态相关。
2. Map4k1 缺失减轻 B16 TIL 的耗竭/anergy/AICD 相关程序并增强功能。
3. HPK1 通过 NF-κB–Blimp1 轴促进功能障碍。
4. MAP4K1 KO CAR-T 的疗效优于对照，并在部分模型中强于 PDCD1 KO。
5. 激酶抑制与 PROTAC 结果说明 HPK1 可用多种方式药物化。

## 6. 机制与策略比较

永久 MAP4K1 KO、激酶抑制和 PROTAC 降解不是等价干预：KO 去除所有蛋白功能，激酶抑制保留 scaffold，PROTAC 则降解整蛋白且依赖 E3/细胞内药代。三者方向一致可加强靶点可信度，但剂量、安全性和脱靶需分别评价。

## 7. 推荐图版

- HPK1 与 TIL dysfunction 的人/鼠相关图。
- NF-κB–Blimp1 机制图。
- MAP4K1 KO 与 PDCD1 KO CAR-T 对照图。
- 小分子抑制剂/PROTAC 疗效图。

## 8. 创新价值

1. 将 HPK1 从 TCR 负调控因子推进到耗竭机制和 ACT 靶点。
2. 同时覆盖遗传敲除、激酶抑制和蛋白降解。
3. RNA 与 ATAC 提供转录/染色质层证据。

## 9. 局限性

1. RNA-seq 2 vs 2、ATAC 3 vs 2，统计功效有限。
2. 小鼠全身 Map4k1 KO 会影响多种造血细胞，不能全部归为 T 细胞内在效应。
3. 人体相关分析和小鼠机制之间仍有物种/模型差异。
4. PROTAC 和抑制剂的选择性、药代及临床安全窗口尚未建立。
5. 不同数据层分散在 GEO、Mendeley 和补充材料，复现需手工整理。

## 10. 对综述的作用

适合“解除 TCR 负反馈以增强 ACT”和“可药物耗竭调控激酶”部分。可与 p38 论文对照：p38 主要强调制造期多表型质量，HPK1 更强调 TCR 近端抑制和多种药物策略。

## 11. 可直接用于综述的观点

> HPK1/MAP4K1 通过 NF-κB–Blimp1 轴加强肿瘤浸润 T 细胞功能障碍；基因敲除、激酶抑制和 PROTAC 降解均可解除这一负反馈并提高 CAR-T/内源性抗肿瘤免疫（Cancer Cell 2020, Si）。

## 12. 数据复用建议

- RNA/ATAC 以通路和效应方向为主，避免对极小样本做过强单基因结论。
- ATAC 原始重分析需重新 peak calling，并在样本层面统计。
- 将 Mendeley source data 与相应图号建立映射表。
- 外部 GEO signature 单独标注“reused”，不纳入新数据样本量。

## 13. 避免误读

- GSE150635 只有 5 个 ATAC 样本，GSE156204 只有 4 个 RNA 样本。
- 不要把全身 KO 的肿瘤效应全部归因于 CD8 T 细胞。
- 不要把三种 HPK1 干预方式视为机制和安全性完全相同。
- Mendeley Original data 不等于完整原始测序 reads。
