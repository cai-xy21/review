# 《Phenotype, specificity and avidity of antitumour CD8+ T cells in melanoma》精读

## 论文信息

- 作者：Gonçalo Oliveira、Anders R. Stromhaug、Sang-Jun Suh 等
- 期刊：Nature
- 年份：2021；596: 119–125
- DOI：10.1038/s41586-021-03704-y
- 原文：[Nature](https://www.nature.com/articles/s41586-021-03704-y)
- PubMed：[PMID 34290406](https://pubmed.ncbi.nlm.nih.gov/34290406/)
- 全文：[PMC9187974](https://pmc.ncbi.nlm.nih.gov/articles/PMC9187974/)
- 受控数据：[dbGaP phs001451.v3.p1](https://www.ncbi.nlm.nih.gov/projects/gap/cgi-bin/study.cgi?study_id=phs001451.v3.p1)
- 分析代码：[GitHub](https://github.com/kstromhaug/oliveira-stromhaug-melanoma-tcrs-phenotypes)

## 一句话结论

作者在 4 名黑色素瘤患者的 5 个肿瘤中结合 scRNA-seq、scTCR-seq、CITE-seq 与大规模 TCR 功能重建，证明真正的肿瘤反应性 CD8 T 细胞主要富集于终末耗竭和激活状态，但同时存在前体耗竭、增殖和效应记忆状态，并显示克隆丰度、表型与抗原 avidity 之间并非简单一一对应。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 发现队列 | 4 名 stage III/IV melanoma 患者 | 5 个肿瘤，细胞主要来自其中 3 个高浸润活检 |
| 单细胞 | 30,319 个 CD8 TIL | scRNA + scTCR + CITE-seq；不同细胞的数据完整性需核查 |
| 功能确认 TCR | 134 条 tumour-specific | 对应 7,451 个单细胞 |
| 状态 | TTE、TAct、TPE、TProl、TEM | 是肿瘤特异细胞中的五类主要表型 |
| 外周血筛选 | 491 条候选，414 条成功重建 | 216 tumour-specific、61 nonspecific、137 nonreactive |
| 数据访问 | dbGaP 受控 | 不能匿名直接下载；需机构和 Data Access Request |
| 其他数据 | WES、RNA、HLA-I immunopeptidomics、功能实验 | 不应假定所有组学均公开逐文件下载 |

## 1. 研究要解决的问题

肿瘤内 CD8 T 细胞常按耗竭 marker 注释，但 PD-1、TIM-3 等并不能单独证明肿瘤特异性。论文直接问：

1. 哪些 TIL TCR 真正识别自体肿瘤；
2. 经功能验证的 tumour-specific cells 处于哪些单细胞状态；
3. 表型、克隆扩增、功能 avidity 与治疗相关性如何对应；
4. 外周血是否能找到与肿瘤相连的特异克隆。

其关键创新是用功能重建给单细胞表型加上“真实抗原反应性”标签，而不是用耗竭表达推断特异性。

## 2. 方法框架

### 2.1 单细胞多组学

肿瘤 CD8 TIL 进行：

- scRNA-seq；
- 配对 scTCR-seq；
- CITE-seq 表面蛋白；
- 克隆丰度与状态分析。

### 2.2 TCR 重建和功能筛选

作者从单细胞或血液候选克隆中重建 TCR，转导报告细胞/原代 T 细胞，与自体黑色素瘤细胞系共培养，以 CD137 等激活读出判断肿瘤反应性。并通过肿瘤全外显子组、RNA、HLA-I 免疫肽组和候选肽实验寻找抗原。

因此：

- tumour-specific 是实验功能标签；
- nonspecific 指在对照条件也反应；
- nonreactive 指在所用检测条件未观察反应，不能绝对证明永远无特异性。

## 3. 数据规模与图谱组成

### 3.1 发现队列和取材

| 层级 | 规模/内容 |
|---|---|
| 患者 | 4，Pt A–D |
| 临床 | stage III/IV melanoma |
| 肿瘤样本 | 5 |
| 单细胞主体 | 30,319 个 CD8 TIL |
| 主要单细胞来源 | 3 个 T 细胞浸润较高的肿瘤活检 |
| 分子层 | GEX、TCR、CITE-seq |

4 名患者、5 个肿瘤与 3 个主要 scRNA 样本是三个不同层级。不能把 5 个肿瘤写成 5 名患者，也不能假设每名患者都同等贡献 30,319 个细胞。

### 3.2 经功能确认的肿瘤特异 TCR

论文最终将 134 条功能确认的 tumour-specific TCR 对应到 7,451 个肿瘤内单细胞。肿瘤特异细胞主要分成：

| 状态 | 含义 |
|---|---|
| TTE | terminally exhausted，终末耗竭 |
| TAct | activated，激活 |
| TPE | progenitor exhausted，前体耗竭 |
| TProl | proliferating，增殖 |
| TEM | effector memory，效应记忆 |

约 78.3% 的 clonotypes 明显偏向 TTE/TAct。重要的是，少数同一 clonotype 可跨多个状态，说明克隆身份与即时功能状态不是同一变量。

134 条 TCR 是“独特受体/克隆”的规模，7,451 是携带这些受体的细胞规模，不能相加或互换。

### 3.3 外周血 TCR 功能筛选

作者从血液候选中选择 491 条 TCR，成功重建并检测 414 条：

| 功能结果 | TCR 数 |
|---|---:|
| tumour-specific | 216 |
| nonspecific | 61 |
| nonreactive | 137 |
| 合计成功重建 | 414 |

其中 51 条 tumour-specific 和 16 条 nonreactive blood TCR 可在 TIL 数据中找到对应克隆。这个配对帮助判断哪些循环克隆与肿瘤内状态相连。

分母必须写清：

- 491：计划/候选重建；
- 414：成功进入功能检测；
- 216：在实验条件下判为肿瘤特异。

### 3.4 独立免疫检查点治疗扩展

研究还分析 16 名接受 ICB 的黑色素瘤患者，并对其中 8 名患者的 94 个扩增 TCR 进行功能验证，用于检验发现队列规律。该队列不是 30,319 个发现单细胞的组成部分，统计时应分开。

### 3.5 受控数据：dbGaP

论文 Data availability 将单细胞 RNA/TCR/CITE 数据指向：

[phs001451.v3.p1](https://www.ncbi.nlm.nih.gov/projects/gap/cgi-bin/study.cgi?study_id=phs001451.v3.p1)

这是论文引用的固定版本。dbGaP 当前可能显示 v3 已被后续版本取代，父项目也可能包含其他样本；当前 study 页面显示的全部 consented subjects 数不应直接当作本文 4 名发现队列患者数。

严格复现：

- 使用 phs001451.v3.p1；
- 在申请中明确本论文和所需 dataset；
- 下载 manifest 后再使用 dbGaP 授权工具。

现在重新申请时，可查看最新版本的变量和文件是否已变更，但必须记录版本差异。

### 3.6 dbGaP 访问流程

该数据不能用 wget 匿名下载。一般需要：

1. PI 具有 eRA Commons 账号；
2. 研究机构具有相应授权与 signing official；
3. 在 dbGaP Authorized Access 提交 Data Access Request；
4. 写明研究目的、数据安全计划和使用期限；
5. 获批后签署/接受 Data Use Certification；
6. 使用 dbGaP 下载清单和授权凭证取得文件。

申请前建议在 public study page 先核查：

- consent group；
- data use limitation；
- subject/sample 数；
- phenotype、sequence、SRA 和 cloud delivery 文件；
- 版本 v3 与当前版本差异。

不要在未获授权时通过第三方转载受控单细胞文件。

### 3.7 代码和分析复现

[GitHub 仓库](https://github.com/kstromhaug/oliveira-stromhaug-melanoma-tcrs-phenotypes) 提供论文分析代码。代码本身不包含受控患者矩阵。

建议的复现目录分层：

- raw_controlled：dbGaP 原始文件，只在授权环境；
- processed_controlled：表达、TCR、CITE 对齐对象；
- functional_screen：TCR 重建与功能结果表；
- public_code：GitHub 固定 commit；
- metadata：患者、样本、肿瘤、时间点和 consent 对应。

### 3.8 其他组学数据

研究还生成或使用：

- 肿瘤 WES；
- 肿瘤 RNA-seq；
- HLA-I immunopeptidomics/mass spectrometry；
- 自体肿瘤细胞系；
- pMHC/肽刺激与 avidity 实验。

论文说明部分其他数据需向作者申请。不能在没有 dataset page 支持时写成“全部原始质谱和临床数据已公开”。对于综述数据表，建议逐项标注 public、controlled 或 available on request。

## 4. 主要生物学发现

### 4.1 肿瘤特异性富集于耗竭/激活状态

功能确认的 tumour-specific clones 明显富集 TTE 和 TAct，而病毒/旁观者 T 细胞更偏记忆样状态。这使终末耗竭成为“长期肿瘤抗原经历”的富集表型，但不是单细胞层面的完美特异性检测器。

### 4.2 同一克隆可以跨状态

一些 tumour-specific clonotype 同时包含 TPE、TAct、TTE 或增殖细胞，提示状态是可变表型层，而 TCR 是较稳定的克隆身份层。这正适合用于研究状态转变。

### 4.3 Avidity 与表型并非简单线性

高 avidity 受体可能更强驱动激活/耗竭，但克隆丰度、抗原表达、HLA 呈递、微环境与取样时间共同决定观察到的状态，不能只凭 TCR avidity 预测命运。

## 5. 关键图表怎么读

- 状态 UMAP：只显示转录相似性，真正特异性来自功能实验标签。
- clonotype/state 图：同克隆跨状态支持关联，不提供方向。
- 血液筛选漏斗：每一步分母不同，失败重建不能直接归为 nonreactive。
- avidity 图：体外反应条件与体内肿瘤生态位不同。

## 6. 创新点

1. 将大规模 TCR 功能验证与单细胞状态一一对应。
2. 明确区分 tumour-specific、nonspecific 与 nonreactive。
3. 同时连接肿瘤和血液克隆。
4. 将 GEX、TCR、表面蛋白、抗原发现和 avidity 置于同一框架。

## 7. 局限性

1. 发现队列仅 4 名患者，患者和肿瘤异质性很大。
2. 大部分数据受控，匿名复现门槛高。
3. 自体肿瘤细胞系可能丢失体内抗原或呈递状态。
4. nonreactive 可能是检测灵敏度、HLA 或靶细胞状态造成的假阴性。
5. 单时间点数据不能判断 TPE 到 TTE 的方向和速率。
6. 五类状态由该队列定义，跨癌种泛化需再验证。

## 8. 对本综述的作用

该论文是“quantitatively characterizing cell phenotypes, functions and molecular markers”部分的重要桥梁：

- 表达 marker 只能给出概率；
- TCR 提供克隆身份；
- 功能重建提供真实肿瘤反应性；
- avidity 提供可量化驱动力。

它也为细胞治疗提出筛选原则：不能只选最扩增或最耗竭克隆，应同时考虑特异性、受体强度、可持久状态和跨状态能力。

## 9. 可直接写进综述的表述

> 将 30,319 个黑色素瘤 CD8 TIL 的单细胞表型与 134 条功能确认的肿瘤特异 TCR 对齐后可见，肿瘤反应性主要富集于终末耗竭和激活状态，但同一克隆可跨越前体耗竭、增殖和效应记忆状态，说明克隆身份、即时功能状态与受体 avidity 必须联合量化。

## 10. 最容易误读的地方

- PD-1/耗竭表型不等于功能验证的 tumour specificity。
- 134 是 TCR 数，7,451 是对应细胞数。
- 491 是候选，414 才成功进入功能检测。
- dbGaP 父研究受试者总数不等于本文发现队列 4 人。
- 受控数据没有公开直链；代码仓库也不包含患者矩阵。
