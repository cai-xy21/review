# 《A cell atlas of human thymic development defines T cell repertoire formation》精读

## 论文信息

- 第一作者：Jong-Eun Park
- 期刊：*Science*，2020；367(6480): eaay3224
- DOI：10.1126/science.aay3224
- 原文：[Science](https://www.science.org/doi/10.1126/science.aay3224)
- 开放全文：[PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC7611066/)
- HCA 项目页：[Human Cell Atlas Data Explorer](https://explore.data.humancellatlas.org/projects/c1810dbc-16d2-45c3-b45e-3e675f88d87b)
- 处理数据：[ArrayExpress E-MTAB-8581](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-8581)
- 在线对象：[CELLxGENE collection](https://cellxgene.cziscience.com/collections/de13e3e2-23b6-40ed-a413-e9e12d7d3910)

## 一句话结论

研究以 255,901 个细胞覆盖 15 份胚胎/胎儿胸腺和 9 份出生后胸腺，联合 TCR 重排与空间验证，建立人 T 细胞从早期胸腺祖细胞到成熟 αβ/γδ T 细胞的发育参考图谱。

## 1. 为什么重要

外周 T 细胞图谱记录的是发育完成后的状态；这篇论文回答“这些状态从哪里来”。它把转录状态、胸腺微环境与 TCR 重排/选择放到同一发育坐标中，是 charting 章节的发育起点。

## 2. 数据护照

| 维度 | 规模/内容 | 含义 |
|---|---:|---|
| 细胞 | 255,901 | 经过质控的胸腺单细胞图谱 |
| 胚胎/胎儿供者 | 15 | 7–17 post-conception weeks |
| 出生后供者 | 9 | 儿童至成人 |
| 主模态 | droplet scRNA-seq（3′/5′） | 不同化学版本需在整合时控制 |
| 受体模态 | 配对 scTCR αβ/γδ，覆盖特定 5′/富集样本 | 不是全部 255,901 个细胞都有 TCR |
| 组织验证 | smFISH/原位与流式等 | 用于定位细胞和验证 marker |
| 公共数据 | E-MTAB-8581、HCA、CELLxGENE | 处理对象与原始 FASTQ 均可追溯 |

## 3. 研究设计

- 使用多种分选策略增加稀有基质、上皮和免疫亚群覆盖。
- 解析早期胸腺祖细胞、DN、DP、正选择、CD4/CD8 单阳性、Treg、γδ T 等连续阶段。
- 将胸腺上皮、成纤维、内皮、髓系和 B 细胞作为发育微环境共同建图。
- 用 TCR 重排状态连接发育阶段，比较人和小鼠的重排与选择偏好。

## 4. 关键发现

### 4.1 人胸腺发育是连续、分支且受组织位置约束的过程

转录组解析出传统 DN→DP→SP 框架内更多过渡阶段；T 细胞命运不是几个离散门，而是受选择信号和微环境共同塑造的连续过程。

### 4.2 Treg 与常规 CD4 谱系在胸腺内出现可分辨分支

图谱提供人胸腺 Treg 发生的细胞状态与候选信号环境，但转录轨迹本身不等于直接谱系证明。

### 4.3 TCR 重排与选择偏好具有物种差异

作者系统描述 αβ 与 γδ TCR 在不同发育阶段的重排和选择，并比较人与小鼠偏好。结论支持：小鼠胸腺发育框架可作参照，但不能直接代替人类 TCR repertoire。

## 5. TCR 数据看什么

1. 每个发育阶段是否已出现 productive TRA/TRB 或 TRG/TRD。
2. V/J 基因使用、CDR3 长度及重排组合随选择阶段如何变化。
3. αβ 与 γδ 分支何时在转录与受体层分开。
4. 人鼠 repertoire 偏好差异。

必须先按单细胞条码把 V(D)J 与 GEX 对齐；“有 contig”不等于“有一条 productive α + 一条 productive β”。多 α 链、双 β 链和低可信 contig 需单独处理。

## 6. 数据获取

- **直接浏览/下载 AnnData**：[CELLxGENE](https://cellxgene.cziscience.com/collections/de13e3e2-23b6-40ed-a413-e9e12d7d3910)。
- **处理矩阵与原始研究入口**：[E-MTAB-8581](https://www.ebi.ac.uk/biostudies/arrayexpress/studies/E-MTAB-8581)。
- **HCA 项目级元数据与原始文件**：[HCA Data Explorer](https://explore.data.humancellatlas.org/projects/c1810dbc-16d2-45c3-b45e-3e675f88d87b)。
- 分析前检查 `donor、age、prenatal/postnatal、sort、chemistry、cell_type、developmental_stage`；TCR 分析另查 productive、chain、V/J、CDR3 与 barcode。

## 7. 推荐图版

- **Fig. 1**：样本设计、全细胞图谱与发育覆盖；最适合章节发育起点。
- **T lineage trajectory 主图**：展示 DN–DP–SP 连续坐标；适合讲“状态是发育连续体”。
- **TCR repertoire formation 图**：展示重排/选择变化；适合连接 HLA–TCR 选择与外周功能。
- **胸腺微环境/配体受体图**：适合讲细胞状态由生态位塑造。

## 8. PPT 单页格式

**标题**：T 细胞分子景观始于胸腺内的发育与受体选择

**正文**：15 份胎儿 + 9 份出生后胸腺；255,901 个细胞；scRNA 与 scTCR 联合解析 DN→DP→SP 及 αβ/γδ repertoire 形成。

**配图**：Fig. 1 + T lineage/TCR formation 主图局部。

**页脚引用**：Science 2020, Park。

## 9. 局限性

- 健康人胸腺材料稀缺，年龄段、组织质量和分选策略不均衡。
- 破坏性单细胞快照和伪时间不能直接证明个体细胞命运。
- TCR 覆盖集中于特定 5′ 数据，不应以全图谱细胞数作 TCR 分母。
- 低丰度上皮/基质细胞对富集方案敏感。

## 10. 可直接用于综述

> 人胸腺单细胞图谱将 255,901 个细胞的转录状态与 TCR repertoire 形成相连，为外周 T 细胞状态提供了发育坐标；其核心启示是，成熟 T 细胞的功能边界在进入肿瘤或炎症组织前已受到胸腺选择与生态位塑造（Science 2020, Park）。
