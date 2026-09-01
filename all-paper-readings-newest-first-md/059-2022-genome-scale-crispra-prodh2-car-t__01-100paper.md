# 《A genome-scale gain-of-function CRISPR screen in CD8 T cells identifies proline metabolism as a means to enhance CAR-T therapy》精读

## 论文信息

- **题目**：A genome-scale gain-of-function CRISPR screen in CD8 T cells identifies proline metabolism as a means to enhance CAR-T therapy
- **作者**：Ye et al.
- **期刊与年份**：Cell Metabolism，2022
- **DOI**：[10.1016/j.cmet.2022.02.009](https://doi.org/10.1016/j.cmet.2022.02.009)
- **PMID / PMC**：[35276062](https://pubmed.ncbi.nlm.nih.gov/35276062/) / [PMC8986623](https://pmc.ncbi.nlm.nih.gov/articles/PMC8986623/)
- **数据入口**：[BioProject PRJNA806391](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA806391)、[Metabolomics Workbench PR001324](https://www.metabolomicsworkbench.org/data/DRCCMetadata.php?Mode=Project&ProjectID=PR001324)、[Mendeley Data 10.17632/pnbjdtdkfg.1](https://doi.org/10.17632/pnbjdtdkfg.1)
- **研究类型**：原代小鼠 CD8 T 细胞 genome-scale CRISPR activation 筛选 + 人 CAR-T 靶向 knock-in/过表达验证 + bulk RNA-seq、metabolomics、CyTOF

## 一句话结论

本文建立适用于原代 CD8 T 细胞的双 guide CRISPRa 功能筛选，以肿瘤接触后的 **CD107a 脱颗粒状态** 为读出，从 22,391 个编码转录本中鉴定 26 个高置信增强因子，并证明 **PRODH2 驱动的脯氨酸代谢重编程** 可提高多种 CAR-T 的持续效应与抗肿瘤能力。

## 数据护照（先看这一表）

| 项目 | 内容 |
|---|---|
| 筛选体系 | OT-I;Cas9β 小鼠原代 CD8 T 细胞，CRISPRa dgTKS，与表达 SIINFEKL 的 E0771 肿瘤细胞共培养 |
| library | mm10dgLib；83,601 条 gene-targeting dgRNA，靶向 22,391 个 coding transcripts，另含 1,000 条 NTC；总计 84,601 条 |
| 筛选重复 | 3 个生物学重复 |
| 表型读出 | 共培养后的 CD107a-high top 5% 与 unsorted population |
| 主筛 hit | FDR 0.1% 下 26 个基因 |
| 公开 screen 数据 | 主要位于论文 Dataset S1/Supplementary Data；未发现与 mouse dgTKS 原始 FASTQ 明确一一对应的独立公共 accession |
| bulk RNA-seq | PRJNA806391；6 个 paired-end runs，3 个 PRODH2-CAR 与 3 个 stop-control CAR |
| metabolomics | PR001324，含 ST002084 与 ST002085；两个 10-sample study |
| CyTOF | Mendeley Data，DOI 10.17632/pnbjdtdkfg.1 |
| 关键限制 | SRA accession 对应人 CAR-T bulk RNA-seq，不应写成全基因组筛选原始测序；筛选使用小鼠 OT-I CD8 T 细胞 |

## 1. 研究要解决的问题

许多 CRISPR 筛选通过基因敲除寻找“移除后有利”的靶点，但细胞治疗中同样需要知道：**主动增强哪些基因表达能够让 T 细胞获得更优状态**。原代 T 细胞难以进行 genome-scale gain-of-function 筛选，且传统单 guide CRISPRa 在原代细胞中效率不足。

本文的目标是：

1. 建立兼容原代 CD8 T 细胞和 active Cas9 的 genome-scale CRISPRa 系统；
2. 以真实的肿瘤细胞接触和脱颗粒功能为筛选压力，而非只看增殖；
3. 从 hits 中找到能够工程化到人 CAR-T 的代谢驱动因子；
4. 用转录组、代谢组和单细胞蛋白表型解释功能改善机制。

## 2. 方法框架：dgTKS 双 guide CRISPRa 筛选

### 2.1 CRISPRa 架构

作者开发 TdgA 载体，同时表达双 guide RNA 和 MCP–p65–HSF1 激活模块。该方案让带 active Cas9 的 T 细胞既能利用 guide 定位，也能通过 MS2 激活复合体增强启动子转录。

双 guide 设计的逻辑是：同一 transcript 使用一对靶向转录起始位点附近的 guide，提高激活成功率并降低单 guide 偶然失效。

### 2.2 功能筛选

- 每个生物学重复约 `1.5 × 10^7` 个 OT-I CD8 T 细胞进行转导，转导率约 75%。
- 细胞与表达 SIINFEKL 的 E0771 肿瘤细胞共培养。
- 功能分选阶段每个重复使用约 `3 × 10^6` 个 T 细胞，E:T 为 1:1。
- 以 CD107a 表面暴露作为脱颗粒 readout。
- 分选 top 5% CD107a-high 细胞，并保留约 `2 × 10^6` unsorted cells 作为参照。
- 三个生物学重复中 library 的累计检测覆盖约 93.3%–98.2%。

### 2.3 从筛选 hit 到 CAR-T 工程

作者聚焦 PRODH2，并采用两种人 T 细胞工程方式：

- lentiviral overexpression；
- AAV donor 介导的定点 knock-in，将 CAR/PRODH2 构建整合到 TRAC 等预设位点。

随后在 CD22、HER2、BCMA 等 CAR 体系中检验杀伤、扩增、重复刺激耐受、代谢和体内抗肿瘤能力。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

本文公共数据由四个彼此独立的层次组成：

1. **genome-scale dgTKS 筛选矩阵**：library 设计、各样本 guide count 和 gene-level hit statistics，主要在 Dataset S1。
2. **人 CAR-T bulk RNA-seq**：PRODH2 工程组与 stop-control 组，6 个样本。
3. **人 CD8/CAR-T metabolomics**：两个研究、各 10 个样本，含 targeted/untargeted LC–MS。
4. **CyTOF 表型数据**：工程后 CAR-T 的高维蛋白表型，存于 Mendeley Data。

这些数据不是一个统一的“单细胞转录组 atlas”。CyTOF 是单细胞蛋白层，RNA-seq 是 bulk，代谢组是群体样本，主筛则是 pooled guide abundance。

### 3.2 mm10dgLib 的设计规模

- 靶向 coding transcripts：22,391 个。
- gene-targeting dgRNA：83,601 条。
- non-targeting controls：1,000 条。
- 总 library：84,601 条 dgRNA。
- 理论上平均约 3.7 个 dgRNA pair/transcript。
- 克隆后检测到 82,197/83,601 条 gene-targeting elements（98.3%）和 988/1,000 条 NTC。

论文叙述中有一处将 84,601 与“targeting + controls”的口径写得容易混淆；Methods 中更清楚的组成是 **83,601 targeting + 1,000 NTC = 84,601 total**。报告或综述中建议采用该口径。

### 3.3 主筛样本和 readout 规模

| 层级 | 规模/组成 |
|---|---|
| 生物学重复 | 3 |
| 起始转导细胞 | 约 1.5×10^7 OT-I CD8 T cells/replicate |
| 转导率 | 约 75% |
| 功能共培养 | 约 3×10^6 T cells/replicate；E:T 1:1 |
| 分选群体 | CD107a-high top 5% |
| 对照群体 | 约 2×10^6 unsorted cells/replicate |
| library 元素检测覆盖 | 三个重复累计约 93.3%–98.2% |
| 统计阈值 | FDR 0.1% |
| hit 数 | 26 genes |

在数据分析时，应该把 CD107a-high 与 unsorted 的 guide abundance 配对，而不是把三个重复简单合并后忽略 replicate。CD107a 是短时脱颗粒功能指标，不等同于长期 persistence。

### 3.4 Supplementary Dataset 的组成

PMC 页面提供多个 Supplementary Data 文件：

| 文件 | 大致大小 | 主要用途 |
|---|---:|---|
| Supplement 1 `.xlsb` | 约 16.7 MB | 大型筛选设计/计数数据之一 |
| Supplement 2 `.xlsb` | 约 4.9 MB | 另一组大型筛选或分析矩阵 |
| Supplement 3 `.xlsx` | 约 8.7 MB | 进一步的结果表/验证数据 |
| Supplementary PDF | 约 4.3 MB | 扩展方法与图 |
| Supplement 6 `.xlsx` | 约 112.7 KB | 较小验证表 |
| Supplement 7 `.xlsx` | 约 13.1 KB | 较小资源/引物表 |

其中 Dataset S1 是重分析主筛时最重要的入口，因为它包含 library 信息和 per-guide abundance。`.xlsb` 不能由所有在线表格工具稳定读取，建议使用桌面 Excel，或在 Python 中用支持 xlsb 的库读取并立即导出为 TSV/Parquet。

### 3.5 bulk RNA-seq：PRJNA806391

- 物种：Homo sapiens。
- 数据类型：paired-end bulk RNA-seq。
- 样本数：6。
- 设计：3 个 CD22CAR–PRODH2 样本，3 个 CD22CAR–PRODH2(stop) control。
- 数据入口：[PRJNA806391](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA806391)。
- 6 个 runs 合计约 8.33 亿 read records/paired observations（不同数据库对 paired reads 的计数口径可能不同，下载时应以 Run Selector 的 `spots`、`bases` 与 FASTQ bytes 为准）。

这里最重要的边界是：**PRJNA806391 对应人 CAR-T 的机制验证 bulk RNA-seq，不是 mouse genome-scale dgTKS screen 的原始 FASTQ。**

### 3.6 metabolomics：PR001324

Metabolomics Workbench 项目 [PR001324](https://www.metabolomicsworkbench.org/data/DRCCMetadata.php?Mode=Project&ProjectID=PR001324) 包含两个 study：

#### ST002084：靶向 knock-in CAR-T

- 样本数：10。
- 5 个 PRODH2-CAR，5 个 PRODH2(stop)-CAR。
- 平台/方法：Agilent 6490 triple quadrupole LC–MS targeted metabolomics。
- 可用文件：study metadata、mwTab、JSON、named metabolite table 以及 raw data archive，具体以 study 页 Download 区为准。

#### ST002085：lentiviral overexpression CD8 T cells

- 样本数：10。
- 5 个 PRODH2 overexpression，5 个 vector control。
- 同时包含 targeted triple-quadrupole 和 untargeted Agilent 6550 QTOF 数据。
- 适合分析脯氨酸相关代谢物、TCA/氧化还原和整体代谢重编程。

两个 study 的工程方式不同，不能把 20 个样本当作一个同质 cohort 直接做组间检验。应先分别分析，再比较共同改变的 pathway。

### 3.7 CyTOF：Mendeley Data

- 数据集标题：CyTOF raw data。
- DOI：[10.17632/pnbjdtdkfg.1](https://doi.org/10.17632/pnbjdtdkfg.1)。
- 许可：页面标注 CC BY 4.0。
- 下载：在数据集页面使用 **Download All**，或逐文件下载。
- 实验设计：论文中 CyTOF 验证通常为每组 3 个重复、每个重复约 `1.5 × 10^6` 细胞起始；具体 FCS 文件与样本名应以下载后的 manifest 为准。

CyTOF 数据应保留原始 FCS、panel/marker mapping、batch 和 condition 信息。若仅拿论文中的二维图，无法重建细胞状态簇。

### 3.8 下载方式

#### RNA-seq FASTQ

从 BioProject 进入 SRA Run Selector，导出 accession list：

```bash
prefetch SRR_ACCESSION
fasterq-dump SRR_ACCESSION --split-files --threads 8
gzip SRR_ACCESSION_1.fastq SRR_ACCESSION_2.fastq
```

也可在 ENA Browser 中按 BioProject 查询，使用 `fastq_ftp` 字段批量下载。下载后应核对 3 vs 3 的 sample title，不要只按 SRR 排序猜测分组。

#### Metabolomics Workbench

```text
Project: https://www.metabolomicsworkbench.org/data/DRCCMetadata.php?Mode=Project&ProjectID=PR001324
Study IDs: ST002084, ST002085
```

进入 study 页面后分别下载 metadata、mwTab/JSON、processed named metabolites 和 raw archive。建议同时保存网页导出的 study design，因为 raw 文件名不一定包含完整分组。

#### CyTOF

```text
https://doi.org/10.17632/pnbjdtdkfg.1
```

#### 筛选矩阵

从 [PMC article](https://pmc.ncbi.nlm.nih.gov/articles/PMC8986623/) 的 **Supplementary Materials** 下载 Dataset S1 等文件。对于 `.xlsb`：

```python
import pandas as pd
df = pd.read_excel("Dataset_S1.xlsb", engine="pyxlsb", sheet_name="...")
df.to_csv("Dataset_S1.tsv.gz", sep="\t", index=False)
```

实际 sheet 名需要先列出，不应假设第一张表就是 guide count。

### 3.9 下载后建议整理

```text
62_Ye_2022/
├── screen/
│   ├── mm10dgLib_annotation/
│   ├── per_guide_counts/
│   └── gene_level_hits/
├── bulk_rnaseq/PRJNA806391/
├── metabolomics/
│   ├── ST002084/
│   └── ST002085/
├── cytof/pnbjdtdkfg_v1/
├── constructs_and_sequences/
└── metadata/sample_manifest.tsv
```

## 4. 主要生物学发现

### 4.1 gain-of-function 筛选发现“添加什么”

与 knockout screen 不同，CRISPRa 直接寻找表达增强后提高脱颗粒的基因，因而更接近细胞治疗中可实施的 overexpression/knock-in 模块。

### 4.2 PRODH2 将代谢状态连接到长期功能

PRODH2 促进脯氨酸分解代谢和线粒体相关代谢重编程。其效果不仅体现在短时 CD107a，还在重复刺激、持久扩增和体内肿瘤控制中得到验证。

### 4.3 工程效果跨多个 CAR 体系

作者在 CD22、HER2、BCMA 等不同 CAR 背景中验证，说明 PRODH2 不完全依赖单一抗原受体。但不同靶点、肿瘤模型和工程方式的效应量仍有差异。

## 5. 状态—功能—驱动因子的连接

```text
PRODH2 expression ↑
→ proline catabolism / mitochondrial program 改变
→ transcriptional and metabolic state 改变
→ cytotoxic degranulation、serial stimulation fitness ↑
→ CAR-T persistence and tumor control ↑
```

本文的优势是同时测量了功能筛选、bulk transcription、metabolites 和单细胞蛋白表型，形成跨层证据；不足是没有同一细胞的多组学配对，因此这些层之间主要通过条件对比而非单细胞联配建立联系。

## 6. 对细胞治疗状态导航的启示

- gain-of-function screen 可直接发现“可装入细胞产品”的功能模块。
- 以肿瘤接触后的 CD107a 作为筛选 readout，可将工程选择与真实效应功能对齐。
- 代谢模块可以与 CAR/TCR 模块组合，而非仅调整培养基。
- 对候选优化应同时考察短时杀伤和长期重复刺激，避免只优化瞬时效应态。

## 7. 可复用的分析思路

1. 对 dgRNA 先做 library representation 和 pair completeness 质控。
2. 分别报告 CD107a-high 与 unsorted 的 guide count、fold change、replicate consistency。
3. 用多种 CAR 架构和两种表达方式验证，以区分靶点效应与载体效应。
4. RNA、metabolomics、CyTOF 各自以样本为统计单位，再做 pathway-level convergence。
5. 长期刺激数据应建模时间，而不是只比较末次时间点。

## 8. 推荐图版

- genome-scale dgTKS 工作流程和 CD107a 分选图。
- 26 个 hit 的排序/富集图，突出 PRODH2。
- PRODH2 CAR-T 重复刺激和体内肿瘤控制结果。
- 代谢组 pathway 与线粒体功能图。
- CyTOF 状态分布图，用于说明高效应不必等同于快速终末耗竭。

综述中可重绘为“CRISPRa → CD107a screen → PRODH2 → multi-omics → CAR-T function”的闭环证据图。

## 9. 创新价值

- 建立原代 CD8 T 细胞 genome-scale gain-of-function 筛选框架。
- 双 guide CRISPRa 提高原代细胞中激活筛选的可行性。
- 从 genome-scale hit 直接走到人 CAR-T 工程和多抗原验证。
- 将可干预代谢驱动因子与长期细胞治疗性能连接。

## 10. 局限性

1. 筛选在小鼠 OT-I 模型完成，人 CAR-T 为后续验证，存在物种和受体背景差异。
2. CD107a top 5% 偏向瞬时脱颗粒，可能漏掉促进长期记忆或存活但短时脱颗粒不高的因子。
3. 主筛原始 FASTQ 没有明确、独立的公共 accession；可复现性主要依赖补充 count 矩阵。
4. bulk RNA-seq 只有 3 vs 3，难以解析亚群异质性。
5. 代谢组的两个 study 使用不同工程方式，应避免简单合并。
6. PRODH2 过表达的临床安全窗、代谢副作用和不同患者来源细胞中的稳定性仍需评估。

## 11. 对本章节的作用

| 综述模块 | 本文贡献 |
|---|---|
| Quantitatively characterizing phenotypes | 以 CD107a-high 极端群体定量细胞毒脱颗粒状态 |
| Techniques to perturb cell states | 原代 CD8 T 细胞 genome-scale CRISPRa；后续 overexpression/knock-in |
| Link transitions with drivers | PRODH2—脯氨酸代谢—线粒体/效应功能链条 |
| Optimize navigation conditions | 指出代谢工程与重复刺激条件需联合优化 |
| Real-time optimization systems | CD107a 流式 readout 可作为快速功能传感器，但实验仍是离线 pooled selection |

## 12. 可直接用于综述的观点

> Genome-scale CRISPR activation complements knockout screening by identifying gene programs that can be directly installed into therapeutic T cells.

> A function-linked screen can reveal metabolic drivers such as PRODH2 that improve not only acute degranulation but also the fitness of CAR T cells under repeated antigen challenge.

> For state engineering, transcriptomic, metabolic and protein-level measurements should be interpreted as distinct but convergent views rather than merged as a single atlas.

## 13. 避免误读

- **84,601 是总 dgRNA 数**：更清楚的构成为 83,601 targeting + 1,000 NTC。
- **PRJNA806391 不是 mouse screen raw sequencing**：它是人 CAR-T PRODH2 vs stop control 的 6-sample bulk RNA-seq。
- **本文没有 scRNA-seq atlas**：单细胞层数据主要是 CyTOF，而 RNA-seq 是 bulk。
- **26 个 hit 是严格 FDR 下的功能筛选结果**，不表示其他基因完全无效。
- **CD107a 是脱颗粒 proxy**，不是直接的长期肿瘤清除或记忆状态。
- **两个 metabolomics study 不应直接合并**，因为工程方式和实验批次不同。

## 数据与资源链接

- 论文全文：[PMC8986623](https://pmc.ncbi.nlm.nih.gov/articles/PMC8986623/)
- RNA-seq：[PRJNA806391](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA806391)
- Metabolomics：[PR001324](https://www.metabolomicsworkbench.org/data/DRCCMetadata.php?Mode=Project&ProjectID=PR001324)
- CyTOF：[Mendeley Data](https://doi.org/10.17632/pnbjdtdkfg.1)
