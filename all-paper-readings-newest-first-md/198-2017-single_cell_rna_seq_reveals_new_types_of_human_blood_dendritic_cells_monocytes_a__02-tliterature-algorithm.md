# Algorithm Report 043

## Paper
Single-cell RNA-seq reveals new types of human blood dendritic cells, monocytes, and progenitors

## 算法视角定位
`043` 是 early human immune discovery atlas。它不是 T-cell 专题论文，也没有独立发布一个新算法包；但它在算法史上的价值很高：把“基于少数 FACS markers 的血液免疫 taxonomy”推进到“单细胞全转录组无监督发现 + prospective validation”。

对 T-cell 人群免疫综述，它适合放在 reference-building 章节：T-cell interpretation 依赖 antigen-presenting cells、monocytes 和 progenitor references，且稀有 immune state 的发现范式在这里已被证明可行。

## 题录与数据
- 年份：2017
- 期刊：Science
- DOI：https://doi.org/10.1126/science.aah4573
- PMID：28428369
- PMCID：PMC5775029
- 物种/样本：`Homo sapiens`；fresh peripheral blood / PBMC-enriched DC, monocyte and progenitor fractions from healthy donors
- 测序协议：Smart-seq2 full-length scRNA-seq
- GEO：`GSE94820`
- raw controlled-access：dbGaP `phs001294.v1.p1`
- Single Cell Portal：`SCP43`

## 数据任务定义
1. 从 marker-enriched human blood cells 中恢复 DC/monocyte/progenitor transcriptome landscape。
2. 在 discovery phase 找到传统 immune gates 未清楚分出的 cell states。
3. 用 deep characterization、functional and phenotypic follow-up 把 cluster 变成可验证的 taxonomy。
4. 为后续 blood immune monitoring 建 reference signatures。

## 关键算法问题
1. 罕见 DC/progenitor populations 在高噪声、marker-enriched sampling 中如何稳健发现。
2. 如何让 unsupervised clusters 与可 prospectively isolate 的 surface-marker phenotype 对齐。
3. reference taxonomy 如何允许新类出现，而不被旧 gating scheme 强行压回已有标签。
4. atlas 中不同分期/深度 characterization 数据如何作为 discovery 与 validation，而非简单堆叠。

## 详细算法贡献
### 1. Unbiased genomic classification 作为 taxonomy engine
- 论文以约 2,400 个 Smart-seq2 profiles 重建 blood DC/monocyte heterogeneity。
- 其核心并非一个新的损失函数，而是用 transcriptome-wide clustering 与 marker/signature extraction 取代固定 marker taxonomy。
- Single Cell Portal 也明确把该策略表述为 unbiased genomic classification。

### 2. 两阶段数据设计
- GEO 将数据拆成：
  - exploratory phase：1,140 single human blood DC/monocyte cells + 12 population samples
  - deep characterization phase：额外 1,261 single cells + 9 population samples
- 这对算法评估很有用：稀有 subtype discovery 不应只在单一 exploratory subset 上自洽，还要有 follow-up characterization。

### 3. cluster-to-signature-to-validation 闭环
- 文章从 clusters 提取 discriminatory gene sets，并用后续表型与功能实验验证新 taxonomy。
- 输出不是仅供作图的 t-SNE cluster，而是可用于监测的 signature 与 surface-marker hypotheses，例如 AS DC 和 cDC progenitor。
- 这类闭环是后续 reference annotation 算法的必要先例。

### 4. 对 T-cell 研究的间接算法贡献
- AS DC 与 CD1C+ DC subtype 的发现直接影响 antigen presentation 与 T-cell activation 的 reference context。
- 因此 donor-level T-cell immune fitness model 若只建 T-cell manifold，容易丢掉上游 APC variation。

## 代码专项
- 本轮在论文公开入口、GEO 与 Single Cell Portal 中未定位到作者单独发布的通用代码仓库。
- Single Cell Portal 注明其门户 cluster view 与 manuscript figure 可能因 newer Seurat version 视觉投影不同，但 clusters 是同一套。
- 对复用者而言，主要输入输出是数据资源：
  - 输入：Smart-seq2 expression matrices、sample/cell metadata、marker-enriched blood cell subsets
  - 输出：DC/monocyte/progenitor clusters、gene signatures、revised taxonomy 与 prospective marker hypotheses
- 因而它应写作“reference discovery workflow”，不应写成“可直接调用的模型包”。

## 对新算法贡献程度
- 直接新算法：**低**
- immune reference/taxonomy 任务定义：**很高**
- 数据复用价值：**高**
- 对 rare-state discovery 启发：**高**
- 综合判断：**P0 级 blood immune reference/discovery anchor**

## 数据可用性评估
- DOI：https://doi.org/10.1126/science.aah4573
- GEO：https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE94820
- GEO 数据规模：2,422 single-cell and population samples processed；processed records保留 2,384 public samples after excluded bulk/QC records说明
- dbGaP raw accession：`phs001294.v1.p1`
- BioProject：`PRJNA374527`
- GEO supplementary matrices：
  - `GSE94820_raw.expMatrix_DCnMono.discovery.set.submission.txt.gz`
  - `GSE94820_raw.expMatrix_deeper.characterization.set.submission.txt.gz`
- Single Cell Portal：https://singlecell.broadinstitute.org/single_cell/study/SCP43/atlas-of-human-blood-dendritic-cells-and-monocytes
- SCP 页面当前展示：1,078 total cells、26,593 genes 的 atlas view
- 可复用性：processed expression 与 metadata 可直接拿；raw reads 需走 dbGaP controlled access

## 新算法空间
1. **Open-world immune annotation**
   - reference mapping 时允许未知 DC/monocyte/T-cell states 出现，并输出 novelty score。
2. **Rare immune state validation**
   - 把 discovery、deep characterization、orthogonal phenotype evidence 编入 evaluation protocol。
3. **APC-T-cell coupled donor model**
   - 联合 antigen-presenting cell composition/state 与 T-cell activation baseline。
4. **Marker-efficient prospective isolation**
   - 从 transcriptome signatures 反推最小 surface marker panel，并量化 ambiguity。

## 最终判断
`043` 的算法意义是 reference discovery，而不是 TCR 或多模态整合。它应该在 method report 中用来说明：单细胞算法最早改变的，不只是降维图，而是免疫细胞分类与后续 monitoring target。
