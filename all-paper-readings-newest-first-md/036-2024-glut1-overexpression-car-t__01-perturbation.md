# 《GLUT1 overexpression in CAR-T cells induces metabolic reprogramming and enhances potency》精读

## 论文信息

- **作者**：Justin A. Guerrero, Dorota D. Klysz, Yiyun Chen 等
- **期刊与年份**：*Nature Communications*, 2024
- **DOI**：10.1038/s41467-024-52666-y
- **本地原文**：[PDF](<D:/research/review/perturbation33references/17-GLUT1 overexpression in CAR-T cells induces metabolic reprogramming and enhances potency.pdf>)
- **核心数据入口**：[GEO GSE275152](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE275152)

## 一句话结论

在 CAR-T 中过表达葡萄糖转运体 GLUT1/SLC2A1 可重塑代谢和转录状态，提高扩增、效应功能与抗肿瘤效力，说明营养摄取能力本身可作为细胞治疗工程参数。

## 数据护照

| 项目 | 内容 |
|---|---|
| 数据类型 | 人 CAR-T bulk RNA-seq；代谢、功能和体内实验 |
| 公开组学 | GSE275152，共 42 个 RNA-seq文库 |
| 独立人供体 | 未刺激模块 6 位；刺激时间模块 3 位（按 GEO donor ID 区分） |
| 时间点 | 未刺激；刺激后 4 h、14 h |
| 构建 | 未转导 T、CD19 CAR、GLUT1OE-CD19 CAR、HA CAR、GLUT1OE-HA CAR |
| 原始数据 | SRA / PRJNA1148127 |
| 处理后数据 | stimulated/unstimulated 的 raw counts 与 TPM，共 4 个矩阵 |

## 1. 研究问题

CAR-T 在肿瘤中需要与肿瘤细胞竞争葡萄糖等营养物。本文检验提高 GLUT1 表达是否能突破代谢限制，并进一步问这种改变是单纯增加燃料输入，还是会系统性重塑 T 细胞状态与效力。

## 2. 实验设计与方法框架

作者在抗 CD19 CAR 或无关抗原 HA CAR 构建中加入 GLUT1 过表达模块，评估葡萄糖摄取、代谢通量、扩增、细胞因子、杀伤和肿瘤模型疗效。bulk RNA-seq 分为未刺激基线与抗原刺激后 4 h/14 h 两套设计，用无关 CAR 和未转导 T 细胞帮助区分抗原特异与构建本身的效应。

## 3. 数据规模与图谱组成

### 3.1 总体规模

GSE275152 包含 **42 个 Homo sapiens bulk RNA-seq样本/文库**，平台为 NovaSeq 6000，原始数据关联 PRJNA1148127。42 个文库由不同供体、构建和时间点展开，不是 42 位供体。

### 3.2 未刺激基线模块：12 个样本

该模块比较 control 与 GLUT1OE：

- 3 位 CD19 CAR 供体（GEO donor 11、16、17）× 2 构建 = **6 个样本**；
- 3 位 HA CAR 供体（donor 77、86、90）× 2 构建 = **6 个样本**；
- 合计 **12 个未刺激样本**。

这里可评估 GLUT1OE 的基线效应，但 CD19 CAR 和 HA CAR 使用不同 donor ID，不能把 CAR 靶点差异与供体差异完全分离。

### 3.3 抗原刺激时间模块：30 个样本

使用 donor 61、62、63，在 4 h 与 14 h 各测量 5 个构建/条件：

- 未转导 T 细胞；
- CD19 CAR；
- GLUT1OE-CD19 CAR；
- HA CAR；
- GLUT1OE-HA CAR。

因此每个时间点为 **3 位供体 × 5 条件 = 15 个样本**，两个时间点合计 **30 个样本**。加上未刺激模块即 12 + 15 + 15 = **42**。

HA CAR 是无关抗原/构建对照，有助于区分 CD19 抗原触发与 GLUT1 普遍效应。4 h 与 14 h 来自相同三位供体的条件展开，统计模型应保留 donor 配对。

### 3.4 公开数据包

GEO 提供 4 个主要处理文件：

- stimulated TPM 矩阵，约 7.5 MB；
- stimulated raw-count 矩阵，约 1.6 MB；
- unstimulated TPM 矩阵，约 3.1 MB；
- unstimulated raw-count 矩阵，约 0.8 MB。

原始 reads 在 SRA。论文中的代谢通量、营养摄取和体内实验属于 Source Data/补充材料，不是 GEO 中的另一种 omics assay；本文公开项目也不是单细胞数据。

### 3.5 推荐下载方式

1. 从 GSE275152 下载四个矩阵与 series/sample annotation。
2. 差异表达使用 raw counts；TPM 用于表达展示或基因间谨慎比较。
3. 需要重比对时进入 SRA Run Selector，导出 42 条记录的 `RunInfo.csv` 和 accession list，再用 SRA Toolkit 下载。
4. 将 stimulated 与 unstimulated 分目录保存；不要因列名相似而直接合并。
5. 建立 `donor`、`target`、`GLUT1OE`、`antigen_stimulated`、`time_h`、`construct` 字段，并对照 GEO title 检查 12/15/15 的计数闭合。

### 3.6 推荐统计设计

刺激模块可用 `~ donor + time + construct + time:construct`，或在每个时间点内做供体配对对比。重点对比包括：GLUT1OE-CD19 CAR vs CD19 CAR、GLUT1OE-HA CAR vs HA CAR，以及两种增量效应的差别。未刺激模块应在每种 CAR 内分别配对分析，避免把不同 donor 组强行比较。

## 4. 主要结果

GLUT1OE 提高葡萄糖利用并改变 CAR-T 的代谢、扩增与效应状态，在抗原刺激和肿瘤模型中增强功能。转录组支持这种增强不仅是瞬时摄取变化，还伴随激活/代谢程序重排。

## 5. 机制理解

更高 GLUT1 增加葡萄糖输入，改变糖酵解与相关生物合成能力，从而支持快速效应响应和持续扩增。其净作用依赖抗原刺激、构建和时间，不能用单一“糖酵解升高”概括所有状态。

## 6. 推荐重点阅读的图

- GLUT1 表达、葡萄糖摄取和代谢通量验证图。
- 4 h/14 h RNA-seq PCA、差异基因与通路图。
- 体外扩增、杀伤和细胞因子图。
- 体内肿瘤负荷、CAR-T 持续性与生存图。

## 7. 创新性

将营养转运器工程直接整合进 CAR-T 构建，并通过抗原特异/无关 CAR、时间点和供体配对形成较清晰的因果设计。

## 8. 局限性

bulk RNA-seq不能解析亚群；未刺激模块中不同 CAR 使用不同供体集合；公开组学主要是转录组，代谢数据的再分析入口较分散。持续提高营养摄取也可能促进终末分化或在不同肿瘤营养环境中产生不同结果。

## 9. 在综述中的定位

适合作为“代谢底物入口工程”的代表，与删除腺苷抑制受体、细胞因子工程和表观遗传预处理形成互补。

## 10. 可直接写入综述的表述

> GLUT1 过表达通过提高葡萄糖摄取并重塑刺激后的代谢转录程序增强 CAR-T 效力，提示营养转运能力可作为对抗肿瘤代谢竞争的工程化抓手。

## 11. 数据复用建议

可利用 42 样本的完整因子设计计算“GLUT1 增量效应”在 CD19 与 HA 构建、4 h 与 14 h 之间的交互，并与其他代谢干预数据比较通路层面的一致性。供体配对优先于简单 pooled t-test。

## 12. 转化与安全性关注

需要评估长期 GLUT1OE 是否改变终末分化、非特异增殖、低糖环境适应和细胞产品稳定性。更高代谢能力不必然等于更佳持久性，制造和体内阶段应分开验证。

## 13. 避免误读

- **42 是文库数，不是 42 位供体。**
- 未刺激模块的 CD19 CAR 与 HA CAR 采用不同 donor ID，不能做无混杂的靶点间比较。
- 4 h 与 14 h 的 30 个样本来自 3 位供体 × 5 条件 × 2 时间点。
- GEO 是 bulk RNA-seq，不是单细胞或完整代谢组仓库。

