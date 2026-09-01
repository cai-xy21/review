# 054. Clonal replacement of tumor-specific T cells following PD-1 blockade

## 基本信息
- 年份：2019
- 期刊：Nature Medicine
- DOI：https://doi.org/10.1038/s41591-019-0522-3
- PMID：31359002
- 主题：PD-1 blockade；pre/post TIL clones；clonal replacement；RNA/TCR coupling

## 为什么重要
这篇文章把 checkpoint blockade response 明确写成一个纵向 TCR-state 问题：治疗后扩增的 exhausted/tumour-specific CD8 T cells，究竟来自治疗前已经存在的 TIL reinvigoration，还是来自新进入肿瘤的 clonotypes？作者用 site-matched pre/post tumour、paired scRNA/TCR 和 bulk TCR 证明 clonal replacement 是重要现象。对算法综述而言，它是“therapy-time repertoire turnover + phenotype transition”建模的核心基准。

## 数据与研究设计
- 物种/组织：Homo sapiens；site-matched basal cell carcinoma (BCC) and squamous cell carcinoma (SCC) skin tumours before and after anti-PD-1
- 样本结构：同一患者/病灶治疗前后配对 tumour sampling，是本文算法价值的关键
- 单细胞规模：79,046 cells with paired RNA/TCR context
- 技术：single-cell RNA-seq + TCR-seq；bulk TCR-seq；exome sequencing；部分 ensemble/bulk expression reference
- 研究目标：区分 pre-existing tumour-infiltrating clones 的 reinvigoration、post-treatment novel clonotypes 的进入/扩张，以及 phenotype state coupling

## 核心贡献
- 用 pre/post exact clonotypes 证明 therapy-expanded exhausted CD8 compartment 含大量 novel clonotypes。
- 将 tumour recognition、clonal expansion 与 dysfunction state 放入同一 single-cell frame。
- 把 clonal persistence、clonal replacement、phenotypic exhaustion 和 checkpoint therapy response 组织成可量化任务。
- 为 longitudinal immunotherapy clone-state model 提供 benchmark。

## 文章中的算法/分析流程
### 1. Pre/post paired clone map
- 每个 T cell 同时有 expression state 和 TCR clonotype。
- 按 patient、tumour site、treatment timepoint、T-cell cluster/state、clonotype 建表。
- 治疗后扩增 clone 与治疗前同病灶 clone list 比较，从而定义 persistent clones 与 novel/replacement clones。

### 2. Tumour-specific/exhausted CD8 state coupling
- 作者把 CD8+CD39+ expanded T cells 作为 tumour recognition/dysfunction 相关 compartment。
- 通过 expression signatures、clonal expansion 和 TCR identity 耦合，说明 exhausted CD8 T-cell expansion 不是单一 state score，而是 clone-state 共同变化。

### 3. Bulk TCR 对 sampling depth 的补充
- 单细胞 TCR 提供 phenotype-resolved clone identity，但深度有限。
- bulk TCR-seq 提供更深的 repertoire 观察，用于验证治疗前后 clone 是否确实缺失/出现。
- 这提示后续算法应显式整合 scTCR 和 bulk TCR 的不同采样深度与误差模型。

### 4. Clonal replacement 的方法边界
- “治疗前未观察到”不等于绝对不存在；可能受采样深度、TCR capture rate 和组织空间异质性影响。
- 因此后续新算法应把 replacement 做成概率估计，而不是二元 presence/absence rule。

## 数据可用性
- 文章 DOI：https://doi.org/10.1038/s41591-019-0522-3
- GEO ensemble/scRNA-seq：`GSE123814`，链接 <https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE123814>
- GEO-linked BioProject：`PRJNA509907`
- exome SRA/BioProject：`PRJNA533341`
- bulk TCR ImmuneACCESS DOI：https://doi.org/10.21417/KY2019NM
- ImmuneACCESS page：<https://clients.adaptivebiotech.com/pub/yost-2019-natmed>
- 数据性质：human BCC/SCC tumours；site-matched pre/post anti-PD-1；79,046 paired scRNA/TCR cells；bulk TCR-seq；exome sequencing
- 数据访问边界：GEO 提供 ensemble/scRNA-seq 数据；exome 在 SRA BioProject；bulk TCR 通过 ImmuneACCESS；其他相关数据可向作者合理请求
- 论文网页：<https://www.nature.com/articles/s41591-019-0522-3>
- PMC 入口：<https://pmc.ncbi.nlm.nih.gov/articles/PMC6689255/>

## 代码可用性
- 公开 repository：未在文章 Code availability 中定位到可直接下载的作者代码仓库。
- 文章代码声明：custom code available from corresponding authors upon reasonable request。
- 可复现输入：
  - GEO ensemble/scRNA matrices and cell metadata
  - single-cell TCR clonotype annotation
  - pre/post sample metadata
  - ImmuneACCESS bulk TCR tables
  - exome mutation/neoantigen context from SRA/supplement
- 预期输出：
  - T-cell cluster/state annotation
  - CD8+CD39+ exhausted/tumour-reactive compartment summaries
  - pre/post clone sharing matrix
  - expanded-clone novelty/replacement statistics
  - bulk TCR-supported repertoire turnover comparisons
- 模型结构与意义：
  - 本文不是 packaged algorithm，而是一个 longitudinal clone-state analysis framework。
  - 可概括为 `paired pre/post tumour + scRNA/TCR + bulk TCR -> clone persistence/replacement + phenotype coupling`。

## 对新算法开发的启发
1. 估计 clonal replacement 时要处理 sampling absence。
2. 跨时间 repertoire turnover 应和 expression phenotype 一起建模。
3. 区分新进入、局部扩增和已有 clone state conversion。
4. scTCR 与 bulk TCR 可用多观测层级模型整合，校正不同测序深度。
5. 需要把 tumour mutation/neoantigen、blood reservoir 和 tissue spatial sampling 纳入机制模型。

## 对新算法贡献程度
- 直接算法创新：**中等偏低**。没有发布通用新算法包。
- 任务定义贡献：**很高**。把 immunotherapy response 形式化为 longitudinal clonal replacement problem。
- 数据贡献：**高**。pre/post site-matched paired scRNA/TCR + bulk TCR 对 clone-state model 很有价值。
- 新算法空间：**很高**。适合发展 dropout-aware clonal turnover、immigration/local-expansion decomposition 和 response prediction。

## 可作为我们 method 报告里的位置
建议放在“longitudinal immunotherapy TCR-state dynamics”章节。它能帮助我们说明已有研究已经观察到 replacement，但还缺少显式不确定性、机制分解和跨模态层级建模。

## 一句话结论
`054` 把 checkpoint therapy response 写成 clonal turnover 问题，是 longitudinal TCR-state algorithm 的重要起点。
