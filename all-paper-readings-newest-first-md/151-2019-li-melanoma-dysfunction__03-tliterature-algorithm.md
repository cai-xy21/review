# Algorithm Report 052

## Paper
Dysfunctional CD8 T cells form a proliferative, dynamically regulated compartment within human melanoma

## 算法视角定位
`052` 是 melanoma immune atlas 中非常重要的 T-cell dysfunction dynamics 文献。它没有提出一个专门 TCR joint model，但它用 `MetaCell`、gene modules、continuous CD8 state organization 与 TCR sharing，把“exhausted CD8 cells 是静态 terminal sink”改写成“高度增殖、可动态分化的 compartment”。

## 题录与数据
- 年份：2019
- 期刊：Cell
- DOI：https://doi.org/10.1016/j.cell.2018.11.043
- 物种/器官：`Homo sapiens`；melanoma tumors with several matched PBMC samples
- 数据规模：25 melanoma patients；46,612 intratumoral immune cells after QC；29,825 T/NK cells in T-cell analyses
- processed GEO：`GSE123139`
- raw EGA：`EGAS00001003363`
- software：MetaCell R package and source at Tanay lab Bitbucket

## 数据任务定义
1. 在患者间抽取 conserved immune/T-cell programs，而不被 lesion composition 主导。
2. 解析 CD8 transitional-to-dysfunctional continuum。
3. 把 gene modules、proliferation、clonality 和 tumour-reactivity signatures 对齐。
4. 区分 complete cytotoxic cells 与 transitional/dysfunctional clone relation。

## 详细算法贡献
### 1. `MetaCell` coarse-graining
- 文章使用 metacells 来聚合相似 single cells，稳定估计 co-regulated programs 和 patient-robust states。
- 对高异质 tumor immune data，metacell layer 减少单 cell dropout 对 module/state interpretation 的影响。

### 2. Gene-module view of dysfunction
- CD8 metacell analysis de novo extracts modules for TCF7-like naive/memory regulation, effector/cytotoxic genes, dysfunction checkpoints and proliferation。
- 这比单 marker exhaustion score 更适合描述 continuum and mixed programs。

### 3. Continuous CD8 state relationship
- 论文组织了 early effector transitional to dysfunctional progression，并用 TCR sharing 支持 cytotoxic and dysfunctional compartments 的关系差异。
- 算法含义：tumor T-cell states 应允许 continuous regulated manifolds，而不是只输出 disjoint cluster labels。

### 4. Patient composition link
- 文章还比较 patient-specific transcriptional states 与 infiltrating immune-cell compositions，暴露出 cell-state dynamics 与 donor/tumor ecosystem 的耦合问题。

## 代码专项
- MetaCell 输入：single-cell gene-expression matrix plus filtering/QC and annotation metadata。
- MetaCell 输出：metacell assignments、co-regulated gene modules、state/module scores；本研究再接 TCR sharing and patient-level comparisons。
- 原文 Data and software availability 指向 MetaCell open source；复现分析脚本网页也由作者提供。
- 意义：直接算法核心是 stable state/module coarse-graining，不是 antigen specificity inference。

## 对新算法贡献程度
- 直接算法创新：**中高**，依赖 MetaCell analysis framework
- dysfunction task definition：**很高**
- TCR-state benchmark value：**高**
- 综合判断：**P1 dynamic dysfunction benchmark**

## 数据可用性评估
- GEO：https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE123139
- GEO records scRNA-seq immune cells from 25 melanoma patients and several PBMC samples。
- Raw reads：EGA `EGAS00001003363`
- Processed data：GEO `GSE123139`
- 作者分析公开性：MetaCell package/source可定位；paper-specific scripts 指向作者 compgenomics analysis page

## 新算法空间
1. Metacell/state module 与 exact clonotype/sequence specificity 的 joint model。
2. Proliferation 与 dysfunction disentanglement，避免 cycling dominates state geometry。
3. Cross-patient calibrated dysfunction progression score。
4. Tumor-reactivity supervision for dysfunctional vs bystander cell states。

## 最终判断
`052` 适合作为 dysfunction continuum 和 module-based T-cell modeling 的核心场景。它说明 exhaustion 轴不能只靠 terminal marker，算法要同时容纳 proliferation、clonality 和 patient context。
