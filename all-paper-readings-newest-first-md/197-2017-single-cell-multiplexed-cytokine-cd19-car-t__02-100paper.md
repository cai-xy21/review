# 《Single-cell multiplexed cytokine profiling of CD19 CAR-T cells reveals a diverse landscape of polyfunctional antigen-specific response》精读

## 论文信息

- 作者：Qing Xue、Eleonora Bettini、Patrick Paczkowski 等
- 期刊：*Journal for ImmunoTherapy of Cancer*
- 年份：2017；5: 85；发表于 2017 年 11 月 21 日
- DOI：10.1186/s40425-017-0293-7
- 原文：[JITC](https://jitc.bmj.com/content/5/1/85)
- PubMed：[PMID 29157295](https://pubmed.ncbi.nlm.nih.gov/29157295/)
- 开放全文：[PMC5697351](https://pmc.ncbi.nlm.nih.gov/articles/PMC5697351/)
- 补充材料：PMC 正文末尾的 Additional files 1–8

## 一句话结论

作者用 12,720 微室、16-plex 抗体条码芯片测量 4 位健康供者来源 CD19 CAR-T 的单细胞分泌，发现抗原特异多功能组合和显著供者差异；但逐细胞强度矩阵未存入公共数据库，只能向作者申请。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 供者 | 4 位健康供者，2 女 2 男，26–51 岁 | 不是患者输注产品，不能直接推断临床结局 |
| CAR-T 制造 | PBMC；CD3/CD28 激活；CD19-BB-ζ 慢病毒；扩增 10 d | 收获后 CAR⁺磁珠富集，纯度 >90% |
| 条件 | anti-CAR beads 与 IgG beads 对照 | 先刺激 6 h，再在芯片中孵育 16 h |
| 芯片 | 24 × 530 = 12,720 个微室/芯片 | 每室约 1.2 nL；不是每室都有单细胞 |
| 单细胞输入 | 每芯片约需 1×10⁴ cells；文中称 thousands of cells | 没有公开每供者最终有效单细胞数 |
| 分泌面板 | 16 proteins | 单细胞蛋白分泌强度，不是转录表达 |
| 表型信息 | CD4、CD8 荧光及微室细胞数 | 与条码荧光合并形成逐微室记录 |
| 公开数据 | 8 个补充 PDF + 正文 | 无 cell-by-protein CSV、无图像 TIFF、无代码仓库；数据须合理请求 |

## 1. 研究要解决的问题

CAR-T 产品是高度异质的活细胞群。bulk IFN-γ、细胞毒实验或少数胞内细胞因子无法回答：到底哪些单细胞同时部署多个效应程序、这些组合是否因供者而异、怎样把 2^16 种潜在分泌组合可视化。

## 2. 方法框架

### 2.1 CAR-T 产品

4 位健康供者的单采 PBMC 经 anti-CD3/anti-CD28 beads 激活，转导 CD19-BB-ζ 慢病毒，在改良 X-VIVO 15 中扩增 10 d；随后以抗 CAR-PE 和 anti-PE beads 富集 CAR⁺细胞，流式确认纯度 >90%，冻存后复苏过夜用于实验。

### 2.2 SCBC 芯片与读出

- PDMS 微室阵列有 24 列，每列 530 室，共 12,720 室；单室 20 μm × 2,060 μm × 20 μm，约 1.2 nL。
- 玻片预制 16 条抗体条码；显微镜先记录每室细胞数和 CD4/CD8 表型。
- anti-CAR 或 IgG beads 与 CAR-T 按 1:4 混合；刺激 6 h 后装芯片，再孵育 16 h。
- 累积的分泌蛋白被捕获并以 GenePix 扫描；图像与条码信号叠加得到逐室数据。
- IsoSpeak/CytoSpeak 用于背景校正、PSI、PCA、viSNE、polyfunctional heat map 和 PAT-PCA。

## 3. 数据规模与数据组成

### 3.1 数据到底是什么

最小数据单元是一个微室，而不是一个“细胞文件”。每条有效记录理论上包含：供者、刺激条件、微室编号、细胞数、CD4/CD8 表型、16 个蛋白荧光强度、背景/SNR 和是否多功能。空室用于背景估计，多细胞室应从单细胞分析中排除。

论文还包含 bulk ELISA/16-plex supernatant、ICS 验证和抗体标准曲线。这些验证数据与主单细胞矩阵不同，不应混在一起统计。

### 3.2 生物样本与技术规模

| 层级 | 规模/组成 | 应如何理解 |
|---|---:|---|
| 生物供者 | 4 | 真正独立的生物重复仅 4 |
| 供者构成 | 2 女、2 男；26–51 岁 | 无疾病、治疗和临床结局 |
| 细胞亚群 | total CD3、CD4、CD8 | 主文重点是 CD4 与 CD8 CAR⁺分泌谱 |
| 刺激 | anti-CAR 与 IgG | anti-CAR 是受体交联模型，不是完整靶细胞免疫突触 |
| 微室容量 | 12,720/芯片 | 芯片容量不等于最终有效细胞数 |
| 所需输入 | 约 10,000 cells/芯片 | 论文未公开过滤后的逐组 n |
| 蛋白维度 | 16 | 组合空间最大 65,536 种，实际观察远少于此 |

### 3.3 16-plex 面板

论文将分子分为四类：

- 抗肿瘤效应：granzyme B、IFN-γ、MIP-1α、TNF-α；
- 刺激：GM-CSF、IL-2、IL-8；
- 调节：IL-4、IL-13、IL-22；
- 炎症：IL-6、IL-17A；
- 面板还包含用于完整 16 维分析的其余条目，精确抗体对、供应商和工作浓度见 Additional file 1，而不是仅从正文摘要推断。

报告中若要完整复刻面板，应直接抄录 Table S1 的 16 个 capture/detection antibody pairs；不要把后续 32-plex 临床论文的面板混入本研究。

### 3.4 公开文件有什么

PMC 提供 8 个补充 PDF：

| 文件 | 内容 | 是否原始数据 |
|---|---|---|
| Additional file 1 | Table S1，16-plex 抗体面板 | 否，试剂表 |
| Additional file 2 | PAT-PCA 数据变换示意 | 否，方法图 |
| Additional file 3 | 4 供者、CD3/CD4/CD8 的分泌概览 | 否，汇总图 |
| Additional file 4 | 5–5,000 pg/mL 标准曲线与特异性验证 | 否，图形化验证结果 |
| Additional file 5 | SCBC 与 ICS、重复实验一致性 | 否 |
| Additional file 6 | 单细胞与 bulk 刺激比较 | 否 |
| Additional file 7 | CD8 PCA 与传统展示 | 否 |
| Additional file 8 | viSNE 展示 | 否 |

论文 Data and materials 声明为“可向通讯作者合理请求”。截至 2026 年 8 月，没有找到 GEO、SRA、PRIDE、ImmPort、Zenodo/Figshare accession，也未找到公开的 CytoSpeak/PAT-PCA 代码版本。

### 3.5 如何获取

#### 路线 A：下载公开论文和补充材料

进入 [PMC 全文](https://pmc.ncbi.nlm.nih.gov/articles/PMC5697351/) 的“Additional files”区逐个下载。公开补充材料足以恢复芯片几何、抗体面板、验证设计和图中主要组合，但不足以重算每个细胞的 PSI 或重新聚类。

#### 路线 B：申请原始逐细胞数据

建议明确请求：

1. 4 donors × CD4/CD8 × anti-CAR/IgG 的逐微室矩阵；
2. 16 通道原始/背景校正 RFU、SNR、阳性阈值；
3. 单细胞、空室和多细胞室注释；
4. 明场、CD4、CD8 图像或至少图像提取的 cell-count 表；
5. 芯片批次、重复批次、标准曲线和异常值剔除规则；
6. IsoSpeak/CytoSpeak 版本及 PAT-PCA 的实现参数。

### 3.6 拿到数据后先做什么

先以供者为随机效应，检查每供者、每芯片的有效单细胞数和检测下限。逐细胞点不能代替供者重复。建议重做三种表征：二值 secretion pattern、连续 RFU、以及多功能度（阳性分子数），再比较结果是否一致。

## 4. 主要结果

1. anti-CAR 刺激相对 IgG 在 bulk IFN-γ 上约提高 1,000 倍，支持刺激特异性。
2. 4 位供者均出现抗原特异分泌增加，但供者间多功能性差异明显。
3. donor 1 和 donor 4 多功能性最高，donor 2 最低；这说明相同制造流程仍产生不同功能景观。
4. 多功能组合以 granzyme B、MIP-1α、IFN-γ、IL-8、TNF-α 等效应/刺激分子为主。
5. donor 1/4 出现 GM-CSF + granzyme B + IFN-γ + IL-8 + IL-13 + MIP-1α + TNF-α 的 7-plex 组合。
6. polyfunctional heat map 与 PAT-PCA 比普通 PCA/viSNE 更直接显示组合成分和供者差异。

## 5. 多功能性如何量化

论文把共分泌 ≥2 种蛋白的细胞视为 polyfunctional，并计算 PSI。二值阳性组合先形成 16 维 0/1 向量，再按多功能度加权频率用于 PAT-PCA。这个过程会放大高阶组合，因此分析时应同时保留未加权的细胞比例，避免少数高阶组合支配结果。

## 6. 对“T cell state navigation”的意义

该工作展示了一个可作为制造反馈的功能坐标：不是问“细胞像 TCM 还是 TEM”，而是问单个 CAR-T 在遇到抗原时能同时调用多少、哪些功能。它连接了细胞状态与功能，但没有揭示产生差异的转录、表观或代谢驱动，也没有实时连续追踪同一细胞。

## 7. 推荐图版

- **Fig. 1**：芯片、显微与 16-plex 工作流，适合技术概览。
- **Fig. 2**：刺激前后和供者间 PSI 差异。
- **Fig. 4–5**：CD4/CD8 polyfunctional heat map 与 PAT-PCA，是本文最具辨识度的图。
- **Supplementary Fig. S6–S7**：抗体和平台验证，适合方法补充。

## 8. 创新价值

1. 将微室单细胞分泌测量扩展到 CAR-T 产品。
2. 同时记录细胞数、CD4/CD8 表型与 16 种真分泌蛋白。
3. 引入面向组合功能的可视化，而非只对连续矩阵降维。
4. 揭示同流程健康供者产品仍有强供者效应。

## 9. 局限性

1. 仅 4 位健康供者，没有患者、疗效或毒性结局。
2. anti-CAR beads 不复制靶细胞抗原密度、黏附和免疫突触。
3. 16 h 累积信号没有分泌动力学，不能区分早峰和持续输出。
4. 预选 16 分子面板遗漏大量功能和抑制因子。
5. 芯片内细胞独立但供者不独立，统计容易伪重复。
6. 逐细胞原始矩阵、图像和软件未公开，复现受限。

## 10. 对本综述章节的作用

适合放在“quantitatively characterizing cell phenotypes, functions and molecular markers”部分，作为单细胞功能组学的早期代表；与第 81 篇衔接时，可说明本研究先建立 16-plex 方法，第 81 篇再用 32-plex 将其推向临床关联。

## 11. 可直接用于综述的观点

> 16-plex 单细胞分泌芯片显示，即使由相似流程制造的健康供者 CD19 CAR-T 产品，也具有截然不同的抗原诱导多功能组合，说明细胞治疗产品的功能状态不能由 bulk IFN-γ 或单一表型比例概括（JITC 2017, Xue）。

## 12. 数据复用建议

若能取得原始矩阵，最有价值的分析不是重新画 t-SNE，而是建立供者留出的组合稀疏模型，评估哪些功能组合稳定、哪些受芯片批次影响，并将连续强度与二值组合分开。现有公开文件适合方法史与面板设计，不足以做可靠的数值再分析。

## 13. 避免误读

- 不要把 12,720 个微室写成 12,720 个有效单细胞。
- 不要把 4 位供者的成千上万细胞视为大样本临床证据。
- 不要把 anti-CAR bead 刺激等同于真实肿瘤细胞杀伤。
- 不要把 Additional files 称为原始单细胞数据下载包。
