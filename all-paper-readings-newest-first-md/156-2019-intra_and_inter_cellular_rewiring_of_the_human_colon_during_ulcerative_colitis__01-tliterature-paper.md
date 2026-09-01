# 041. Intra- and Inter-cellular Rewiring of the Human Colon during Ulcerative Colitis

## 基本信息
- 年份：2019
- 期刊：Cell
- DOI：https://doi.org/10.1016/j.cell.2019.06.029
- PMID：31348891
- 主题：ulcerative colitis；human colon mucosa；tissue immune atlas；cell-cell rewiring；GWAS risk mapping

## 为什么保留
这篇是自免/黏膜组织免疫方向的重要单细胞资源论文。它并不只回答“UC 中哪些 T cells 变了”，而是把 T cells 放进上皮、成纤维细胞、单核细胞与遗传风险模块共同重排的 colon ecosystem 中，因此很适合作为 tissue-aware method report 的关键例子。

## 数据与研究设计
- 样本对象：18 名 UC patients、12 名 healthy individuals
- 组织：人结肠黏膜 biopsies，覆盖 healthy、UC non-inflamed 与 UC inflamed tissue context
- 主数据：10x scRNA-seq colon mucosal atlas
- 原文单细胞规模：366,650 cells
- atlas 结构：51 个 epithelial、stromal 与 immune subsets；其中包含 10 个 T-cell subsets
- 额外解释层：anti-TNF resistance、bulk transcriptomic validation、histology/IFA、UC GWAS risk gene modules

## 主要发现
1. UC 不是单一免疫细胞扩张，而是 epithelial、stromal 与 immune compartments 同时被 rewired。
2. inflammatory fibroblasts、inflammatory monocytes、microfold-like cells 和 CD8/IL-17 co-expressing T cells 与 disease expansion/interactions 相关。
3. IL13RA2+IL11+ inflammatory fibroblasts 与 anti-TNF resistance 连接，提示临床 outcome 可以来自 multicellular tissue state。
4. UC risk genes 在细胞类型与共调控 module 上并非均匀分布，适合映射到具体 cell subset/pathway。

## 与 T 细胞和人群免疫力的关系
- 该文让 T-cell disease signal 从“单亚群 marker”扩展到“组织网络中的 T-cell node”。
- 对人群免疫研究，它强调个体疾病差异可能来自相同 T-cell state 在不同 stromal/myeloid context 中的作用差异。
- 由于没有 TCR/V(D)J，不能用它直接研究 clone specificity 或 clone-state trajectory。

## 算法与分析解读
- 先建 colon whole-tissue atlas，再在 major compartments 内细分 subsets。
- 将 disease-associated abundance change 与 within-subset DE 分开统计。
- 公开脚本中保留 ambient RNA contamination、cell composition change、DE test 和 clustering 复现步骤。
- 结合 cell-cell interaction rewiring 与 GWAS risk module mapping，把 atlas 推进到 disease mechanism hypothesis。

## 对算法开发的启发
1. 组织疾病中的 T-cell 模型应能接收 multicellular context，而不是把邻近 lineage 当噪声。
2. donor-aware compositional inference 与 state-DE inference 应拆开评估。
3. interaction graph 需要更强的空间/干预/纵向证据约束。
4. GWAS risk mapping 可以从 enrichment 扩展到 state-conditioned variant-to-module model。

## 数据可用性
- 文章数据入口：https://singlecell.broadinstitute.org/single_cell/study/SCP259/intra-and-inter-cellular-rewiring-
- Single Cell Portal accession：`SCP259`
- Portal 摘要规模：365,492 total cells、21,784 genes
- 文章正文规模：366,650 colon mucosal cells，18 UC + 12 healthy donors
- 作者代码：https://github.com/cssmillie/ulcerative_colitis
- 代码输入：`SCP259` 的 lineage-specific sparse matrices、metadata 和 discovery cohort Seurat objects
- 代码输出：subset clustering、ambient contamination diagnostics、disease composition change、differential expression 和 manuscript plots
- 当前访问判断：processed matrix 和 analysis scripts 可定位；本轮未定位到文章单独提供的 GEO/SRA/EGA raw-read accession

## 可信度与边界
- 可信度：高
- 强项：样本组织真实、细胞规模大、跨 cell compartments、公开 portal 与分析脚本
- 边界：主要是 scRNA atlas；缺少 TCR、protein multimodal readout 和 raw-read accession 级开放入口

## 一句话结论
这篇文章最有价值的地方，是把 UC 中的 T-cell 变化解释为 colon tissue network rewiring 的一部分，为 multicellular and donor-aware immune algorithms 提供了真实任务。
