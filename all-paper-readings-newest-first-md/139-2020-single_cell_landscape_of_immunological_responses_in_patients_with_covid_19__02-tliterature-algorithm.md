# Algorithm Report 033

## Paper
Single-cell landscape of immunological responses in patients with COVID-19

## 题录与数据
- 年份/期刊：2020, Nature Immunology
- DOI：https://doi.org/10.1038/s41590-020-0762-x
- 数据：human PBMC scRNA-seq plus TCR/BCR V(D)J profiling
- cohort：COVID-19 patients `n=13`，healthy donors `n=5`
- raw accession：GSA-Human `HRA000150`

## 算法视角定位
这是 **infection-state scRNA + adaptive repertoire descriptive coupling**。它比纯 PBMC atlas 多了 TCR/BCR，但仍以 mature pipeline and statistics 为主，不是 TCR specificity 或 multimodal latent algorithm。

## 输入、处理与输出
### 输入
- 10x 5' gene-expression data
- paired TCR/BCR V(D)J calls
- donor and disease labels

### 处理链
1. 10x preprocessing
2. Seurat-style QC, normalization, clustering and marker annotation
3. V(D)J calls linked to transcriptomic cell barcodes
4. TCR/BCR expansion and V(D)J usage statistics by immune subset and disease context
5. differential abundance/expression interpretation across patients and controls

### 输出
- PBMC immune-state map
- T-cell clone expansion and receptor usage summaries
- B-cell/plasmablast and BCR repertoire summaries
- cell-state metadata usable for downstream clone-aware modeling

## 详细算法贡献
### 1. 把 adaptive receptor readout回填到感染 atlas
在 COVID PBMC 的早期资源中，本文明确给出 receptor-linked cell-state analysis。它定义了常见 baseline：
`expression state + receptor clonotype + disease label -> clone-state descriptive statistics`

### 2. 暴露出 clone/state modeling 的缺口
本文 receptor 分析主要在注释后的 cell groups 上做 expansion/usage statistics：
- clonotype 是 feature/metadata，不是 graph constraint
- disease label 常在 group comparison 阶段进入，而不是层级模型
- donor heterogeneity 没有被统一建模成 outcome uncertainty

### 3. 适合作为 benchmark ingredient
由于 public raw accession 存在，后续可把它变成：
- TCR-state coupling benchmark
- COVID PBMC severity/domain shift test
- receptor-aware annotation stress test

## 代码与模型边界
- 作者没有发布专用代码仓库。
- 文章说明实验协议和分析 pipeline follow 10x Genomics 与 Seurat；custom scripts on request。
- 因而本文可复用的是数据与任务结构，不是可直接调用的新 package。

## 对新算法贡献程度
- 任务定义：高
- 数据资源：中高
- 直接新算法：低
- 对 receptor-aware COVID methods 的启发：高

## 可开发空间
1. **Clone-aware hierarchical model**：同时建模 donor、disease、clone size 与 cell state。
2. **Sequence-aware repertoire embedding**：让 CDR3/V-gene features 与 RNA state jointly learned。
3. **Outcome-aware immune response score**：把 expression activation、clone expansion 和 B/T-cell coordination 汇成 donor-level score。

## 数据可用性评估
- 物种/器官：`Homo sapiens`；peripheral blood / PBMC
- Raw accession：`HRA000150`
- Code：无公开 author repository
- 复用风险：raw human data access、custom script unavailable、early COVID cohort heterogeneity

## 可纳入 method report 的一句话
COVID PBMC repertoire studies already linked TCR/BCR expansion to transcriptomic cell states, but most remained descriptive pipelines rather than donor-aware receptor-state models.
