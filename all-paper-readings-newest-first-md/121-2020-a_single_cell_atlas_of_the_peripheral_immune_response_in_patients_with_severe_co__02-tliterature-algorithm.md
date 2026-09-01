# Algorithm Report 008

## Paper
A single-cell atlas of the peripheral immune response in patients with severe COVID-19

## 算法视角定位
这篇文章是感染队列中的高价值应用型 atlas 研究。它的重要性在于定义了 severe infection 下的 cohort-level immune-state analysis 任务，而不是提供新算法。它把单细胞状态、患者异质性和临床严重度第一次较系统地放进同一分析框架里。

## 核心算法贡献
- 没有提出新的基础模型。
- 主要贡献是把 PBMC single-cell analysis 与疾病严重程度和患者层临床信息关联起来。
- 为 severity-aware immune modeling、outcome association 和 clinical transfer 提供真实 benchmark 场景。
- 它说明感染研究中的关键任务不是单纯找 marker，而是建立可解释的 severity-linked representation。
- 具体算法链路可概括为：`PBMC scRNA preprocessing -> clustering/annotation -> donor/severity-aware abundance and DEG comparison -> state/module interpretation -> clinical severity association`。这条链路后来成为 COVID-19 与其他 severe infection 单细胞队列的常见模板。

## 关键方法学价值
- 为 severe infection immune modeling 提供真实 cohort 数据结构。
- 把 donor、severity、outcome 和 cell state 同时纳入分析对象。
- 为 infection-driven manifold shift 建模提供早期参考案例。
- 为患者层风险评分与细胞层特征聚合之间的建模提供问题设定。

## 相比既有工作的改进
它比早期描述性免疫学更进一步，把单细胞状态放入 cohort 和临床严重度框架中，使感染免疫重塑可量化、可比较。相比仅比较病例/对照的分析，它更强调 severity continuum 与患者异质性。

## 适合抽象出的计算任务
- severity-conditioned donor representation
- T-cell state to outcome modeling
- infection-driven manifold shift estimation
- clinical severity transfer learning
- patient-level immune risk scoring
- disease-stage-aware trajectory inference

## 数据/代码可用性
- DOI：https://doi.org/10.1038/s41591-020-0944-y
- 数据：7 名住院 COVID-19 患者、6 名健康对照，13 个 PBMC 样本，76,533 个高质量单细胞。
- 正式 accession：GEO `GSE150728`；论文 Data availability 明确说明 raw sequencing data 在 GEO，processed matrices/metadata/embeddings 可从 COVID-19 Cell Atlas 和 CZ CELLxGENE 获取。
- 代码：<https://github.com/ajwilk/2020_Wilk_COVID>
- 代码输入/输出：输入 `.h5ad`/matrix 与 patient metadata，输出 cluster annotations、severity comparison、cell-frequency/DEG figures 与复现图表。
- 复用性：高。
- 代码可获得性：中高。

## 对新算法开发的贡献程度
- 评级：**中等（任务场景价值）/低（直接算法创新）**
- 原因：是高价值应用场景和 benchmark 背景，但不是新算法论文。

## 对我们方法论文的启发
- severity-conditioned donor representation
- T-cell state to outcome modeling
- infection-driven manifold shift estimation
- 从 cell-level 特征稳定提升到 patient-level severity prediction
- 在感染背景中区分 generic inflammation 与 disease-specific immune dysregulation

## 方法局限对建模的提醒
- 外周血与病灶组织之间存在显著 domain gap。
- 严重度标签容易混入治疗时点和基础疾病效应。
- 感染进程是动态的，横截面样本不应被过度解释为完整轨迹。

## 总结
这篇文章最大的算法价值在于，它给出了“感染严重度相关免疫状态建模”的真实数据场景。对方法研究而言，它更像 severe infection immune modeling 的任务定义论文，而不是方法创新论文。
