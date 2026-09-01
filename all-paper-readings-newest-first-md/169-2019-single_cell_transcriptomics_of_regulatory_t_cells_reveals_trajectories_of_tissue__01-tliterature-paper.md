# 063. Single-cell transcriptomics of regulatory T cells reveals trajectories of tissue adaptation

## 基本信息
- 年份：2019
- 期刊：Immunity
- DOI：https://doi.org/10.1016/j.immuni.2019.01.001
- PMID：30737144
- 主题：Treg tissue adaptation；pseudotime；gene kinetics；RNA velocity；TCR clonality

## 为什么重要
这篇文章把 Treg 从静态 subtype catalog 推进到 tissue adaptation trajectory：Treg 在 lymphoid tissue 到 non-lymphoid tissue 的迁移和适应过程中，会经历连续状态变化，而不是简单分成“淋巴组织 Treg”和“组织 Treg”。对 T 细胞-人群免疫力算法而言，它提供了 tissue-conditioned trajectory modeling 的经典任务。

## 数据与研究设计
- 样本对象：小鼠 CD4+ regulatory T cells 和 memory T cells；另有人类 blood/non-lymphoid tissue CD4+ T-cell comparison
- 物种/器官：主要为 Mus musculus；spleen、skin、colon、skin-draining lymph node、mesenteric lymph node；人类部分用于 conserved tissue program 对照
- 细胞规模：约 35,000 CD4+ Treg/Tmem cells
- 模态：Smart-seq2 scRNA-seq、10x Chromium scRNA-seq、TraCeR TCR reconstruction、velocyto RNA velocity、体内 melanoma challenge validation
- 研究目标：解析 Treg 从 lymphoid tissue 到 non-lymphoid tissue 的适应轨迹、基因程序启动顺序与 clonal context

## 核心亮点
1. **连续适应轨迹**：使用 BGPLVM latent variables 组织 Treg tissue adaptation，而不只做 cluster comparison。
2. **gene kinetic ordering**：沿 latent axis 拟合 sigmoidal curves，估计不同 gene programs 的启动时间。
3. **方向性证据**：用 RNA velocity 支持 NLT-like/eTreg 向更明显 non-lymphoid phenotype 过渡。
4. **受体与轨迹连接**：用 TraCeR 从 scRNA-seq 重建 TCR，提供 clonality 层信息。

## 核心贡献
- 定义了 Treg tissue adaptation 的 transitional subpopulations，包括 lymphoid tissue 中的 NLT-like Treg 和 non-lymphoid tissue 中的 LT-like Treg。
- 证明 skin 与 colon 适应轨迹具有相似 gene kinetic order，同时保留组织特异 homing/metabolic programs。
- 将 migration、glycolysis、proliferation、cytokine production、fatty-acid homeostasis 等过程放到相对时间顺序中。
- 提供 mouse-human conserved non-lymphoid tissue CD4/Treg signature，对人类组织免疫转化研究有参考价值。

## 与 T 细胞-人群免疫力的关系
Treg 决定免疫耐受、组织稳态和炎症阈值。该文说明，个体免疫底盘中的 Treg 功能不能只看频率，还要看其组织适应程度、迁移预激活状态和局部代谢程序。人群免疫算法若要解释组织炎症、肿瘤免疫或自身免疫，应把 Treg adaptation 当作连续过程建模。

## 文章中的算法/分析流程
### 1. State discovery and annotation
作者对 Treg/Tmem scRNA 数据进行 QC、normalization、clustering 和 marker annotation，识别 lymphoid、non-lymphoid、activated/effector-like、tissue-homing 等状态。

### 2. BGPLVM latent trajectory
Bayesian Gaussian Process Latent Variable Model 用于 mLN-colon 和 bLN-skin Treg cells 的低维连续建模。最相关 latent variable 被解释为 lymphoid-to-non-lymphoid adaptation axis。

### 3. Sigmoidal gene kinetics
沿 latent axis 对每个基因拟合 sigmoidal response，并估计 activation point；随后按 GO biological process 聚合，比较 skin/colon 适应过程的先后顺序。这是本文最值得算法化复用的部分。

### 4. RNA velocity and TraCeR
velocyto 提供 spliced/unspliced 方向性证据；TraCeR 从 full-length scRNA-seq 中恢复 TCR，用于分析 clonality 与 tissue state 的关系。本文把 TCR 用作解释层，但尚未形成 clone-aware trajectory model。

## 对算法工作的启发
1. **Tissue adaptation VAE**：显式拆分 shared Treg identity、tissue-specific adaptation 和 activation/perturbation factors。
2. **Clone-aware adaptation trajectory**：把同一 TCR clonotype 的跨组织分布作为 trajectory constraint。
3. **Cross-species alignment with uncertainty**：mouse-to-human Treg translation 不能只做 one-to-one ortholog mapping。
4. **Kinetic program modeling**：从 DEG 走向 gene program timing，更适合解释干预窗口。

## 数据可用性
- Raw scRNA-seq accessions：ArrayExpress `E-MTAB-6072`、`E-MTAB-7311`
- Single Cell Expression Atlas：<https://www.ebi.ac.uk/gxa/sc/experiments/E-MTAB-7311>
- Processed data：Figshare project `Treg_scRNA-seq` <https://figshare.com/projects/Treg_scRNA-seq/38864>
- 作者数据入口：<http://www.teichlab.org/data/>
- 代码仓库：<https://github.com/tomasgomes/Treg_analysis>
- 数据性质：mouse spleen、lymph nodes、skin、colon CD4+ Treg/Tmem；约 35,000 cells；包含 human tissue comparison
- 代码输入：expression matrices、cell metadata、tissue labels、Treg/Tmem annotations、TCR reconstruction files
- 代码输出：cell-state labels、BGPLVM latent axes、gene kinetic timing、velocity plots、clonality summaries、mouse-human signature comparison
- 模型结构与意义：组合式 workflow；最核心的模型层是 BGPLVM trajectory + sigmoidal gene kinetics

## 可信度评估
- 期刊层面：Immunity，高影响力免疫学期刊
- 可复现性：raw accession、processed data 和 analysis repository 均明确
- 局限：主要为 mouse 数据；human 部分是对照/转化，不是大规模人群 cohort；pseudotime/velocity 不能替代真实 lineage tracing
- 综合判断：**直接算法创新中等，trajectory task definition 和组织免疫建模价值高**

## 一句话结论
这篇文章应作为 Treg tissue adaptation trajectory 的关键文献：它把组织适应写成可建模的连续过程，为 donor-aware、clone-aware、cross-species Treg 算法留下明确空间。
