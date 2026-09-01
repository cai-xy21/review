# 《Single-cell multiomics dissection of basal and antigen-specific activation states of CD19-targeted CAR T cells》精读

## 论文信息

- 作者：Zhen Bai、Boyang Feng、Rong Fan 等
- 期刊：*Journal for ImmunoTherapy of Cancer*
- 年份：2021；9:e002328
- DOI：[10.1136/jitc-2020-002328](https://doi.org/10.1136/jitc-2020-002328)
- PubMed：[PMID 34006631](https://pubmed.ncbi.nlm.nih.gov/34006631/)；[PMC8137188](https://pmc.ncbi.nlm.nih.gov/articles/PMC8137188/)
- 复用外部数据：[BioProject PRJNA554339](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA554339)

## 一句话结论

健康供者与两份儿童 ALL CAR T 产品的 23,349 个单细胞显示，制造后的 basal state 已混合激活、细胞毒和耗竭程序，而 CD19 特异刺激引发的反应强度和 MHC-II 程序具有显著供者/临床产品差异。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 来源产品 | 3 | 健康供者 1、CR 患者产品 1、NR 患者产品 1 |
| 总细胞 | 23,349 | basal + CD19 + MESO 三条件合计 |
| CD19 刺激子集 | 9,744 | 用于抗原特异响应分析 |
| 模态 | scRNA + 10 个表面蛋白 | scFTD-seq microwell |
| 刺激 | 6 h，1:1 effector:target | 3T3-CD19 与 3T3-MESO 对照 |
| 测序 | HiSeq 4000，20–40k reads/cell | 非 10x 平台 |
| 新生成数据可用性 | 合理申请获取 | 文中未给新 23,349 细胞的公共 accession |
| PRJNA554339 | 外部复用数据 | Boroughs 等两份 4-1BB CAR T，不是本研究主数据 |

## 1. 研究要解决的问题

CAR T 在没有抗原时是否已经处于不同激活/耗竭状态？遇到特异抗原与非特异靶细胞后，不同供者/临床产品如何分叉？作者用低成本 microwell 多组学同时测量转录组和有限表面蛋白。

## 2. 方法框架

- 三份来源产品：健康供者（HD）、完全响应者（CR）和非响应者（NR）；
- basal、不加靶细胞；
- 与 3T3-CD19 共培养 6 h，作为特异刺激；
- 与 3T3-MESO 共培养 6 h，作为非特异细胞接触对照；
- scFTD-seq 获取 RNA + 10 个 ADT；
- 比较来源、刺激条件、细胞亚群和功能程序。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这是一个 3 个来源产品 × 3 种刺激环境的短时状态响应图谱：

1. **basal layer**：评估 tonic signaling、制造诱导激活和潜在耗竭；
2. **CD19-specific layer**：3T3-CD19 触发 CAR 信号；
3. **nonspecific-contact layer**：3T3-MESO 控制共培养/细胞接触效应；
4. **RNA layer**：激活、细胞毒、耗竭、MHC-II 与记忆程序；
5. **ADT layer**：有限的 10 个表面标志物；
6. **来源标签**：HD、临床 CR、临床 NR。

这是 ex vivo 6 小时响应，不是输注后患者纵向图谱；CR/NR 各只有一个产品，临床标签与个体差异不可分离。

### 3.2 多大规模、表面 panel 是什么

| 层级 | 规模/内容 | 应如何理解 |
|---|---:|---|
| 来源产品 | 3 | 1 HD + 1 CR + 1 NR |
| 全部细胞 | 23,349 | 三条件与三来源合计 |
| CD19-specific 子集 | 9,744 | 特异刺激分析口径 |
| 刺激时间 | 6 小时 | 描述早期响应，不代表长期命运 |
| E:T 比 | 1:1 | 3T3 target 共培养 |
| ADT | 10 | CD4、CD8、CD45RO、CD45RA、CCR7、CD62L、HLA-DR、CD38、CD69、4-1BB |
| reads/cell | 20,000–40,000 | HiSeq 4000 |

scFTD-seq 为 microwell-based 平台；其捕获效率、barcode 结构与 10x 不同。与后续 10x atlas 合并前应做平台留出验证，而非直接整合后比较比例。

### 3.3 新生成数据在哪里

论文 Data availability 写明研究产生的数据可在合理请求下向作者获取，但没有为这套 **23,349-cell HD/CR/NR 数据**提供可核验的 GEO/SRA accession。因此：

- 无法仅凭公共 accession 直接下载主数据；
- 需要联系通讯作者，明确申请 raw FASTQ、gene-by-cell matrix、ADT counts、cell metadata 和 condition labels；
- 申请时应询问是否可提供 scFTD barcode whitelist、demultiplex/QC 脚本和每个来源的细胞数。

这类“available upon reasonable request”数据的可得性取决于作者回复与数据共享条款，报告中不能写成 open access。

### 3.4 PRJNA554339 到底是什么

论文引用 `PRJNA554339` 作为外部 CAR T 单细胞数据。该 BioProject 对应 Boroughs 等研究的两份 4-1BB CAR T donor samples，用于外部比较/验证；它不是本论文新生成的 HD、CR、NR 三来源 atlas。

若需要替代性公开数据，可下载 PRJNA554339，但必须标记为 external dataset，不能声称已获得本文的 23,349 个细胞。数据谱系建议写成：

- primary generated data：on request；
- external reused data：PRJNA554339；
- paper figures/supplement：公开，可用于核对 cluster/marker 与细胞数。

### 3.5 获取后先做什么

1. 检查 3 个来源 × 3 条件的细胞数是否平衡；
2. 明确 raw/filtered 矩阵、ADT normalization 与 doublet 过滤；
3. 将 CR 与 NR 视为两个个案，不做患者群体统计推断；
4. 分别比较 CD19 vs basal、MESO vs basal、CD19 vs MESO；
5. 保留 donor/source 为主效应，避免把来源差异误写成抗原响应。

## 4. 主要发现

即使无外源抗原，CAR T 产品也具有混合的激活、细胞毒和耗竭/tonic-signaling 程序。CD19 刺激后，健康供者产品显示更强激活及 MHC-II 相关程序；临床产品之间反应幅度和状态构成不同。

## 5. 与“状态导航”最相关的分析

研究提示优化不能只测刺激后的最大细胞因子：basal state 决定后续可响应空间，而特异刺激与非特异细胞接触需要分开。它为制造放行加入“basal + standardized challenge”双状态测试提供依据。

## 6. 推荐图版

- 实验设计与 scFTD-seq 示意图。
- basal、CD19、MESO 三条件 UMAP/组成。
- 激活、耗竭、细胞毒和 MHC-II 程序对比。

## 7. 创新价值

在较早阶段就用 RNA+蛋白单细胞多组学比较 tonic 与 antigen-specific activation，并加入非特异细胞接触对照。

## 8. 局限性

仅 3 个来源且 CR/NR 各 1 人；6 小时 ex vivo；10-ADT panel 较窄；主数据未开放 accession；无法把临床反应差异与供者个体差异分离。

## 9. 对本章节的作用

适合“t cell at the start point”：强调输注前 basal state 和标准化挑战后的可塑性共同定义产品起点。

## 10. 可直接用于综述的观点

> CAR T 制造品在无抗原时已呈混合激活、细胞毒与耗竭程序，而 6 小时 CD19 特异刺激揭示不同来源产品的响应能力差异，说明产品放行应同时表征 basal state 与 challenge-induced state（JITC 2021, Bai）。

## 11. 避免误读

- 不要把 1 个 CR 与 1 个 NR 产品写成临床预测队列。
- 不要把 PRJNA554339 当作本文新生成 23,349 细胞的数据入口。
- 不要把 6 小时 ex vivo 反应写成患者体内长期状态转换。

