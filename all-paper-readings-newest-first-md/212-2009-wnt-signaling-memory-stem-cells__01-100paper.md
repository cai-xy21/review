# 《Wnt signaling arrests effector T cell differentiation and generates CD8+ memory stem cells》精读

## 论文信息

- 作者：Luca Gattinoni、Xin-Sheng Zhong、Douglas C. Palmer 等
- 期刊：*Nature Medicine*
- 年份：2009；15: 808–813；发表于 2009 年 6 月 14 日
- DOI：10.1038/nm.1982
- 原文：[Nature Medicine](https://www.nature.com/articles/nm.1982)
- PubMed：[PMID 19525962](https://pubmed.ncbi.nlm.nih.gov/19525962/)
- 开放全文：[PMC2707501](https://pmc.ncbi.nlm.nih.gov/articles/PMC2707501/)
- 补充材料：PMC 页面提供 1 个 DOC（约 40.5 KB）和 1 个 PDF（约 805 KB）

## 一句话结论

在 pmel-1 小鼠 CD8 T 细胞中，TWS119、BIO 类 GSK-3β 抑制剂或 Wnt3A 在激活期抑制效应分化并产生 CD44^low^CD62L^high^Sca-1^high^CD122^high^Bcl-2^high^ TSCM；少量 TSCM 的扩增和抗 B16 效力优于 TCM/TEM，但论文没有公共原始数据 accession。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 生物体系 | pmel-1 TCR 转基因小鼠 CD8 T 细胞 | 主要是鼠数据，不是人 CAR-T 制造数据 |
| 抗原 | hgp100~25–33~；B16 gp100⁺ melanoma | TCR 模型抗原特异性强，外推需谨慎 |
| 主要扰动 | TWS119 7 μM；BIO/BIO-acetoxime；Wnt3A | GSK-3β 有 Wnt 外靶通路，药理结果不等同纯 β-catenin 因果 |
| 培养 | IL-2 10 ng/mL；4–5 d | 高 TWS119 同时抑制分裂，存在产量—干性权衡 |
| in vivo immunization | 10⁶ naïve pmel-1；TWS119 30 mg/kg ×4 d | 表型变化并未解析细胞内/外作用 |
| 肿瘤比较 | 4×10⁴ TSCM/TCM/TEM；n=5/group | 同时有疫苗和 exogenous IL-2 |
| 公开数据 | 正文 + 2 个补充文件 | 无 GEO/SRA、无 FCS、无 BLI/tumor CSV、无代码 |

## 1. 研究要解决的问题

Wnt/β-catenin 是否能像维持组织干细胞一样，阻止成熟 CD8 T 细胞过早进入终末效应状态，并生成既有记忆功能又有自我更新/多向分化能力的 TSCM；这种“少分化”是否转化为更强的体内扩增和肿瘤控制。

## 2. 方法框架

### 2.1 分子扰动

naïve pmel-1 CD8 T 细胞用 hgp100、IL-2 激活，同时加入不同剂量 TWS119。作者测 β-catenin 累积、TCF/LEF DNA binding、`Tcf7`、`Lef1`、`Jun`、`Fzd7`、`Nlk` 和 `Eomes`。为降低单药解释，另用 BIO、BIO-acetoxime、失活 Methyl-BIO 和天然 Wnt3A。

### 2.2 状态与功能

- CFSE：分裂；
- CD44/CD62L：分化；
- Sca-1、CD122、Bcl-2：TSCM 支持 marker；
- IFN-γ、IL-2、^51^Cr release：效应功能；
- 连续/二次转移：自我更新和多向分化；
- WT、Tcra^-/-^、Rag1^-/-^、B2m^-/-^ 宿主：homeostatic expansion 与 MHC-I independence；
- B16 模型：TSCM/TCM/TEM 的 recall 和抗肿瘤效力。

## 3. 数据规模与实验组成

### 3.1 数据到底是什么

该文的数据是流式、CFSE、qRT-PCR、Western blot/EMSA、细胞因子、细胞毒、肿瘤体积、生存和体内转移的图表集合。没有全转录组或单细胞组学；分子层仅为预选 Wnt/效应基因。

它建立的是一个“扰动—状态—功能”因果框架，而不是可下载的 atlas。

### 3.2 实验模块与规模

| 模块 | 关键设计 | 规模信息 |
|---|---|---:|
| Wnt activation | anti-CD3/CD28 ± 7 μM TWS119 | ≥2 独立实验 |
| dose response | hgp100 + IL-2 + graded TWS119 | ≥2 独立实验 |
| in vivo vaccination | 10⁶ naïve pmel-1；30 mg/kg TWS119 d0–3 | ≥2 独立实验；图注未给每组总鼠数 |
| memory proof | TWS119-generated CD44^low^CD62L^high^ vs TN | 多宿主、1 month readout |
| secondary transfer | 再转移后 4 weeks | 43% 仍保留 TSCM phenotype |
| recall expansion | 5×10⁴ TSCM/TCM/TEM + vaccinia-gp100 + IL-2 | TSCM 脾内约 200-fold expansion |
| B16 therapy | 4×10⁴ cells；10 d established B16 | n=5/group；≥2 independent experiments |

主文很多 panel 写“representative of at least two independently performed experiments”，但这不能推导出准确总动物数；报告和 meta-analysis 应只引用明确的 n=5/group 部分。

### 3.3 公开文件有什么

PMC 的 Associated Data 仅列：

- `NIHMS115647-supplement-1.doc`，约 40.5 KB；
- `NIHMS115647-supplement-2.pdf`，约 805 KB。

补充材料包含额外图和说明，但没有机器可读的 flow cytometry event table、qPCR Ct、tumor growth spreadsheet、BLI 或 survival data。论文也没有 Data availability statement 或 GEO/SRA accession。

### 3.4 如何获取

#### 路线 A：公开结果

从 [PMC2707501](https://pmc.ncbi.nlm.nih.gov/articles/PMC2707501/) 下载正文和两个补充文件。适合提取剂量、细胞数、时间点、marker 和图中统计。

#### 路线 B：原始数据

需联系作者/机构档案，请求：

1. 所有 FCS（含补偿、gating hierarchy 和批次）；
2. qPCR Ct/ΔCt 与 Western densitometry；
3. CFSE generation-level event summary；
4. 每只小鼠 tumor volume、BLI、survival、随机化和排除记录；
5. 每次独立实验的 donor/mouse/cell preparation 映射。

由于研究发表于 2009 年，原始文件可获得性不能预设；“PMC 有补充材料”不等于原始数据公开。

### 3.5 若从图中重建数据

可用 WebPlotDigitizer 提取 tumor growth/expansion，但必须保留图层、误差条定义和重复结构。生存曲线重建只能近似事件时间；不得把从均值±SEM 反推的点当作原始个体数据。

## 4. 主要发现

1. TWS119 使 β-catenin 增加约 6.8-fold，并上调 TCF/LEF 靶基因。
2. TWS119 剂量依赖保留 CD62L、抑制 CD44 上调；高剂量同时抑制分裂。
3. TWS119 降低即时杀伤和 IFN-γ、保留 IL-2，并早期抑制 `Eomes`。
4. TWS119、BIO-acetoxime 和 Wnt3A 均可产生 TSCM-like 状态；失活 Methyl-BIO 无效。
5. TWS119-derived TSCM 在体内强扩增且能再生多个状态；二次转移后仍有 43% 保留 TSCM。
6. TSCM recall 约 200-fold，约为 TCM 10 倍、TEM 30 倍。
7. 仅 4×10⁴ TSCM 就能控制约 1 cm³ B16 肿瘤，优于 TCM/TEM。

## 5. 关键状态 marker

- TSCM-like：CD44^low^CD62L^high^Sca-1^high^CD122^high^Bcl-2^high^；
- Wnt active：β-catenin、Tcf7、Lef1、Jun、Fzd7、Nlk；
- effector differentiation：CD44、Eomes、IFN-γ、cytolysis；
- memory/recall：IL-2、快速再应答、homeostatic proliferation、MHC-I-independent persistence。

## 6. 状态导航的核心机制

最重要的概念不是“Wnt 让 T 细胞更强”，而是 Wnt 在激活窗口减慢分化，使细胞牺牲部分即时效应和增殖，换取更大的后续扩增/多向分化空间。这形成典型的制造多目标优化：短期产量和即时杀伤与长期干性、持久性可能方向相反。

## 7. 推荐图版

- **Fig. 1**：Wnt pathway target engagement。
- **Fig. 2**：剂量—分化—分裂—效应权衡，最适合优化章节。
- **Fig. 3**：TSCM 自我更新和多向分化。
- **Fig. 4**：TSCM/TCM/TEM recall 与 B16 efficacy。

如果只能选一张，选 Fig. 2；若强调治疗价值，选 Fig. 4。

## 8. 创新价值

1. 用可药理操纵的 Wnt/GSK-3β 轴导航成熟 CD8 状态。
2. 定义 murine TSCM 的表型与功能证据链。
3. 显示更少即时效应可以换来更强体内治疗效力。
4. 将“干性”转化为制造过程可优化变量。

## 9. 局限性

1. 主要是单一 TCR 转基因鼠模型和高亲和 gp100 系统。
2. GSK-3β 抑制影响多条通路，不能把全部效应归因于 β-catenin。
3. 高剂量 TWS119 抑制细胞分裂，转化到制造存在产量问题。
4. B16 治疗同时使用 lymphodepletion、vaccination 和 IL-2，非单因素。
5. marker 定义与人 TSCM 不完全相同，Sca-1 无人同源物。
6. 无公共原始数据，精确复现和再统计困难。

## 10. 对本综述章节的作用

这是“techniques to perturb/manipulate cell states”与“optimize conditions”部分的奠基文献：它以剂量和时间窗口展示如何用通路扰动改变 T cell differentiation trajectory，并揭示必须同时优化产量、即时功能和长期效力。

## 11. 可直接用于综述的观点

> 激活期 Wnt/GSK-3β 调控可以暂时压低 CD8 T 细胞的效应分化和即时杀伤，却生成具有更强自我更新、扩增与抗肿瘤潜力的 TSCM，体现细胞治疗制造中“短期效应—长期干性”的关键权衡（Nature Medicine 2009, Gattinoni）。

## 12. 数据复用建议

该文更适合作为因果机制和实验设计先例，而不是大数据二次分析资源。若设计新筛选，应把 TWS119/Wnt3A 作为 positive control，同时测细胞产量、分化、代谢、持久性和体内效力，避免只以 CD62L 高作为优化目标。

## 13. 避免误读

- 不要把“抑制分化”写成“全面增强所有 T cell 功能”。
- 不要把 TWS119 的作用等同于纯 Wnt/β-catenin 特异作用。
- 不要把鼠 TSCM marker 直接照搬到人细胞。
- 不要称本研究有公开组学数据集；只有正文和补充文件。
