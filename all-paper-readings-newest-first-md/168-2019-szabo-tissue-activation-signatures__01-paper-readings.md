# 《Single-cell transcriptomics of human T cells reveals tissue and activation signatures in health and disease》精读

## 论文信息

- 第一作者：Peter A. Szabo、Hanna Mendes Levitin（共同第一作者）
- 期刊：*Nature Communications*，2019；10: 4706
- DOI：10.1038/s41467-019-12464-3
- 原文：[Nature Communications](https://www.nature.com/articles/s41467-019-12464-3)
- 开放全文：[PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC6797728/)
- 数据：[GEO GSE126030](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE126030)
- 在线数据集：[EMBL-EBI Single Cell Expression Atlas E-HCAD-8](https://www.ebi.ac.uk/gxa/sc/experiments/E-HCAD-8/results/tsne)
- 代码：[cluster/differential expression](https://github.com/simslab/cluster_diffex2018)；[scHPF](https://github.com/simslab/scHPF)；[UMAP projection](https://github.com/simslab/umap_projection)

## 一句话结论

研究以约 5.2 万个健康人血液及肺、淋巴结、骨髓 T 细胞为基础，在静息和抗 CD3/CD28 激活条件间建立共同参考，显示组织环境强烈塑造静息记忆 T 细胞，而激活程序在组织间更保守，并可用于解释肿瘤相关 T 细胞偏向激活 CD8 而缺少完整 CD4 功能激活的状态。

## 1. 研究问题

外周血易获得，却只代表人体 T 细胞的一小部分。作者希望区分两个经常被混为一谈的变化来源：

1. T 细胞处于血液、淋巴组织或黏膜组织造成的**组织适应**；
2. TCR 刺激造成的**激活程序**。

只有同时测量多组织、静息与激活条件，才能避免把组织差异误命名为新的功能状态，或把体外激活差异误认为组织特异性。

## 2. 数据护照

| 维度 | 规模/内容 | 解释 |
|---|---:|---|
| 健康供者 | 4 人 | 2 名器官供者 + 2 名健康血液供者 |
| 组织 | 肺、肺引流淋巴结、骨髓、外周血 | 涵盖黏膜、淋巴、骨髓和循环环境 |
| 条件 | 静息 + 抗 CD3/CD28 激活 | 体外培养约 16 h |
| 文库 | 16 个 | 2 器官供者 × 3 组织 × 2 条件 + 2 血供者 × 2 条件 |
| 单细胞 | 论文表述 >50,000；公开处理对象约 51,876 个 T 细胞 | 10x Genomics 3′ scRNA-seq |
| 受体测序 | 无配对 scTCR-seq | TCR 仅作为抗 CD3/CD28 激活通路，不提供 clonotype |
| 外部癌症数据 | NSCLC、CRC、乳腺癌、黑色素瘤 | 将已发表 T 细胞投影到健康参考，并非本文新测肿瘤样本 |

## 3. 实验与计算设计

1. 对肺、淋巴结、骨髓和血液进行 CD3 T 细胞磁珠负向富集。
2. 同一样本分为静息与抗 CD3/CD28 刺激条件。
3. 以 10x 3′ scRNA-seq 构建转录组，不进行 V(D)J 捕获。
4. 分供者聚类，并通过 UMAP/scmap 将血液 T 细胞投影到组织参考。
5. 用差异表达建立 resting memory T cell 的组织签名。
6. 用 scHPF 从各组织和供者中提取可复用的静息及激活表达模块。
7. 用 diffusion map 描述从静息到 IFN response、proliferation 或 CD8 effector 的连续变化。
8. 将四种癌症的公开 T 细胞数据投影到健康参考，检验参考图谱的解释能力。

## 4. 关键发现

### 4.1 静息状态最受组织影响

静息血液 T 细胞与骨髓 T 细胞最相似，而与肺和淋巴结重叠较少。组织中的 CCL5+ memory T 细胞共同表达细胞骨架、细胞—基质相互作用、增殖和信号相关基因，构成区别于血液的 tissue-associated signature。

这意味着将血液参考直接用于组织 T 细胞注释，会把正常组织适应误判为疾病或异常激活。

### 4.2 激活程序跨组织更保守

静息 T 细胞按组织分开明显，抗 CD3/CD28 激活后不同组织之间更接近。scHPF 总结出七类可解释模块：

- 静息：Treg、CD4 naive/central memory、CD4/CD8 resting；
- 激活/功能：proliferation、IFN response、CD8 cytokine、CD8 cytotoxic。

因此，“组织身份”和“功能激活”是可以部分分解的两条轴。

### 4.3 CD4 与 CD8 激活采用不同程序

- CD8 T 细胞主要沿细胞毒模块与细胞因子/趋化因子模块分化。
- CD4 T 细胞出现由 TCR 诱导 IFN-γ、继而形成的 II 型 IFN response 中间状态，随后进入增殖状态。
- NME1 标记更广泛的激活/增殖变化；IFIT3 更突出 CD4 的 IFN-response 状态。

### 4.4 癌症投影显示 CD8 激活强、CD4 功能激活不足

四种癌症的肿瘤相关 CD8 T 细胞覆盖健康参考中的静息与激活区域，并表达 TRM、细胞毒和细胞因子模块；相反，肿瘤相关 CD4 T 细胞更多投向静息状态，较少覆盖完整功能激活区域。表达耗竭标志的肿瘤 CD8 T 细胞仍与健康激活 CD8 程序重叠，同时出现健康参考外的增殖等偏离。

这里的“投影相似”不等于肿瘤 T 细胞与健康激活细胞完全相同；参考外残差恰恰可能包含耗竭、肿瘤适应或疾病特异程序。

## 5. TCR 数据边界

本文没有 scTCR-seq，也没有 TRA/TRB 配对、clonotype、克隆扩增或抗原特异性分析。“TCR stimulation”指用抗 CD3/CD28 激活细胞，而不是测量每个细胞的 TCR 序列。

因此该数据适合做表达参考映射，不适合回答：

- 某状态是否由同一克隆产生；
- 血液与组织是否共享克隆；
- 肿瘤 T 细胞识别什么抗原；
- 激活状态是否由特定 TCR 驱动。

## 6. 数据获取

| 数据 | 入口 | 内容 |
|---|---|---|
| 原始与处理 scRNA | [GSE126030](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE126030) | 16 个静息/激活文库；表达矩阵及样本信息 |
| 在线浏览 | [E-HCAD-8](https://www.ebi.ac.uk/gxa/sc/experiments/E-HCAD-8/results/tsne) | 可浏览表达与细胞分布 |
| 论文 Source Data | [Nature Communications](https://www.nature.com/articles/s41467-019-12464-3#Sec27) | T 细胞条码、UMAP、组织、刺激、CD4/CD8 等元数据 |
| 聚类/差异表达 | [GitHub](https://github.com/simslab/cluster_diffex2018) | 原始分析代码 |
| scHPF | [GitHub](https://github.com/simslab/scHPF) | 表达模块提取 |
| UMAP 投影 | [GitHub](https://github.com/simslab/umap_projection) | 查询数据映射 |

复用时必须保留 `donor、tissue、resting/activated、CD4/CD8`。供者只有四人，差异分析应以供者/样本为重复，不能把约 5.2 万个细胞当作 5.2 万个独立生物学重复。

## 7. 推荐图版

- **Fig. 1**：多组织、静息/激活设计及 T 细胞状态；介绍研究设计首选。
- **Fig. 3**：组织 memory T cell signature；讲“血液不能代表组织”首选。
- **Fig. 4**：scHPF 七个模块和 CD4/CD8 激活轨迹；讲参考坐标首选。
- **Fig. 5**：NME1 与 IFIT3 的激活动力学和机制验证；讲 CD4 IFN-response 时使用。
- **Fig. 6**：四种癌症 T 细胞向健康参考投影；讲疾病映射首选。
- **Fig. 7**：肿瘤 T 细胞的功能模块与耗竭标志；适合强调参考内与参考外程序。

若只能放一张，选 **Fig. 4**；若本页论点是“健康参考用于解释肿瘤”，选 **Fig. 6**。

## 8. PPT 单页格式

**标题**：组织身份与激活程序构成人 T 细胞状态的两条主轴

**正文**：约 51,876 个健康人 T 细胞，覆盖血液、肺、淋巴结和骨髓的静息与抗 CD3/CD28 激活条件。静息记忆 T 细胞具有显著组织签名，而 CD4 IFN-response、proliferation 及 CD8 cytokine/cytotoxic 程序可跨组织复现。

**配图**：Fig. 1a + Fig. 4a/d/e；如强调疾病应用，替换为 Fig. 6。

**页脚引用**：Nature Communications 2019, Szabo。

## 9. 在 T 细胞图谱中的定位

这是一张规模不算最大的“机制型参考图谱”。它最重要的贡献不是发现更多 cluster，而是明确指出：整合 T 细胞数据时，应把组织来源和激活条件作为真实生物轴保留，不能简单当作 batch 删除。它也构成后来 atlas projection 和疾病状态映射的早期范式。

## 10. 局限性与避免误读

- 仅四名供者，组织和血液并非同一批配对供者。
- 抗 CD3/CD28 是强烈、非抗原特异的体外刺激，不完全等同于体内感染或肿瘤激活。
- 只有肺、淋巴结、骨髓和血液，不能代表全部人体组织。
- 10x 3′ 数据没有配对 TCR，不能进行克隆或抗原层面的解释。
- 癌症部分整合不同研究和平台，投影会受到数据处理、测序深度和参考覆盖的影响。
- 肿瘤 CD4 缺少健康参考中的激活状态是相对映射结果，不应写成“肿瘤内 CD4 完全未激活”。

## 11. 可直接用于综述

> 健康人 T 细胞的分子状态同时受组织定位与激活条件塑造：静息记忆 T 细胞保留强烈的组织适应程序，而 CD4 IFN-response、增殖及 CD8 效应程序在多个组织间更为保守，提示跨队列整合必须保留组织域而非将其作为批次删除（Nature Communications 2019, Szabo）。

