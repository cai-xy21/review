# 051. Lineage tracking reveals dynamic relationships of T cells in colorectal cancer

## 基本信息
- 年份：2018
- 期刊：Nature
- DOI：https://doi.org/10.1038/s41586-018-0694-x
- PMID：30479382
- 主题：CRC T cells；TCR-state coupling；STARTRAC；clone expansion/migration/transition；tumour-resident T cells

## 为什么重要
这篇是肿瘤 T 细胞单细胞算法方向的关键论文之一。它把 TCR clonotype 当作天然 lineage tag，用同一批单细胞的转录状态和 TCR 克隆共享关系推断 T 细胞在肿瘤、邻近正常组织和外周血之间的扩张、迁移与状态转换。对“T 细胞-人群免疫力”方法综述而言，它提供了一个非常明确的算法基线：表达相似性只能说明状态接近，TCR lineage sharing 才能提供更接近谱系/克隆历史的证据。

## 数据与研究设计
- 样本对象：12 名 treatment-naive colorectal cancer patients；包含 MSS/MSI 背景
- 样本类型：人类 colorectal cancer tumour、adjacent normal colorectal tissue、peripheral blood
- 物种/器官：Homo sapiens；结直肠肿瘤、邻近肠组织、外周血
- 单细胞规模：11,138 个 single T cells
- 技术路线：Smart-seq2 scRNA-seq；从同一单细胞数据中恢复 TCR alpha/beta 信息并进行 clonotype tracking
- 输出结构：20 个 T-cell subsets；按 patient、tissue、cluster/state、clonotype 组织的 T-cell table
- 研究目标：用 TCR clone sharing 量化 CRC 中 T 细胞的局部扩张、跨组织迁移和跨状态转换关系

## 核心亮点
1. **TCR 作为 lineage tag**：把 TCR clonotype 从附加注释变成推断 T 细胞动态关系的核心变量。
2. **STARTRAC 指标体系**：提出可复用的 clonal expansion、migration、transition 指标，后续很多 TCR-state 论文仍以类似思想组织分析。
3. **三组织 compartment 设计**：tumour、adjacent normal、blood 同时采样，使 migration index 有真实解剖语境，而不是纯嵌入空间迁移。
4. **纠正 expression-only interpretation**：表达相近的 T-cell states 不一定共享克隆历史；同一克隆跨状态分布才更接近 lineage evidence。

## 文章中的算法/分析流程
### 1. 单细胞 T 细胞状态图谱
- 对 11,138 个 T cells 做表达矩阵质控、归一化、降维和聚类，得到 CD8、CD4、Treg、TH-like 等 20 个状态。
- 作者比较 SC3、Seurat、sscClust 等聚类结果，并用 marker gene 与组织分布解释 cluster identity。
- 这一步给 STARTRAC 提供 `state/cluster` 维度：算法并不替代 scRNA clustering，而是把已有 state annotation 接入 TCR lineage 层。

### 2. TCR clonotype 定义与 clone sharing
- 从 Smart-seq2 单细胞转录本中提取 TCR alpha/beta chain 信息。
- clonotype 主要由 paired TCR sequence 定义；同一 clonotype 的多个细胞被视为同一 T-cell lineage 的观测点。
- clone size 用于衡量扩增；clone 在 tissue/state 间的分布用于后续 migration 与 transition 指标。

### 3. STARTRAC 指标
- 全称：Single T-cell Analysis by RNA-seq and TCR TRACking。
- 输入：每个 T cell 的 patient、tissue、cluster/state、clonotype；可衔接 GEO 的 expression matrix 与 metadata。
- 输出：subset-wise STARTRAC index summaries，主要包括：
  - expansion：一个状态内克隆扩张程度
  - migration：同一 clonotype 跨 tissue/compartment 出现的程度
  - transition：同一 clonotype 跨 T-cell state/cluster 共享的程度
- 算法意义：把 clone sharing 从图形化观察转成可比较、可复现的定量指标。

### 4. 结果解释中的算法价值
- CD8 effector 与 exhausted cells 都表现出 clonal expansion，但 TCR sharing 显示它们与 tumour-resident CD8 effector-memory states 的连接方式不同。
- tumour Treg、TH17 和 TH1-like states 的关系也通过 CD4 clonotype sharing 得到量化。
- MSI patients 的 context 支持把 IFNG+ TH1-like programs、checkpoint context 和 clone dynamics 放到同一解释框架。

## 对算法工作的启发
1. **clone-aware transition model**：STARTRAC 仍是 summary index，可扩展为带 donor/tissue 层级的概率转移模型。
2. **sampling uncertainty**：组织采样深度、TCR 捕获率和稀有 clone dropout 会影响 migration/transition，需要置信区间或 Bayesian correction。
3. **TCR sequence similarity**：STARTRAC 主要依赖 exact clonotype identity；可进一步引入 TCR sequence embedding、GLIPH2 类 specificity group 或 pMHC specificity label。
4. **therapy longitudinal extension**：这篇是 cross-sectional tissue design；后续可加入治疗前后时间轴，建模 clone replacement、persistence 和 reinvigoration。
5. **donor-aware model**：不同 patient 的 clone pool 不可直接混合，适合层级模型或 mixed-effect representation learning。

## 数据可用性
- 文章 DOI：https://doi.org/10.1038/s41586-018-0694-x
- GEO processed data：`GSE108989`，链接 <https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE108989>
- BioProject：`PRJNA429424`
- raw EGA study：`EGAS00001002791`
- raw EGA dataset：`EGAD00001003910`（相关 Data Descriptor 中注明）
- 数据性质：12 名 treatment-naive CRC patients；11,138 single T cells；tumour、adjacent normal colorectal tissue、peripheral blood；Smart-seq2 expression + TCR recovery
- GEO 文件类型：count matrix、TPM matrix、centered matrix、cell annotation/metadata、bulk/exome supporting tables 等 processed files
- 数据访问限制：GEO 提供 processed expression；raw human sequencing reads 需通过 EGA controlled access 申请
- 论文网页：<https://www.nature.com/articles/s41586-018-0694-x>
- 相关 Data Descriptor：<https://pmc.ncbi.nlm.nih.gov/articles/PMC6656756/>

## 代码可用性
- STARTRAC GitHub：<https://github.com/Japrin/STARTRAC>
- 论文 processing code Figshare DOI：https://doi.org/10.6084/m9.figshare.8204624.v1
- 代码输入：
  - clone/cell table，例如 GitHub 示例 `example.cloneDat.Zhang2018.txt`
  - 必需字段本质上包括 clonotype、patient、tissue/compartment、cluster/state
  - expression-derived cluster annotation 可由 GEO processed matrix 和 metadata 衔接
- 代码输出：
  - 每个 T-cell subset 的 expansion、migration、transition 等 STARTRAC indices
  - 可用于比较不同 cluster/state 的 clonal expansion、跨组织迁移和状态转换倾向
- 模型结构与意义：
  - STARTRAC 不是 deep learning model，也不负责 raw scRNA-seq preprocessing
  - 它是 receptor-state coupling 的统计指标层：`cell-state table + clonotype table -> clone dynamics indices`
  - 在算法谱系里应归为 clone-aware summary statistics / lineage-informed single-cell analysis

## 对新算法贡献程度
- 直接算法贡献：**高**。有命名方法 STARTRAC、可复用代码和明确输入输出。
- 数据贡献：**高**。同一研究设计中包含 tumour/normal/blood 三 compartment 和 paired expression/TCR 信息。
- 对新算法启发：**很高**。适合作为 clone-aware T-cell dynamics 的 baseline。
- 复现友好度：**中高**。processed data 和 STARTRAC 可获得；raw reads 为 controlled access。

## 可作为我们 method 报告里的位置
建议放在“TCR-state coupling / clone-aware T-cell dynamics”章节开头。它代表已有算法已经能量化 clonal expansion、migration 和 transition，但还没有把这些关系做成生成式、层级化、specificity-aware 的统一模型。

## 一句话结论
`051` 是 TCR-state coupling 从描述性统计走向可复用算法指标的关键条目；它的 STARTRAC 可作为我们讨论新算法空间时必须对标的既有方法。
