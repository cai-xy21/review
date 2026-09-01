# 《Epigenetic stability of exhausted T cells limits durability of reinvigoration by PD-1 blockade》精读

## 论文信息

- 作者：Kristen E. Pauken、Morgan A. Sammons、Patrick M. Odorizzi 等
- 期刊：Science
- 年份：2016；354(6316): 1160–1165
- DOI：10.1126/science.aaf2807
- PubMed：[PMID 27789795](https://pubmed.ncbi.nlm.nih.gov/27789795/)
- 全文：[PMC5484795](https://pmc.ncbi.nlm.nih.gov/articles/PMC5484795/)
- microarray：[GEO GSE86796](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE86796)
- bulk RNA-seq：[GEO GSE86881](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE86881)
- bulk ATAC-seq：[GEO GSE86797](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE86797)

## 一句话结论

在小鼠 LCMV 慢性感染模型中，PD-L1 阻断能短暂重塑耗竭 CD8 T 细胞的转录和功能，却只引起有限的染色质可及性改变；稳定的耗竭表观遗传景观使细胞在停药或再次抗原暴露后重新耗竭，从而限制持久 reinvigoration。

## 数据护照（先看这一表）

| 维度 | 内容 | 分析提醒 |
|---|---|---|
| 模型 | P14 CD8 T cells；LCMV Arm vs Cl13 | 小鼠病毒模型，不是肿瘤患者 |
| 干预 | anti-PD-L1 | 主要比较慢性感染耗竭细胞治疗前后 |
| 分子数据 | microarray、bulk RNA-seq、bulk ATAC-seq | 没有单细胞 RNA/ATAC |
| microarray | GSE86796，8 samples | 4 个生物重复 × untreated/anti-PD-L1 |
| RNA-seq | GSE86881，13 samples | 含 acute/chronic、短期和长期治疗/追踪条件 |
| ATAC-seq | GSE86797，10 samples | 5 状态 × 2 replicates |
| accession 纠错 | GSE86765 不是本文数据 | paperlist 若写 GSE86765，应改为上述三个 accession |

## 1. 研究要解决的问题

PD-1 阻断可以使耗竭 T 细胞增殖和部分恢复功能，但临床上并非所有反应都持久。论文要区分：

1. reinvigoration 是稳定重编程，还是短暂转录响应；
2. 耗竭细胞的染色质状态能否回到 effector/memory；
3. 治疗后细胞在长期或再次遇到抗原时是否保留改善状态。

## 2. 方法框架

### 2.1 LCMV/P14 模型

作者转移具有固定病毒特异 TCR 的 P14 CD8 T 细胞：

- LCMV Armstrong：急性感染，产生 effector 和 memory；
- LCMV Clone 13：慢性感染，产生 exhausted cells；
- 慢性感染中给予 anti-PD-L1。

通过流式、细胞因子、增殖、过继转移和再挑战评估功能，并分选 P14 细胞进行 bulk 分子测量。

### 2.2 三套分子数据

- microarray：较早、较集中的治疗转录比较；
- bulk RNA-seq：更广的时间和处理条件；
- bulk ATAC-seq：比较 naive、effector、memory、exhausted 和治疗后 exhausted 的开放染色质。

数据的统计单位是分选细胞群的生物重复，不是单细胞。

## 3. 数据规模与组成

### 3.1 GSE86796：microarray

[GSE86796](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE86796) 包含 8 个样本：

| 条件 | 生物重复 |
|---|---:|
| chronic exhausted untreated | 4 |
| chronic exhausted + anti-PD-L1，约 2 周 | 4 |

平台为 Affymetrix microarray。GEO 提供：

- 原始 CEL 文件；
- GSE86796_RAW.tar，约 66.8 MB；
- series matrix/样本注释。

下载：

~~~bash
wget -c \
  https://ftp.ncbi.nlm.nih.gov/geo/series/GSE86nnn/GSE86796/suppl/GSE86796_RAW.tar
~~~

该数据最适合复现早期“治疗响应基因”分析；跨平台整合时不能把 probe intensity 当作 RNA-seq counts。

### 3.2 GSE86881：bulk RNA-seq

[GSE86881](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE86881) 共 13 个 bulk RNA-seq 样本，SRA study 为 SRP087641。样本组成可概括为：

| 状态/条件 | 样本数 |
|---|---:|
| naive P14 | 2 |
| exhausted untreated，治疗结束后 1 天 | 3 |
| exhausted + anti-PD-L1，治疗结束后 1 天 | 4 |
| exhausted untreated，day 160 与 day 235 | 2，各时间点 1 |
| exhausted + anti-PD-L1，day 160 与 day 235 | 2，各时间点 1 |

GEO 当前提供约 1.1 MB 的 processed FPKM CSV.gz；原始 reads 从 SRA 下载。13 个样本比较的都是 LCMV Clone 13 慢性感染背景中的 naive 参照、短期 untreated/treated 与 day 160/235 长期随访；这里没有 Armstrong 急性感染组。实际分组应严格使用每个 GSM title 和 characteristics，不能只按文件顺序分组。

下载处理后矩阵可从 GEO supplementary files 页面取得；原始 run table：

~~~bash
curl -L \
  "https://trace.ncbi.nlm.nih.gov/Traces/sra-db-be/runinfo?acc=SRP087641" \
  -o SRP087641_runinfo.csv
~~~

### 3.3 GSE86797：bulk ATAC-seq

[GSE86797](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE86797) 共 10 个样本：

| 状态 | 重复数 |
|---|---:|
| naive | 2 |
| effector | 2 |
| memory | 2 |
| exhausted | 2 |
| exhausted + anti-PD-L1 | 2 |

GEO 提供约 7.7 MB 的 processed BED/peak RAW.tar；原始 reads 对应 SRA study SRP088651。

下载处理后 peaks：

~~~bash
wget -c \
  https://ftp.ncbi.nlm.nih.gov/geo/series/GSE86nnn/GSE86797/suppl/GSE86797_RAW.tar
~~~

ATAC 的 10 个样本是 5 种群体状态各 2 个生物重复，不是 10 个时间点，也不是 10 个单细胞。

### 3.4 三套数据如何对应

| 数据集 | 技术 | 样本数 | 主要问题 |
|---|---|---:|---|
| GSE86796 | microarray | 8 | anti-PD-L1 的转录响应 |
| GSE86881 | bulk RNA-seq | 13 | 急/慢性、短期/长期转录状态 |
| GSE86797 | bulk ATAC-seq | 10 | 五种 T 细胞状态的开放染色质 |

三个 GEO series 不是同一批细胞的严格多组学配对。联合解释可以说明转录与染色质总体不一致，但不能对每个样本做单细胞级配对。

### 3.5 原始与处理后下载建议

快速复图：

- microarray 下载 CEL 或 series matrix；
- RNA-seq 下载 processed FPKM；
- ATAC 下载 BED/peak 文件；
- 同时下载全部 sample metadata。

统一重处理：

- microarray 用对应芯片注释和 RMA；
- RNA-seq 从 SRP087641 重新比对/定量；
- ATAC 从 SRP088651 重新比对、去重复、统一 peak calling；
- 在 donor/mouse replicate 层面做统计。

不建议把 FPKM 直接输入现代 counts-based 差异表达模型，也不要把处理后 peak 文件误作 fragment file。

## 4. 核心发现

### 4.1 耗竭是独立且稳定的染色质状态

exhausted P14 与 naive、effector、memory 在 ATAC 空间中明显分离，说明耗竭并非只是在记忆细胞上叠加一组抑制受体，而是具有系统性染色质架构。

### 4.2 PD-L1 阻断重塑转录多于染色质

治疗诱导增殖、效应和炎症相关转录变化，但绝大部分 exhausted-specific accessible regions 仍保留。治疗后细胞因此更像“在耗竭框架内被激活”，而不是被重置为 memory。

### 4.3 Reinvigoration 不持久

治疗细胞在长期跟踪、转移或再挑战后重新显示耗竭特征，提示持续抗原与既有表观遗传景观共同限制耐久性。

## 5. 分子驱动与可干预线索

作者从差异转录和染色质 motif 提出 NF-κB、IL-7R 等潜在协同靶点。论文的最强结论是“单独 PD-1 阻断不足以重置表观遗传状态”；具体联合靶点仍需独立因果验证。

## 6. 关键图表怎么读

- RNA/ATAC PCA：展示群体均值差异，无法判断是否由亚群比例变化造成。
- accessibility heatmap：bulk peak 强度是群体平均。
- 治疗前后转录比较：短期响应不等于稳定命运改变。
- transfer/rechallenge：增强持久性结论，但小鼠病毒环境与肿瘤不同。

## 7. 创新点

1. 区分短期功能恢复与稳定表观遗传重编程。
2. 将长期命运实验与 bulk RNA/ATAC 对接。
3. 提出 exhausted lineage 的稳定性框架。
4. 为“checkpoint blockade 后为何复发/再耗竭”提供机制解释。

## 8. 局限性

1. bulk 数据不能分辨 Tpex 与 terminal Tex 等亚群。
2. 不同组学并非完全同一样本配对。
3. 每组通常只有 2–4 个重复。
4. 固定 TCR、LCMV 小鼠模型限制人肿瘤外推。
5. ATAC 可及性不等于 TF 实际结合或增强子功能。
6. 2016 年数据缺少现代单细胞多组学分辨率。

## 9. 对本综述的作用

该论文是“状态导航不只改变转录输出，还必须跨越稳定表观遗传势垒”的奠基证据。它支持：

- 用多层 readout 定义导航是否成功；
- 把短期功能恢复和长期可塑性分开；
- 将 PD-1 阻断与染色质重塑、代谢或记忆维持干预组合；
- 构建闭环系统时设置长期回弹/再耗竭指标。

## 10. 可直接写进综述的表述

> 在 LCMV 慢性感染中，PD-L1 阻断可显著改变耗竭 CD8 T 细胞的转录和短期功能，却仅有限改变其 bulk ATAC 景观；稳定的耗竭染色质状态使 reinvigoration 易于回弹，说明状态导航的评价必须同时包含即时功能与长期表观遗传可塑性。

## 11. 最容易误读的地方

- 本文没有单细胞 RNA-seq 或单细胞 ATAC。
- GSE86765 不是本文数据；正确入口是 GSE86796、GSE86881、GSE86797。
- 10 个 ATAC samples 是 5 状态 × 2 重复。
- 短期转录改善不等于记忆样重编程。
- bulk 峰变化不能直接定位到某个 T 细胞亚群。
