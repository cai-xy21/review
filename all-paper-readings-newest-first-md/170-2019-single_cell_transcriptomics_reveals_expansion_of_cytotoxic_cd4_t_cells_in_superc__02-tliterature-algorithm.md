# Algorithm Report 027

## Paper
Single-cell transcriptomics reveals expansion of cytotoxic CD4 T cells in supercentenarians

## 题录与数据
- DOI：https://doi.org/10.1073/pnas.1907883116
- 样本：`61,202` human PBMC from 7 supercentenarians and 5 younger controls
- TCR 子集：2 supercentenarians with single-cell TCR sequencing
- 公开数据入口：<http://gerg.gsc.riken.jp/SC2018/>

## 算法定位
这是 `rare human cohort state discovery` 条目，而不是新算法方法论文。它对方法报告的价值在于构造一个难题：极端人群、donor 数少、cell 数多、composition shift 强、clone expansion 只在部分样本可见。

## 论文实际分析链
1. PBMC scRNA-seq state annotation
2. supercentenarian/control composition comparison
3. cytotoxic CD4 transcriptional program discovery
4. selected-donor single-cell TCR clonality analysis
5. biological interpretation as exceptional healthy aging remodeling

## 数据输入与输出
- 输入：PBMC count matrices、per-cell annotations、subset TCR calls
- 输出：cell-type/state composition、cytotoxic CD4 program、clone dominance evidence
- 独立模型输出：无
- 独立作者代码仓库：本轮未定位

## 对算法开发的贡献程度
- 直接算法创新：**低**
- T-cell population question relevance：**高**
- rare phenotype benchmark value：**高**
- 数据开放程度：**中高**，有 `SC2018` 支持站点但未定位标准 GEO/SRA accession

## 最值得提炼的算法问题
### Donor-aware inference
61k cells 不能替代 12 donors。任何新模型若宣称识别 longevity phenotype，需要把 donor-level uncertainty 写清。

### Compositional robustness
CD4 CTL expansion 同时是 cell-state signal 与 composition shift。方法应区分 abundance change、state program change 与 clone-dominance effect。

### Clone-aware outlier modeling
TCR 子集提示 phenotype 与 clonal expansion 相关，但覆盖不完整。适合发展 missing-modality-aware clone/state model。

## 新算法空间
1. normative immune-aging manifold with exceptional outlier score
2. donor-level compositional model for rare cohorts
3. clone-expanded state discovery under partial VDJ coverage
4. uncertainty-calibrated resilience phenotype detector

## 最终判断
027 不应被包装成算法突破；它应作为“为什么 population T-cell algorithms 需要 donor hierarchy 和 rare-cohort calibration”的证据论文。
