# Algorithm Report 049

## Paper
Landscape of Infiltrating T Cells in Liver Cancer Revealed by Single-Cell Sequencing

## 算法视角定位
`049` 是 early tumor T-cell atlas and trajectory paper。它的重要性在于把 human cancer T-cell analysis 组织成三条并行算法线：
- transcriptomic state discovery
- pseudotime/trajectory analysis of infiltrating T-cell programs
- TCR reconstruction from Smart-seq2 transcriptomes for clone/state context

它不是新通用算法论文，但它把“肿瘤 T-cell heterogeneity 不只是 cluster abundance，而是 state continuum + clonality”这个问题提到了前台。

## 题录与数据
- 年份：2017
- 期刊：Cell
- DOI：https://doi.org/10.1016/j.cell.2017.05.035
- 物种/器官：`Homo sapiens`；hepatocellular carcinoma tumor, adjacent normal liver tissue and peripheral blood
- 主数据：5,063 single T cells from 6 HCC patients
- 测序：Smart-seq2 为主；GEO 说明 patient P1202t 使用 Tang2010 protocol
- GEO：`GSE98638`
- BioProject：`PRJNA385799`
- raw controlled access：EGA `EGAS00001002072`

## 数据任务定义
1. 在 blood、tumor 和 adjacent normal liver tissue 间比较 CD8, CD4 helper and Treg transcriptomes。
2. 抽取 tumor-associated T-cell clusters 与 exhaustion/regulatory programs。
3. 用 trajectory methods 描述 CD8 and CD4 T-cell state continua。
4. 从 scRNA reads 中重建 TCR sequence context，连接 clone expansion 与 infiltrating state。

## 关键算法问题
1. tumor、adjacent tissue、blood 是 tissue domains，如何同时比较 state 与 origin。
2. T-cell exhaustion、activation 与 differentiation continua 用 clusters 表达会发生什么损失。
3. Smart-seq2 transcriptome 中重建 TCR 时，clone calls 与 expression QC 如何联合。
4. 小 donor count 下，cell-level trajectory 如何避免被 patient-specific sampling 放大。

## 详细算法贡献
### 1. Tumor T-cell state atlas
- 文章对 sorted T-cell compartments 建 expression landscape，输出 CD8、helper 和 regulatory T-cell states。
- 相比只报 bulk TIL signatures，它提供 cell-level state taxonomy 和 tissue distribution。

### 2. 轨迹分析压力测试
- 正文/补充材料以 Monocle 2.0 为主做 T-cell trajectory，并对 EMBEDDR、SCORPIUS、TSCAN 等 trajectory methods 做对照展示。
- 对 method report 这点很重要：肿瘤 T-cell exhaustion trajectory 是典型 high-value task，但不同 trajectory assumptions 会影响路径解释。

### 3. TCR reconstruction as clone layer
- 文章使用 TraCeR 从 single-cell RNA-seq reads 组装 TCR，得到 CDR3、rearranged TCR genes 与 expression abundance。
- 这让每个 transcriptomic state 能附带 clone relation，而不必依赖单独 bulk TCR 数据。
- 但 clone layer 仍主要用于 post hoc context；它没有训练一个端到端 sequence-state model。

### 4. Differential programs and clinically relevant signatures
- 论文提取 exhausted CD8 and Treg-associated programs，为后续 tumor immunology signature transfer 提供早期来源。
- 对算法综述，重要的是 signature transfer 的边界：不同 tumor sites、人群背景和 protocol 会改变 exhausted-like program 的含义。

## 代码专项
- 本轮未定位到该文作者单独发布的 analysis repository。
- 可复现算法组件以已有工具为主：
  - 输入：Smart-seq2/Tang2010 T-cell expression matrices and raw reads
  - TraCeR 输出：TCR CDR3/rearranged chain/clonotype context
  - trajectory 输出：ordered CD8/CD4 state paths and gene-program trends
  - GEO 输出：count、TPM、centered expression matrices and cell metadata
- 因而它应被写成“task-defining biological atlas using existing algorithms”。

## 对新算法贡献程度
- 直接算法创新：**低到中**
- T-cell state/trajectory task definition：**很高**
- clone-state benchmark potential：**高**
- donor-scale statistical strength：**中等**
- 综合判断：**P1 tumor T-cell trajectory and clonality anchor**

## 数据可用性评估
- DOI：https://doi.org/10.1016/j.cell.2017.05.035
- GEO：https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE98638
- GEO accession：`GSE98638`
- GEO sample records：6 patient-level samples；cell metadata table has 5,063 rows
- GEO supplementary files：
  - `GSE98638_HCC.TCell.S5063.count.txt.gz`
  - `GSE98638_HCC.TCell.S5063.TPM.txt.gz`
  - centered expression and small bulk files
- EGA raw accession：`EGAS00001002072`
- Sample types：PTC/NTC/TTC for CD8 and corresponding helper/Treg tissue categories plus joint-area categories described by GEO
- 可复用性：processed matrix and metadata easy; raw scRNA reads controlled via EGA; author-specific code not located

## 新算法空间
1. **Clone-aware trajectory inference**
   - 把 TCR clonality 作为 transition prior/graph edge，而非后验 annotation。
2. **Tumor-normal-blood domain model**
   - 区分 tissue migration, adaptation and protocol effects。
3. **Trajectory uncertainty across methods**
   - 对 competing pseudotime topologies 给 uncertainty，而不是只选一条路线。
4. **Donor-aware tumor T-cell signatures**
   - 从 cell-level exhausted signatures 走向 patient-level predictive and calibrated scores。

## 最终判断
`049` 应在 method report 中作为 tumor T-cell state continuum 的早期核心条目。它的数据规模和 public processed matrices 很有用，算法上的最大机会是把 TCR clone relation 真正并入 trajectory/model fitting。
