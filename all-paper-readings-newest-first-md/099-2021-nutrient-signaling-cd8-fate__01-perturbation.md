# 《In vivo CRISPR screening reveals nutrient signaling processes underpinning CD8+ T cell fate decisions》精读

## 论文信息

- 作者：Hongling Huang、Peipei Zhou、Jun Wei 等
- 期刊：*Cell*
- 年份：2021；184: 1245–1261.e11
- DOI：10.1016/j.cell.2021.02.021
- 原文：[Cell](https://doi.org/10.1016/j.cell.2021.02.021)
- PubMed：[PMID 33636132](https://pubmed.ncbi.nlm.nih.gov/33636132/)
- 主要数据：[GEO GSE148681](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE148681)；[GEO SuperSeries GSE160341](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE160341)

## 一句话结论

体内 pooled CRISPR 筛选显示，氨基酸转运和营养感知并非只决定 CD8⁺ T 细胞扩增量，还控制效应—记忆命运；其中 Pofut1 将 GDP-fucose 可用性连接到 Notch–Rbpj 信号，促进高增殖中间态向终末效应态转换，而其缺失可保留早期效应/记忆潜力。

## 数据护照（先看这一表）

| 数据集 | 数据类型与比较 | 样本/文件 | 下载提醒 |
|---|---|---|---|
| GSE148681 | Pofut1 扰动及 TINT/TE0/MP 等群体的 Affymetrix Clariom S 转录组 | 28 个样本；RAW CEL TAR 约 35.2 MB | 适合在样本层面重做芯片标准化和 GSEA |
| GSE160218 | NICD vs empty vector 芯片 | 8 个样本；RAW TAR 约 9.6 MB | GSE160341 子系列 |
| GSE160225 | WT/Pofut1-null × ±NICD 芯片 | 16 个样本；RAW TAR 约 20 MB | 用于 Pofut1—Notch 上下游关系 |
| GSE160305 | sgNTC vs sgPofut1、empty vector vs NICD 的 scRNA-seq | 8 个 GEO 样本；RAW TAR 约 692 MB；SRA SRP289470 | GEO TAR 主要为处理后矩阵；原始 reads 走 SRA |
| GSE160313 | Pofut1 KO vs spike 对照 ATAC-seq，第 5 天 | 8 个样本；summarized counts 约 1.7 MB；SRA SRP289484 | 处理后 counts 可快速复核；原始 FASTQ 从 SRA 下载 |
| CRISPR 筛选 | 体内代谢基因库的 sgRNA 丰度和命中结果 | 论文补充数据表 | 不要误认为全部 pooled-screen reads 都在 GEO |

## 1. 研究要解决的问题

CD8⁺ T 细胞激活后同时经历代谢重编程和命运分化，但哪些营养转运、代谢酶或感知通路在体内直接控制效应与记忆命运，传统逐基因实验难以系统回答。本文将代谢定向 CRISPR 库放入 LCMV 特异性 P14 细胞的体内应答中筛选。

## 2. 筛选与验证框架

1. 在 Cas9 P14 CD8⁺ T 细胞中导入代谢相关 lentiviral sgRNA 库。
2. 细胞转入感染宿主后，根据数量和 KLRG1/CD127 等命运表型分选，测定 sgRNA 富集/耗竭。
3. 验证氨基酸转运、mTORC1 和 Pofut1 等命中。
4. 用芯片、scRNA-seq、ATAC-seq、代谢和遗传互作解析 Pofut1—GDP-fucose—Notch–Rbpj 轴。

## 3. 数据内容详解

### 3.1 GSE148681

该系列是 Affymetrix Clariom S 小鼠芯片，共 28 个样本。包含 sgPofut1 与配对 spike/sgNTC 对照在感染后第 5 天和第 7.5 天的比较，并含第 7.5 天分选的 TINT、TE0 和 MP 群体。它可用于区分“Pofut1 缺失效应”与“天然命运群体 signature”。

### 3.2 GSE160341 不是单一实验

GSE160341 是 SuperSeries，共 40 个样本条目，实际由四个子系列组成：

- GSE160218：8 个 NICD/empty-vector 芯片样本；
- GSE160225：16 个 WT/Pofut1-null 与 NICD 组合芯片样本；
- GSE160305：8 个 scRNA-seq 样本，比较 sgPofut1 和 NICD 相关处理；
- GSE160313：8 个 ATAC-seq 样本，比较 Pofut1 KO 与对照。

因此复用时应从子系列下载，而不是把 SuperSeries 的 40 个样本直接拼成同一种矩阵。

### 3.3 组学数据和筛选数据的边界

GEO 保存的是验证阶段的芯片、scRNA-seq 和 ATAC-seq。体内 pooled CRISPR 筛选的库设计、命中和 sgRNA 统计主要位于补充表。若要复刻筛选，应先核对补充数据是否包含每组原始/标准化 sgRNA counts、库序列和动物批次；GEO accession 本身不能替代完整 screen count matrix。

## 4. 数据下载方式

### 4.1 GEO 处理后/芯片原始文件

```text
https://ftp.ncbi.nlm.nih.gov/geo/series/GSE148nnn/GSE148681/suppl/
https://ftp.ncbi.nlm.nih.gov/geo/series/GSE160nnn/GSE160218/suppl/
https://ftp.ncbi.nlm.nih.gov/geo/series/GSE160nnn/GSE160225/suppl/
https://ftp.ncbi.nlm.nih.gov/geo/series/GSE160nnn/GSE160305/suppl/
https://ftp.ncbi.nlm.nih.gov/geo/series/GSE160nnn/GSE160313/suppl/
```

芯片分析下载 RAW TAR 中的 CEL 文件；scRNA-seq 可先下载 GSE160305 RAW TAR（约 692 MB）；ATAC 可先下载 `GSE160313_Pofut1_ATAC_summarizedCounts.tsv.gz`（约 1.7 MB）。

### 4.2 原始测序 reads

GSE160305 对应 SRA SRP289470，GSE160313 对应 SRP289484。可在 Run Selector 导出 accession list，再用：

```bash
prefetch --option-file SraAccList.txt
fasterq-dump --split-files --threads 8 SRRxxxxxxx
```

先保存 GEO 的 Series Matrix/样本注释表，保证 SRR、GSM、处理和生物重复可追溯。

## 5. 主要发现

1. 多个氨基酸转运和营养信号基因影响 CD8⁺ T 细胞数量及命运组成。
2. Pofut1 缺失阻断 TINT 向更终末的 TE0 转换，使早期效应样、高增殖细胞积累。
3. Pofut1 依赖 GDP-fucose 并通过 Notch–Rbpj 调控命运；NICD 可用于定位遗传上下游关系。
4. Pofut1 缺失增强代谢、mTORC1 和 AP-1 相关特征，同时改变染色质可及性。

## 6. TINT 状态的意义

TINT 表现为 CXCR3^hi CD127^lo、处于高增殖和早期效应程序，能继续产生 TE0 或记忆前体。该状态提示“效应功能”与“终末分化”并非同义：细胞可暂时具有较强增殖/效应准备，但尚未被锁定为终末效应。

## 7. 推荐图版

- 体内 CRISPR 筛选设计和命中概览。
- TINT、TE0、MP 的 scRNA/表型和转移验证。
- Pofut1 缺失导致 TINT 积累的图。
- Pofut1—GDP-fucose—Notch–Rbpj 机制图。

## 8. 创新价值

1. 在真实免疫应答环境中系统筛选代谢调控因子。
2. 把营养供给与细胞命运而非仅细胞数量联系起来。
3. 以多组学和遗传互作刻画 Pofut1 机制。

## 9. 局限性

1. 筛选使用特定 P14/LCMV 模型，其他抗原、肿瘤和人类细胞需验证。
2. sgRNA 丰度受编辑效率、转导和细胞回收共同影响。
3. 单细胞样本不能因细胞数多而忽略动物/处理层面的重复。
4. SuperSeries 混合不同技术，直接合并会产生错误矩阵。
5. 公开组学数据不自动包含完整 pooled-screen 原始计数和分析代码。

## 10. 对综述的作用

该文适合作为“体内 CRISPR 筛选发现代谢命运调控因子”的代表，也可支撑“阻断终末效应不等于削弱抗肿瘤功能”的观点。

## 11. 可直接用于综述的观点

> 体内代谢 CRISPR 筛选发现，Pofut1 通过 GDP-fucose—Notch–Rbpj 信号促进高增殖的早期效应中间态向终末效应态转换；其缺失可阻断终末分化并扩展具有记忆潜力的 CD8⁺ T 细胞（Cell 2021, Huang）。

## 12. 数据复用建议

- 用 GSE148681 建立 TE0/TINT/MP signature。
- 用 GSE160305 检查这些 signature 在 Pofut1 或 NICD 扰动后的单细胞分布。
- 用 GSE160313 summarized counts 快速做可及性差异，再决定是否下载 SRA 原始 reads。
- 任何统计都保留样本/动物层级，单细胞分析建议伪 bulk 或混合模型。

## 13. 避免误读

- GSE160341 是四个子系列的总入口，不是一个同质数据表。
- 40 个 SuperSeries 样本包括芯片、scRNA 和 ATAC，不能直接联合归一化。
- Pofut1 缺失导致 TE0 减少，不应简化为“全面抑制效应功能”。
- 补充表中的 CRISPR 结果和 GEO 中的验证组学数据是不同数据层。
