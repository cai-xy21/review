# 《C-JUN overexpressing CAR-T cells in acute myeloid leukemia: preclinical characterization and phase I trial》精读

## 论文信息

- **作者**：Shiyu Zuo, Chuo Li, Xiaolei Sun 等
- **期刊与年份**：*Nature Communications*, 2024
- **DOI**：10.1038/s41467-024-50485-9
- **本地原文**：[PDF](<D:/research/review/perturbation33references/15-C-JUN overexpressing CAR-T cells in acute myeloid leukemia preclinical characterization and phase I trial.pdf>)
- **数据入口**：[SRA BioProject PRJNA1132480](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1132480)；临床试验 NCT04835519

## 一句话结论

C-JUN 过表达可缓解 AML CAR-T 的功能衰减并增强临床前效力；但早期 I 期试验仅治疗 4 例患者且因严重毒性/感染事件暂停，说明增强效力与临床安全必须同时评价。

## 数据护照

| 项目 | 内容 |
|---|---|
| 临床前体系 | CD33 CAR-T、C-JUN 过表达 CAR-T；AML/ALL 细胞系与原代样本 |
| 公开测序项目 | SRA BioProject PRJNA1132480 |
| 主要测序类型 | bulk RNA-seq、ATAC-seq；以临床前样本为主 |
| 已明确的 CAR-T RNA-seq核心比较 | 2 位供体 × 2 条件，两个独立实验模块；每个样本由技术孔合并 |
| 临床 | 7 例筛选、4 例入组并接受治疗；NCT04835519 |
| 数据治理 | 患者层扩展数据可能受隐私限制；论文提供去标识化结果和 Source Data |

## 1. 研究问题

AML 抗原异质性、抑制性配体和持续刺激会限制 CAR-T 功能。本文检验转录因子 C-JUN 过表达能否增强抗 AML CAR-T 的耐受持续刺激能力，并把临床前方案推进到首次人体探索。

## 2. 实验设计与方法框架

作者构建 C-JUN 过表达 CAR-T，与常规 CAR-T 比较细胞因子、杀伤、分化/耗竭表型、转录组、染色质开放性和异种移植疗效；同时调查 AML/ALL 模型的抑制性配体背景。随后开展单中心、剂量探索性质的 I 期研究。

## 3. 数据规模与图谱组成

### 3.1 数据的两部分

本文由“临床前多组学”与“小样本早期临床”两部分组成。PRJNA1132480 是测序项目入口，但临床患者数量和 SRA run 数是不同概念；不能用 BioProject 的 run 数替代入组人数。

### 3.2 CAR-T bulk RNA-seq 组成

论文方法可明确还原两组核心转录实验：

1. **不同靶细胞背景实验**：CD33 CAR-T 与 U937-CD33 或 Nalm6-CD33 共培养 2 天，分选 CD8+CAR+ 细胞；**2 位供体 × 2 个靶细胞条件 = 4 个生物学样本**。每个 RNA 样本由 3 个技术重复孔合并，合并孔不能按 3 个独立重复计数。
2. **C-JUN 干预实验**：常规 CAR-T 与 C-JUN-CAR-T 经 U937 刺激后比较；**2 位供体 × 2 个构建 = 4 个生物学样本**，同样由技术孔合并。

此外，作者对若干 AML/ALL 细胞系及原代材料进行了转录测量，用于观察抑制性配体背景。具体 accession 与样本标题应以 SRA Run Selector 导出的 `RunInfo.csv` 为准；论文正文不足以可靠给出 BioProject 的总 run 数，因此本报告不虚构一个总数。

### 3.3 ATAC-seq 组成

ATAC-seq用于比较常规与 C-JUN 过表达 CAR-T 的染色质开放状态，论文方法描述了 HiSeq、150 bp paired-end 测序。其样本与供体映射需要在 PRJNA1132480 的 Run Selector 中根据 `LibraryStrategy=ATAC-seq`、sample title 和 BioSample 核验。分析时应与 RNA-seq 分开建表，不能仅凭相同构建名默认一一配对。

### 3.4 临床队列组成

- 共 **7 例患者接受筛选**，其中 **4 例入组并接受细胞输注**。
- 治疗者为 2 男、2 女，年龄中位数 9.5 岁（范围 3–12 岁）。
- 报告剂量为约 **0.5 × 10^6 CAR-T/kg（允许 ±20%）**。
- 试验在发生 4 级剂量限制性毒性/严重感染事件后暂停。

这 4 例适合描述可行性、早期信号和毒性个案，不足以估计稳定的有效率或做亚组统计。

### 3.5 数据下载方式

1. 打开 PRJNA1132480，进入 **SRA Run Selector**。
2. 导出 `RunInfo.csv`、`SraRunTable.txt` 和 accession list；按 `Assay Type/LibraryStrategy`、BioSample、供体、细胞构建、靶细胞、刺激时间分层。
3. 用 SRA Toolkit 下载：

```bash
prefetch --option-file SraAccList.txt
fasterq-dump --split-files --threads 8 --outdir fastq --option-file SraAccList.txt
```

4. 先检查 paired-end 命名和测序策略，再分别走 RNA-seq 与 ATAC-seq流程。
5. 论文 Source Data（约 67 KB）及 Supplementary Information（约 21.8 MB）包含功能/临床表格，应从期刊页面另行下载；它们不在 SRA FASTQ 中。

### 3.6 下载后的样本表

建议字段包括 `assay`、`biosample`、`donor`、`construct`、`target_cell`、`stimulation_days`、`technical_pooling`、`clinical_or_preclinical`。对总 run 数和每项实验的精确映射，应把 `RunInfo.csv` 与论文方法逐项对照后再报告。

## 4. 主要结果

C-JUN 过表达提高 CAR-T 在抑制性 AML 环境和持续刺激下的功能，伴随转录及染色质状态改变，并改善临床前肿瘤控制。临床部分显示产品能够制备并输注，但样本极小且出现严重安全事件。

## 5. 机制理解

C-JUN/AP-1 活性可与耗竭相关转录网络竞争或重塑其占位，帮助维持效应基因和染色质可及性。本文的 RNA-seq 与 ATAC-seq为此提供关联证据，但临床结局还受感染、疾病负荷和制造差异等因素影响。

## 6. 推荐重点阅读的图

- 常规与 C-JUN CAR-T 在重复刺激后的功能、耗竭表型图。
- RNA-seq/ATAC-seq 的通路与开放染色质比较。
- AML 异种移植中的肿瘤负荷和生存图。
- 4 例患者的时间线、扩增/缓解与不良事件图表。

## 7. 创新性

把 C-JUN 装甲化从多模型临床前机制推进到 AML 早期临床，并同时公开 RNA-seq 与 ATAC-seq原始数据，为效力机制复核提供基础。

## 8. 局限性

临床仅 4 例、无对照；年龄与疾病背景高度特异；临床前组学供体数小；SRA 项目没有直接提供一个整合的处理后表达/可及性矩阵。严重毒性使疗效信号不能脱离安全背景解读。

## 9. 在综述中的定位

适合作为“转录因子装甲化 CAR-T 从机制到首次人体”的案例，同时也是说明临床转化中效力增强不等于净获益的警示性研究。

## 10. 可直接写入综述的表述

> C-JUN 过表达通过维持效应转录与染色质状态增强抗 AML CAR-T 的临床前功能，但仅 4 例的 I 期探索及严重毒性事件提示，其临床价值仍需在更严格的安全框架中验证。

## 11. 数据复用建议

先用 Run Selector 建立可审计的 RNA/ATAC 样本映射，再做供体配对的 C-JUN 效应分析；可将差异基因与差异开放区通过附近基因或 motif 连接，检验 AP-1 与耗竭 TF 网络的变化。临床数据只宜作病例级描述。

## 12. 转化与安全性关注

应重点关注感染、骨髓抑制/靶向正常髓系组织、细胞因子相关毒性和长期扩增。任何后续方案都需要独立安全监测、明确停药规则和更充分的剂量探索。

## 13. 避免误读

- **4 是实际治疗患者数，不是测序样本数；BioProject run 也不等于患者数。**
- 技术孔合并不能被计为 3 个生物学重复。
- 本报告未在缺少 RunInfo 映射时杜撰 PRJNA1132480 的总 run 数。
- 早期疗效个案不能被概括为已证明临床有效。

