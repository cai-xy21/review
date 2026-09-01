# 057. Stem-like CD8 T cells mediate response of adoptive cell immunotherapy against human cancer

## 基本信息
- 年份：2020
- 期刊：Science
- DOI：https://doi.org/10.1126/science.abb9847
- PMID：33303615
- PMCID：PMC8883579
- 主题：ACT infusion product；stem-like CD8 T cells；therapy response

## 研究定位
这篇更适合作为 adoptive cell therapy (ACT) 的 response-state 文献，而不是公开 benchmark 数据论文。它把 infusion product 中的 CD8 T-cell differentiation state 与临床 response、TIL persistence 和 tumour reactivity 连接起来，核心问题是：输入给患者的 TIL 产品中，哪些 T-cell states 真正支持持久体内扩增和肿瘤清除。

## 为什么重要
多数肿瘤单细胞文献研究的是治疗前 tumour biopsy；这篇研究的 population unit 是 ACT infusion product。对算法综述而言，它提示一个不同但很实用的建模任务：不是只预测患者肿瘤里有没有 tumour-reactive T cells，而是预测一个制造出来的 TIL 产品是否含有足够的 stem-like、可持久扩增的 CD8 T cells。它也说明 marker panel compression 很重要：`CD39-CD69-` 和 `CD39+CD69+` 可以作为复杂转录状态的可操作读出。

## 数据与研究设计
- 物种/样本：Homo sapiens；human tumour-infiltrating lymphocyte products for adoptive cell therapy，主要围绕 metastatic melanoma ACT context
- 样本类型：ex vivo-expanded autologous TIL infusion products；部分实验涉及 tumour-reactive/neoantigen-reactive TILs、post-transfer persistence 和体内功能验证
- 关键表型：memory-progenitor/stem-like `CD39-CD69-` CD8 T cells 与 differentiated/terminal-like `CD39+CD69+` states
- 技术视角：高维 flow/cytometry phenotype、single-cell transcriptomic signatures、functional assays、TCR/neoantigen reactivity evidence
- 研究目标：判断 ACT response 是否由 tumour-reactive TIL 的数量本身决定，还是由 tumour-reactive TIL 所处的 stem-like/differentiation state 决定

## 算法与数据视角
- 文章用高维 cell-state analysis 和 single-cell transcriptomic signatures 区分 responder-enriched stem-like and non-responder-enriched differentiated TIL products。
- `CD39-CD69-` versus `CD39+CD69+` 是可解释 phenotype compression：前者对应 memory-progenitor/stem-like potential，后者对应更 differentiated/terminal state。
- 文章将 product state composition、tumour reactivity、post-transfer persistence 和 clinical response 放入同一解释链。
- 当前公开入口未定位到标准 GEO/SRA/EGA/dbGaP accession 或独立代码仓库，因此 benchmark 可复用性弱于 051-056 和 058。

## 文章中的算法/分析流程
### 1. ACT product state stratification
- 输入是每个 ACT infusion product 的 T-cell phenotype/state composition，以及患者 response/persistence metadata。
- 通过 high-dimensional phenotype 和 transcriptomic signatures，把 CD8 TILs 分成 stem-like/memory-progenitor 与 differentiated states。
- 输出是产品层的 state proportion 和 response-associated comparisons。

### 2. Marker compression
- `CD39-CD69-` 被用作有利 stem-like product fraction 的 marker readout。
- `CD39+CD69+` 标记更 differentiated、persistence 较差的 product state。
- 算法意义：将高维 transcriptomic state distill 成可在制造质控中使用的 marker panel。

### 3. Tumour reactivity 与 stemness 的分离
- 文章的重要结论是：多数 antitumour neoantigen-reactive TILs 可能在 differentiated CD39+ state，但 responders 保留 CD39- stem-like neoantigen-specific TIL pool。
- 这提示算法不能只预测 tumour specificity，还要预测 specificity 所在的 differentiation/persistence state。

### 4. ACT product quality score 的雏形
- 本文没有提出正式可下载模型，但自然导向一个 score：输入 infusion product 的 cell-state composition、marker panel、TCR/neoantigen reactivity 和 manufacturing covariates，输出 post-transfer persistence/response probability。

## 数据可用性
- 文章 DOI：https://doi.org/10.1126/science.abb9847
- PMID：33303615
- PMCID：PMC8883579
- 文章入口：<https://www.science.org/doi/10.1126/science.abb9847>
- PMC 入口：<https://pmc.ncbi.nlm.nih.gov/articles/PMC8883579/>
- 数据性质：human ACT/TIL infusion products；response-associated CD8 stemness/differentiation readouts；phenotype、transcriptomic signatures、functional/persistence evidence
- 数据可用性声明：已联网核查到文章层面的 `All data are available in the main text or the supplementary materials`
- 标准 accession：本轮未定位到 GEO/SRA/EGA/dbGaP accession
- 代码：本轮未定位作者独立 repository 或 packaged analysis code
- 评价：适合作为生物学和任务定义锚点，不适合作为 accession-level open benchmark 的主力数据集

## 代码/模型可用性
- 公开代码仓库：未定位。
- 可抽象的模型输入：
  - ACT product cell phenotype table
  - CD39/CD69/TCF7-like marker readouts
  - single-cell stemness/differentiation signatures
  - tumour/neoantigen reactivity labels
  - clinical response and post-transfer persistence metadata
- 可抽象的模型输出：
  - product quality/stemness score
  - responder probability
  - persistence probability
  - candidate product-release marker panel
- 模型结构与意义：
  - 本文不是新算法包，而是把 ACT product 质量控制定义成 single-cell state prediction problem。
  - 它对后续算法的意义在于把 `specificity` 与 `stem-like persistence state` 分离建模。

## 对新算法开发的启发
1. ACT product quality score
2. pre-infusion to persistence modeling
3. marker panel and transcriptomic stemness calibration
4. joint specificity-stemness model：tumour-reactive TCR 是否位于可持久扩增的 stem-like compartment
5. manufacturing-aware model：把培养条件、扩增轮次、细胞组成和最终临床 response 连接起来

## 对新算法贡献程度
- 直接算法创新：**低**。无公开新算法包。
- 任务定义贡献：**高**。非常清楚地定义 ACT product state -> persistence/response 问题。
- 数据开放贡献：**低到中**。主文/补充材料可用，但未定位标准 accession。
- 新算法空间：**高**。适合开发 ACT product QC score、state-to-marker distillation 和 specificity-stemness joint model。

## 可作为我们 method 报告里的位置
建议放在“therapy product / ACT quality modeling”小节，而不要和 GEO/EGA 完整开放的 scRNA/TCR benchmark 混在一起。它的价值是定义产品质量算法任务，弱点是 accession 和代码可复用性不足。

## 一句话结论
`057` 说明 ACT 成败依赖 T-cell product state，但它在 accession 和 public code 层面不如本段其他论文适合直接做算法 benchmark。
