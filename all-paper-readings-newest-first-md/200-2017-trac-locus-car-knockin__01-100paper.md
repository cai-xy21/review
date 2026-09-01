# 《Targeting a CAR to the TRAC locus with CRISPR/Cas9 enhances tumour rejection》精读

## 论文信息

- 作者：Eyquem J, Mansilla-Soto J, Giavridis T, van der Stegen SJ, Hamieh M, Cunanan KM, Odak A, Gönen M, Sadelain M
- 期刊：*Nature* 543:113–117；在线发表于 2017 年 2 月 22 日
- DOI：[10.1038/nature21405](https://doi.org/10.1038/nature21405)
- PubMed：[PMID 28225754](https://pubmed.ncbi.nlm.nih.gov/28225754/)
- 开放全文：[PMC5558614](https://pmc.ncbi.nlm.nih.gov/articles/PMC5558614/)
- 出版社页面、补充材料与 Source Data：[Nature](https://www.nature.com/articles/nature21405)

## 一句话结论

将 CD19-28ζ CAR 定点敲入 TRAC，使内源 TRAC 启动子调控 CAR 表达并同步破坏 TCR，可降低基础表达和 tonic signaling、恢复抗原刺激后的受体内化—再表达动力学，从而延缓分化/耗竭并在低剂量“CAR stress test”中优于随机逆转录病毒整合。

## 数据护照（先看这一表）

| 维度 | 内容 | 数据可获得性 |
|---|---|---|
| 细胞来源 | 多位健康供者 buffy coat 的人原代 T 细胞 | 供者级汇总见图注；无统一公共细胞数据表 |
| 工程条件 | TRAC-CAR、RV-CAR、RV-CAR-TCR−；另含 B2M/TRAC 与多启动子构建 | 构建设计、gRNA、方法在正文/Extended Data |
| 规模能力 | 可重复产生最高约 5×10^7 个 TRAC-CAR T 细胞 | 工艺结果，不是公开下载数据集规模 |
| 体外 readout | CAR/TCR 流式、表达 CV/MFI、连续刺激、细胞因子、杀伤、增殖 | 图源/补充数据；无 GEO/SRA accession |
| 基因组 readout | TLA 全基因组整合位点测序、CAR RNA 与表面蛋白动力学 | 图中代表性数据；未提供结构化公共仓库入口 |
| 体内 readout | NALM6-luc NSG 模型，5×10^4–2×10^5 CAR-T 剂量、最长约 100 天 | 个体小鼠曲线见图/Source Data |
| 公共仓库 | 无论文专属 GEO/SRA 数据集 | Data availability 明确写“相关数据可向作者获得” |

## 1. 研究要解决的问题

常规 γ-retroviral/lentiviral CAR 转导产生随机拷贝数和整合位点，CAR 表达量和细胞间异质性难以控制。作者询问：**CAR 放在哪里、由谁驱动表达，是否会改变同一受体序列的长期状态与疗效？**

## 2. TRAC 定点整合框架

作者使用 CRISPR/Cas9 在 TRAC 第一外显子切割，并以 AAV6 提供同源修复模板：TRAC splice acceptor–P2A–1928ζ CAR–polyA。结果是：

- 内源 TRAC promoter 驱动 CAR；
- TCRα 被破坏，降低内源 TCR 表达；
- CAR 表达从随机、多拷贝的 RV 模式变为相对均一的位点和剂量；
- 通过 B2M 位点以及 EF1α、LTR、PGK、PGK100 等外源 promoter 构建，分离“位点效应”和“启动子效应”。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这不是组学 atlas，而是一套**基因组位置—受体表达动力学—T 细胞状态—体内疗效**的工程比较数据。每个关键结论由以下层级组成：

1. 编辑效率与 CAR/TCR 流式；
2. TLA 测序验证全基因组整合分布；
3. 不同刺激次数后的 CAR MFI、mRNA、CD62L/CD45RA、T-bet/EOMES/GATA3；
4. 细胞因子、杀伤和增殖；
5. NALM6-luc 异种移植中的肿瘤生物发光、生存和骨髓 CAR-T 表型。

文章的“数据集”更接近一组图级实验矩阵，而不是可下载的单一表达矩阵。因此精读时必须把**论文公开内容**与**公共仓库数据**分开：文章和 Supplementary Information 公开，但 raw FCS/TLA/小鼠逐时间点表没有 GEO/SRA accession。

### 3.2 多大规模、覆盖哪些条件

| 模块 | 规模/重复 | 主要比较 |
|---|---:|---|
| 编辑与产量 | 多个健康供者；最高约 50 million TRAC-CAR T cells | CRISPR/AAV6 knock-in 的效率和均一性 |
| 表达均一性 | 通常 3–7 次独立实验、最多 4 位供者，依图而异 | TRAC vs RV；TRAC/B2M；内源/外源 promoter |
| 体外杀伤/增殖 | 3 位供者独立实验；部分细胞因子 n=2 | CD19⁺ 目标细胞、每周刺激或 48 h 内 1/2/4 次刺激 |
| CAR stress test | 每组约 6–20 只鼠，部分合并 2–3 次供者实验 | 2×10^5、1×10^5、5×10^4 CAR-T |
| 组织分析 | 指定时间点通常每组 7 只鼠 | 骨髓 CAR-T、肿瘤负荷、记忆/耗竭 marker |
| TLA | 代表性 TRAC-CAR 样本 | 全基因组覆盖、忠实与非忠实整合 |

供者和小鼠 n 依 figure panel 改变，论文不存在一个可相加的“总样本数”。复用时应逐图读取 Source Data，而不能将所有点视为来自同一 cohort。

### 3.3 公开页面实际提供什么

| 资源 | 内容 | 是否等于 raw dataset |
|---|---|---|
| PMC/Nature 正文 | 主要图、Extended Data、Methods | 否；多数为汇总/可视化数据 |
| Supplementary Figures PDF | CAR MFI 恢复斜率等统计和附加实验 | 否 |
| Nature Source Data | 图背后的数值表（页面提供下载入口） | 部分可重算；需逐 sheet 核对 |
| PowerPoint slides | Figs. 1–4 可编辑图 | 仅用于排版/重绘，不是原始数据 |
| 作者请求 | Data availability 指向作者 | 原始 FCS、TLA 或其他底层文件可能需申请 |

### 3.4 如何获取

#### 路线 A：综述引用和图级复核

从 Nature 页面下载 Source Data、Supplementary Figures 和需要的 figure PPT。优先读取 Source Data 的 sheet 名、列名和 n，再引用具体供者/小鼠规模。

#### 路线 B：需要原始事件或测序

向通讯作者申请 raw flow cytometry、TLA reads/coverage、逐鼠生物发光及未结构化的表达动力学数据。论文没有可以直接用 SRA Toolkit 或 GEOquery 下载的专属 accession。

#### 路线 C：复现实验而非复算数据

Methods 提供 TRAC gRNA、AAV6 donor 设计、细胞培养与 NALM6 模型。复现实验仍需取得/重建载体并执行新的伦理和生物安全审批；Source Data 不能替代试剂。

### 3.5 下载后先做什么

Source Data 应整理成：

```text
figure | donor | construct | locus | promoter | stimulation_count | time | assay | value
```

尤其要区分 CAR MFI、CAR-positive fraction 和总 CAR-positive cell number。TRAC-CAR 的优势来自均一表达和动态调控，不宜只比较单个终点 MFI。

## 4. 主要发现

- TRAC-CAR 表达较 RV-CAR 低但更均一；
- 短期杀伤和扩增可能相近，低剂量体内 stress test 才显出 TRAC-CAR 优势；
- RV-CAR 在肿瘤刺激后出现更高/更不稳定的表达和更快效应分化；
- TRAC promoter 允许抗原刺激后 CAR 内化、降解及适时再合成，类似天然 TCR 动力学；
- B2M 位点或外源强 promoter 不能完全复现 TRAC 内源调控带来的疗效；
- TRAC-19BBζ 也显示更好的体内表型，说明原则不只适用于 CD28ζ。

## 5. 状态导航的核心：表达动力学而非静态表达量

论文最重要的机制不是“TRAC 位点神奇”，而是内源调控使 CAR 在抗原刺激后经历适度下调和恢复。过高、持续和细胞间不均一的表达会增加 tonic signaling 与过快分化；过低表达则可能损害触发。目标是动态范围和恢复速度的校准。

这与综述主题高度吻合：细胞状态由**受体信号的幅度、持续时间和历史**共同决定，工程设计应优化轨迹，而非只最大化峰值。

## 6. TLA 与安全性证据能说明什么

TLA 证明代表性样本中预期 TRAC 整合占主导并揭示 AAV ITR 相关非忠实整合信号，但它不是现代意义的充分全基因组 off-target 风险评估。论文也没有长期克隆扩增或患者级插入位点随访。

## 7. 推荐图版

- **Fig. 1**：TRAC knock-in、表达均一性和 stress test，最适合开场。
- **Fig. 2**：连续刺激后的分化差异。
- **Fig. 3**：位点/启动子拆分，说明不是任何定点整合都等价。
- **Fig. 4**：受体内化—恢复动力学，是状态导航的最佳机制图。

如果只能选一组，选 Fig. 1 + Fig. 4。

## 8. 创新价值

1. 将 CAR 插入位点从安全问题提升为细胞命运控制变量。
2. 同时实现 CAR 定点表达与内源 TCR 破坏。
3. 用 stress test 揭示短期体外 assay 难以看到的持久性差异。
4. 提出“生理性受体表达动力学”这一可迁移设计原则。

## 9. 局限性

1. 无论文专属结构化公共 raw dataset，数据复算性低于 GEO 型研究。
2. 健康供者和 NSG 模型不能覆盖患者原料、免疫微环境和长期安全性。
3. TLA 为代表性样本，不能量化所有潜在 off-target/随机 AAV 整合。
4. CAR stress test 的低剂量优势不保证所有靶点和制造流程中同样成立。
5. TRAC knock-in 效率、AAV6 成本和制造复杂性也是临床约束。
6. 论文没有单细胞转录/表观组，状态判断主要依赖流式和功能。

## 10. 对本章节的作用

该文适合作为“**通过基因组位置和内源调控导航 CAR-T 状态**”的奠基研究。它把定点编辑、受体动力学、耗竭和体内效力连成因果链，并为后续 ITAM 校准和可开关 CAR 提供工程基础。

## 11. 可直接用于综述的观点

> 将 1928ζ CAR 敲入 TRAC 不仅减少随机整合，还借助内源启动子恢复抗原驱动的受体内化和再表达动力学，使 CAR-T 在低剂量压力测试中保持较慢分化和更强肿瘤控制，说明 CAR 的基因组位置本身可以成为状态导航参数（Nature 2017, Eyquem）。

## 12. 避免误读

- 不要写成“TRAC-CAR 表达越低越好”。
- 不要把短期体外相近解释为构建等价。
- 不要把 TLA 当作完整临床 off-target 安全证明。
- 不要虚构 GEO/SRA accession；本论文没有专属公共仓库数据。
- 不要把各图的独立供者和小鼠数简单相加。

## 13. 数据复用优先级

本篇优先下载 Source Data、Supplementary Figures 和 figure PPT，用于重绘表达动力学和 stress-test 图；若目标是建立机器学习 benchmark，本篇开放数据的结构化程度不足，需联系作者获取 raw FCS/逐鼠纵向表，或改用后续有 GEO 的定点整合研究。
