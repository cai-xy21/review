# Algorithm Report 051

## Paper
Lineage tracking reveals dynamic relationships of T cells in colorectal cancer

## 算法视角定位
`051` 是本组最明确的直接算法论文之一。它把单细胞 T-cell transcriptome 与从同一 Smart-seq2 readout 恢复的 TCR clonotype 结合，提出 `STARTRAC` indices，把 clone sharing 从“画克隆频率图”推进为可量化的 expansion、migration 与 state-transition summaries。

## 题录与数据
- 年份：2018
- 期刊：Nature
- DOI：https://doi.org/10.1038/s41586-018-0694-x
- 物种/器官：`Homo sapiens`；colorectal cancer tumor、adjacent normal colorectal tissue、peripheral blood
- 数据规模：12 treatment-naive CRC patients；11,138 single T cells；20 T-cell subsets
- 主模态：Smart-seq2 scRNA-seq with TCR tracking
- GEO：`GSE108989`
- raw EGA：`EGAS00001002791`
- BioProject：`PRJNA429424`
- 方法代码：https://github.com/Japrin/STARTRAC

## 数据任务定义
1. 按 tissue compartment 和 T-cell state 组织 paired expression/clonotype data。
2. 量化克隆在同 state 内扩张、跨 tissue 分布和跨 state 共享。
3. 在 CRC 中比较 effector、exhausted、Treg 和 TH-like programs 的动态关系。
4. 让 TCR lineage tag 成为 state-relationship inference 的约束。

## 详细算法贡献
### 1. `STARTRAC` 指标化 clone-state relation
- 输入不是仅有 expression matrix，而是带 cell barcode、patient、tissue、cluster/state 与 clonotype 的 T-cell table。
- STARTRAC 把 TCR tracking 归纳为 expansion、migration、transition 等 indices，给不同 subsets 可比的动态特征。
- 这一步是 direct algorithm increment：把 lineage evidence 从 qualitative clone sharing 变成 reproducible summaries。

### 2. Expression state 与 TCR lineage 双证据
- Expression clustering 给出 20 subsets；TCR tracking 再判断 transcriptionally adjacent states 是否真的 share clonal history。
- 文中发现 CD8 effector 与 exhausted states 虽都高度扩张，却分别与 tumor-resident CD8 effector memory linked，说明表达连续性不等于单一路径。
- 对 Treg/TH clones 的共享分析同样把 CD4 state relationship 从 marker interpretation 推进到 lineage evidence。

### 3. CRC compartment benchmark
- 血液、邻近组织与肿瘤三域设计使 `migration` index 有真实 anatomical context。
- 4 名 MSI patients 加入疾病人群差异背景，支持将 IFNG+ TH1-like programs 与 checkpoint-responsive context 连接。

## 代码专项
- `STARTRAC` 是 R package；README 示例读入 `example.cloneDat.Zhang2018.txt`，调用 `Startrac.run(in.dat, proj="CRC")`。
- 代码输入：clone-level/cell-level table，至少需 clonotype、patient、tissue and cluster/state metadata；原论文表达分析可从 GEO count/TPM 矩阵衔接。
- 代码输出：subset-wise STARTRAC index summaries，用于 clonal expansion、tissue migration 和 state transition comparison。
- 意义：它不负责 scRNA normalization/clustering 全流程，而是 receptor-state coupling 的 quantification layer。
- 另有 Figshare preliminary processing code DOI `https://doi.org/10.6084/m9.figshare.8204624.v1`。

## 对新算法贡献程度
- 直接算法创新：**高**
- clone-state task definition：**很高**
- 数据 benchmark 价值：**高**
- 综合判断：**P1 clone-aware single-cell T-cell dynamics anchor**

## 数据可用性评估
- GEO processed files include `GSE108989_CRC.TCell.S11138.count.txt.gz`, TPM matrix, centered matrix, bulk/exome tables。
- GEO cell metadata table has 11,138 rows and 12 patient records。
- Raw reads: EGA `EGAS00001002791`；GEO states raw data are not provided in GEO.
- 数据公开链接：GEO、EGA、STARTRAC GitHub、Figshare processing code。

## 新算法空间
1. 把 STARTRAC summary indices 扩展为 hierarchical probabilistic clone-state transition model。
2. 对 tissue migration 与 sampling dropout 给 uncertainty。
3. 纳入 TCR sequence similarity/specificity，而不只 exact clonotype identity。
4. 从 cross-sectional tissue clone sharing 推进 longitudinal therapy tracking。

## 最终判断
`051` 应作为 method report 中“已有算法已经如何量化 TCR-state coupling”的核心引用。它给出明确算法产物，后续空间在生成式、donor-aware、specificity-aware 的 lineage model。
