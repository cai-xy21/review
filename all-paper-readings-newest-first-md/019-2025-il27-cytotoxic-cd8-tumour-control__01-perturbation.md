# 《IL-27 elicits a cytotoxic CD8+ T cell program to enforce tumour control》精读

## 论文信息

- **作者**：Béatrice Bréart、Katherine Williams、Stellanie Krimm 等
- **期刊与年份**：*Nature*, 2025；639: 746–755
- **DOI**：10.1038/s41586-024-08510-w
- **本地原文**：[PDF](<D:/research/review/perturbation33references/25-IL-27 elicits a cytotoxic CD8+ T cell program to enforce tumour control.pdf>)
- **新测小鼠数据**：[E-MTAB-14580](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-14580)、[E-MTAB-14581](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-14581)、[E-MTAB-14584](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-14584)、[E-MTAB-14590](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-14590)、[E-MTAB-14601](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-14601)
- **处理数据与代码**：[OSF f6re4](https://osf.io/f6re4/)
- **人 T 细胞受控数据**：EGA `EGAS50000000694`

## 一句话结论

IL-27 直接作用于肿瘤特异 CD8⁺ T 细胞，维持其存活并诱导高 GZMB 的细胞毒程序；系统性 IL-27R 激动在小鼠中可耐受、能使既有肿瘤退缩并与 PD-L1 阻断协同，其肿瘤内 IL27/EBI3 表达还与多个人群 ICB 队列的较好结局相关。

## 数据护照

| accession | 数据层 | 样本/assay 数 | 图谱组成 |
|---|---|---:|---|
| E-MTAB-14580 | OT-I CD8 bulk RNA-seq | 8 | 重复肽刺激 ± IL-27 |
| E-MTAB-14584 | OT-I CD8 bulk ATAC-seq | 8 | 与 14580 同类条件 |
| E-MTAB-14581 | MC38 CD4 bulk RNA-seq | 36 | tumour/dLN × Tconv/Treg × 治疗 |
| E-MTAB-14590 | MC38 CD45⁺/T 细胞 scRNA/TCR | 7 | 3 个 CD45⁺ + 4 个 T 细胞样本 |
| E-MTAB-14601 | M86 tetramer⁺ CD8 scRNA/TCR/feature barcode | 11 个样本、55 assays | tumour/dLN、IL-27激动或阻断等；5类 assay映射 |
| **新测小鼠归档合计** |  | **70 个生物样本** | assay 总数高于样本数 |
| 人 bulk RNA-seq | 受控 | 未公开样本级细节 | EGAS50000000694 |

## 1. 研究问题

细胞因子可增强 CTL，但系统给药常伴随炎症毒性。作者从人和鼠肿瘤的表达关联出发，测试 IL-27 是否能在不过度炎症的前提下，直接维持肿瘤特异 CTL 的细胞毒性和持久性。

## 2. 实验设计与方法框架

研究结合 TCGA/临床试验队列再分析、MC38 等小鼠肿瘤模型、诱导性 IL-27 过表达与长半衰期蛋白治疗、OT-I 重复刺激、人 CMV 特异 CTL，以及 bulk RNA、ATAC、scRNA、TCR 和 feature-barcode 多层数据。

## 3. 数据规模与图谱组成

### 3.1 五个新测小鼠 accession 的关系

五个 E-MTAB 不是一个大图谱的拆分，而是回答不同问题：OT-I 体外机制（RNA/ATAC）、MC38 全免疫生态（scRNA）、M86 肿瘤特异 CTL（scRNA/TCR）及 CD4/Treg 响应（bulk RNA）。合并描述时应报告 **70 个生物样本**，但分析时分别处理。

### 3.2 OT-I 重复刺激：8 RNA + 8 ATAC

E-MTAB-14580 和 E-MTAB-14584 均使用小鼠 OT-I CD8⁺ T 细胞，连续加入 OVA257–264 肽 4–6 天，并比较是否加入 25 ng/ml IL-27。每套 8 个样本。

- RNA-seq：TruSeq stranded mRNA，NovaSeq 6000，约 3,000 万 single-end 50-bp reads/sample；
- ATAC-seq：paired-end 50-bp；
- 设计目的：把 IL-27 引起的转录程序与染色质开放变化对应起来。

不要把 8+8 自动视为同一样本的技术配对；需要从 SDRF 中核对 sample identifier。

### 3.3 CD4/Treg bulk 图谱：E-MTAB-14581，36 个样本

从 MC38 荷瘤小鼠肿瘤和引流淋巴结分选 CD4⁺ conventional T 与 GFP⁺ Treg，比较 IL-27R 激动相关条件。每个样本分选约 2,000–33,000 个细胞，Smart-seq V4/Nextera XT 建库，NovaSeq 6000，每样本约 3,000 万 single-end 50-bp reads。

这一模块主要用于安全性和生态解释：IL-27 的抗肿瘤作用是否以 Treg/常规 CD4 的广泛异常为代价。

### 3.4 MC38 全免疫单细胞：E-MTAB-14590，7 个样本

- 3 只小鼠分选 live CD45⁺ 肿瘤免疫细胞；
- 4 只小鼠额外分选 live CD45⁺TCRβ⁺ T 细胞；
- 每样本装载 20,000 个细胞，目标回收 2,000–10,000；
- 10x 5′ GEX 与 TCR 文库，GEX 约 2 亿 paired reads/library，TCR 约 4,000–5,000 万。

这 7 个样本包括不同富集策略，T 细胞比例不能不经校正直接比较。

### 3.5 肿瘤特异 CTL 图谱：E-MTAB-14601，11 个样本、55 assays

作者从 MC38 肿瘤与引流淋巴结分选 M86 neoantigen tetramer⁺ CD8⁺ T 细胞，并以 hashtag 标记重复。数据库报告 11 个生物样本、55 assays；后者来自同一样本的 GEX、TCR、feature barcode/hashtag/tetramer 等文库或数据链路，不是 55 只小鼠。

每样本计划装载 20,000 个细胞、目标回收 2,000–10,000；GEX 约 2 亿 reads，TCR 约 4,000–5,000 万，feature barcode 约 1,500–2,000 万。该模块是论文判断 IL-27 如何改变肿瘤特异 CTL 状态和克隆结构的核心。

### 3.6 人临床与外部数据

| 数据 | 用途 | 访问属性 |
|---|---|---|
| TCGA / UCSC Xena | 泛癌 IL27/EBI3 与 CTL signature 关联 | 公开处理数据 |
| GSE91061 | nivolumab 治疗黑色素瘤 | 公开再分析 |
| GSE168846 | 未治疗小鼠同系肿瘤 | 公开再分析 |
| GSE199045 | 静息小鼠 CD8⁺ T 细胞 | 公开再分析 |
| IMvigor210/211、OAK | atezolizumab/化疗结局分析 | EGA：EGAS00001002556、EGAD00001007703、EGAD00001008391、EGAS50000000497 |
| 人 T 细胞 bulk | 本文新测 | EGA EGAS50000000694，需申请 |

临床图中的 n 值依终点和可用表达数据变化，例如 OAK/IMvigor 生存分层不能简单相加为一个统一队列。

### 3.7 推荐下载方式

1. 在 BioStudies 各 accession 页面先下载 `IDF` 与 `SDRF`，获得样本—条件—ENA run 映射。
2. bulk 分析先从 ENA/作者 OSF 获取 count/processed objects；需要重比对时再下载 FASTQ。
3. 单细胞先用 OSF 处理对象和代码；若重跑 Cell Ranger，再按 ENA run 下载 GEX/TCR/feature barcode。
4. 人临床数据按 accession 单独申请 EGA，不要预期通过 ArrayExpress 获得。

## 4. 主要结果

IL-27 与人和鼠肿瘤中的 CTL signature 正相关。IL-27R 信号直接增强肿瘤特异 CTL 的 GZMB、存活和持久性；长半衰期 IL-27 或诱导表达可使既有肿瘤回缩，并与 PD-L1 阻断协同。高 IL27/EBI3 在部分 atezolizumab 队列中关联更好结局。

## 5. 机制理解

IL-27 在本研究中不是简单“促炎”或“抑炎”标签，而是情境依赖地强化持续抗原刺激下 CTL 的细胞毒输出，同时保留足够适应性。其效果依赖 CTL 直接感知，而非仅通过 APC 或 Treg 间接发生。

## 6. 推荐重点阅读的图

- 人/鼠肿瘤 IL27 与 CTL signature 关联。
- CTL 特异 Il27ra 遗传验证。
- M86 tetramer⁺ 单细胞/TCR 图谱。
- IL-27 蛋白治疗与 PD-L1 联合疗效。
- OAK/IMvigor 分层与人 CMV 特异 CTL 重复刺激。

## 7. 创新性

以肿瘤特异 tetramer⁺ CTL 的 GEX + TCR 为主线，把细胞因子治疗、克隆持久性和临床队列关联连在一起；同时提供全免疫生态和 CD4/Treg 数据评估非目标效应。

## 8. 局限性

临床部分主要是回顾性表达关联；IL27 与 EBI3 高表达也可能是既有炎症浸润的结果。小鼠给药耐受性不能排除人类系统毒性。多个 accession 的样本设计不同，不能简单合并为单一 70 样本差异表达分析。

## 9. 在综述中的定位

适合作为“细胞因子程序化提升肿瘤特异 CTL 质量”的代表，与 IL-2、IL-15、IL-21 及 PD1-IL2v 对照其受体分布、持久性和毒性。

## 10. 可直接写入综述的表述

> IL-27 直接在肿瘤特异 CD8⁺ T 细胞中维持高细胞毒和持续性程序，并在小鼠中与 PD-L1 阻断协同，提示其可作为区别于经典 IL-2 轴的 CTL 定向细胞因子策略。

## 11. 数据复用建议

可在 E-MTAB-14601 中做 clonotype-aware pseudobulk，检验同一克隆跨 tumour/dLN 和治疗状态的分布；用 14580/14584 联合分析 IL-27 响应基因与开放染色质；再用 14590 验证这些程序在全 TME 的细胞类型特异性。

## 12. 转化与安全性关注

应重点评估慢性感染、自身免疫和造血系统中的 IL-27R 响应，监测 IFN样炎症、肝毒性和非肿瘤特异 CTL 激活。生物标志物需区分 IL27/EBI3 是预测性还是仅预后性。

## 13. 避免误读

- 70 是五个新测小鼠 accession 的生物样本合计，不是一个同质队列。
- E-MTAB-14601 的 55 assays 对应 11 个样本的多类文库。
- 临床表达关联不能证明 IL-27 治疗因果效应。
- EGA 数据受控，只有 accession 不等于无需申请即可下载。

