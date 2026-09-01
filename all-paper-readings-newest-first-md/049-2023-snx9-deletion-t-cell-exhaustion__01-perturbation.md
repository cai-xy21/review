# 《Deletion of SNX9 alleviates CD8 T cell exhaustion for effective cellular cancer immunotherapy》精读

## 论文信息

- **作者**：Marcel P. Trefny, Nicole Kirchhammer, Priska Auf der Maur 等
- **期刊与年份**：*Nature Communications*, 2023
- **DOI**：10.1038/s41467-022-35583-w
- **本地原文**：[PDF](<D:/research/review/perturbation33references/13-Deletion of SNX9 alleviates CD8 T cell exhaustion for effective cellular cancer immunotherapy.pdf>)
- **数据入口**：[GSE190247](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE190247)；[EGA EGAS00001006794](https://ega-archive.org/studies/EGAS00001006794)；[分析代码 Zenodo 7307407](https://doi.org/10.5281/zenodo.7307407)

## 一句话结论

以重复肿瘤刺激下的脱颗粒能力为功能筛选终点，作者发现删除内吞/膜运输因子 SNX9 可减轻 CD8 T 细胞耗竭，并提高过继细胞治疗的持续功能与肿瘤控制。

## 数据护照

| 模块 | 规模 | 公开位置 |
|---|---:|---|
| 定向 CRISPR 筛选 | 32 个基因类别、180 条 sgRNA、5 位健康供体 | GSE190246 |
| 人 CD8 状态 bulk RNA-seq | 16 个样本，4 位供体 × 4 状态 | GSE210534；raw 受控于 EGA |
| 小鼠肿瘤内 OT-I scRNA-seq | 7,017 个细胞；4 个捕获/文库记录 | GSE210535 |
| SuperSeries | 汇总上述 GEO 模块 | GSE190247 |
| 代码 | v1.1 ZIP，约 181.6 MB | Zenodo 7307407 |

## 1. 研究问题

T 细胞耗竭由多条调控路径共同塑造。本文不以单一表面标记作筛选终点，而是问：在重复抗原刺激后，哪些可编辑因子决定 CD8 T 细胞仍能否脱颗粒并保持效应功能？

## 2. 实验设计与方法框架

作者从候选耗竭调控基因构建定向 CRISPR 文库，在原代人 CD8 T 细胞中重复肿瘤刺激，再按 CD107a 阳性/阴性进行功能分选和 sgRNA 富集分析。SNX9 命中后，通过人细胞体外验证、小鼠肿瘤模型、bulk RNA-seq 与肿瘤内 OT-I 单细胞 RNA-seq解释机制和状态改变。

## 3. 数据规模与图谱组成

### 3.1 图谱的三层结构

这套数据不是一个单一队列，而是“功能遗传筛选 → 人 T 细胞状态参照 → 小鼠肿瘤内单细胞验证”三层证据。三层的统计单位依次是供体、供体和小鼠；GEO 的文库数不能替代生物学重复数。

### 3.2 定向 CRISPR 筛选：GSE190246

- 候选集为 **29 个基因 + 3 个必需基因对照 = 32 个基因类别**。
- 每基因 5 条 sgRNA，并加入 20 条基因间区对照，合计 **180 条 sgRNA**。
- 文库克隆覆盖度超过 1,000 个菌落/sgRNA，文库 Gini 系数为 0.188。
- 原代人 CD8 T 细胞来自 **5 位健康供体**。重复肿瘤刺激后按 CD107a+ 与 CD107a− 分选，比较 sgRNA 丰度，并用 STARS 等方法排序命中。
- GEO 提供筛选的原始测序关联和处理后 sgRNA 计数/排名文件。这里的“样本数”应按供体和分选群体解释，180 是 guide 数，不是细胞数。

### 3.3 人 CD8 状态 bulk RNA-seq：GSE210534

共 **16 个样本**，由 **4 位供体 × 4 种状态**组成：静息/起始状态（Trest）、肿瘤共培养相关状态（Ttumor）、效应状态（Teff）与耗竭状态（Tex）。GEO 提供处理后的 `GSE210534_ensdb_105_dge_list_final.rds.gz`（约 2.6 MB）。

原始人测序数据因受试者数据治理要求不在 GEO 公开下载，而在 **EGA EGAS00001006794** 受控访问；申请联系人见 EGA 数据访问委员会信息。分析时应以供体为配对或阻断因子，不能把 16 个样本视为独立个体。

### 3.4 小鼠肿瘤内 scRNA-seq：GSE210535

实验比较肿瘤内对照 OT-I 与 Snx9-KO OT-I：

- 对照组：来自 **5 只小鼠的 3,405 个细胞**；
- Snx9-KO：来自 **6 只小鼠的 3,612 个细胞**；
- 合计 **7,017 个细胞**；
- GEO 中有 **4 个文库/捕获记录**：对照 pool A/B 与 Snx9-KO pool A/B。

pool A/B 是捕获或测序池，不代表只有 4 只小鼠。处理后文件包括约 1.2 GB 的整合注释 RDS 与约 297.8 MB 的 SingleCellExperiment 对象，原始数据可由 SRA 获取。

### 3.5 推荐下载方式

1. 从 GSE190247 分别进入 GSE190246、GSE210534、GSE210535，不要只下载父系列说明页。
2. 筛选复核：下载 sgRNA count 与 STARS 排名表；若重跑流程，再通过 SRA Run Selector 下载 FASTQ。
3. 人 bulk：一般分析先取 GEO 的 RDS；只有需要重新比对时才向 EGA 申请 raw data。EGA 是受控访问，不能用普通 `wget` 直接匿名下载。
4. 单细胞：优先下载已注释 RDS/SCE；需要重做 Cell Ranger 时再取 SRA raw reads。
5. 代码从 Zenodo 下载，并固定使用论文对应版本 v1.1；记录 R/包版本后再复现。

### 3.6 下载后如何整理

筛选元数据至少保留 `donor`、`sort_fraction`、`guide`、`gene`、`replicate`；bulk 保留 `donor` 与 `state`；scRNA 保留 `mouse`、`genotype`、`pool`、`cluster`。单细胞差异分析宜以小鼠为重复做 pseudobulk，不能把 7,017 个细胞当作 7,017 个独立重复。

## 4. 主要结果

SNX9 缺失在筛选中富集于维持脱颗粒能力的细胞，后续实验显示其可改善持续刺激下的功能。肿瘤内单细胞数据支持 Snx9-KO 细胞减少耗竭相关状态、保留更有利的效应/记忆样程序，并转化为更好的肿瘤控制。

## 5. 机制理解

SNX9 参与膜重塑和内吞运输。作者提出其删除会改变受体/信号调控及耗竭程序的建立，使 T 细胞在反复抗原暴露下不易陷入终末低功能状态。该结论由功能、转录状态和体内疗效共同支持，但具体膜运输底物并未被完全穷尽。

## 6. 推荐重点阅读的图

- 定向 CRISPR 文库、CD107a 分选和 SNX9 命中排序图。
- 重复刺激后 SNX9-KO 的脱颗粒、细胞因子和杀伤图。
- scRNA-seq UMAP、状态比例与耗竭/效应基因模块图。
- 小鼠肿瘤生长和生存结果。

## 7. 创新性

用功能性 CD107a 读出代替单纯增殖或标记表达，使筛选直接指向“反复刺激后仍能杀伤”的工程性状；同时公开了筛选、bulk、单细胞和代码，适合复现完整证据链。

## 8. 局限性

初筛是候选基因文库而非全基因组；人 bulk 与小鼠单细胞来自不同系统；单细胞鼠数有限且细胞经过池化；EGA 原始人数据需要申请。SNX9 的广泛细胞生物学功能也要求更长期的安全和归巢评估。

## 9. 在综述中的定位

可作为“功能表型驱动的耗竭抗性 CRISPR 筛选”案例，与以增殖为终点的 ORF/KO 筛选形成方法学对照。

## 10. 可直接写入综述的表述

> 基于重复刺激后 CD107a 脱颗粒能力的定向 CRISPR 筛选将 SNX9 识别为耗竭促进因子；其删除在肿瘤内单细胞状态和体内疗效层面均显示出维持 CD8 T 细胞功能的潜力。

## 11. 数据复用建议

可先用 GSE190246 重算 donor-aware guide 富集，再用 GSE210534 建立人 Tex/Teff 参考签名，最后在 GSE210535 的 mouse-level pseudobulk 中测试签名方向。这样比直接比较细胞比例更能避免供体/小鼠伪重复。

## 12. 转化与安全性关注

需评估 SNX9 删除是否影响正常免疫突触、迁移、内吞和非肿瘤组织中的激活阈值。当前数据支持增强效力，但不足以证明长期、全场景安全。

## 13. 避免误读

- **7,017 是细胞数，4 是文库/池记录，生物学重复是 5 只对照鼠和 6 只 KO 鼠。**
- GSE210534 的 16 个样本来自 4 位供体，不是 16 位供体。
- 人 bulk 原始数据受控存于 EGA；GEO 的 RDS 是处理后对象。
- 180 指 sgRNA 总数，不是筛选基因数；候选基因类别为 32。

