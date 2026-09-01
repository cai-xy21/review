# 053. Transcriptional programs of neoantigen-specific TIL in anti-PD-1-treated lung cancers

## 基本信息
- 年份：2021
- 期刊：Nature
- DOI：https://doi.org/10.1038/s41586-021-03752-4
- PMID：34290408
- 主题：NSCLC；neoantigen-specific TIL；scRNA/TCR barcode tracking；anti-PD-1 response

## 为什么重要
这篇文章最适合放在“antigen specificity-aware single-cell T-cell algorithms”章节。它不是只用 exhaustion marker 猜测 tumour-reactive T cells，而是先通过 MANAFEST/ViraFEST 功能实验识别 MANA-specific 与 virus-specific TCR，再把这些 TCR CDR3 当作 barcode 回填到大规模 scRNA/TCR atlas。这样，T-cell state inference 有了外部实验验证的 specificity label，能直接比较 tumour-specific、virus-specific、responding 和 non-responding contexts。

## 数据与研究设计
- 临床队列：20 名 stage I-IIIA resectable NSCLC patients，接受 neoadjuvant anti-PD-1/nivolumab；与临床 pathological response 关联
- 临床试验背景：neoadjuvant nivolumab NSCLC trial，文中关联 MPR/non-MPR
- 样本类型：TIL、adjacent normal lung、tumour-draining lymph node、distant metastasis
- 单细胞样本：TIL 15、adjacent normal lung 12、TDLN 3，另含 distant metastasis
- 物种/器官：Homo sapiens；NSCLC tumour、normal lung、TDLN
- 单细胞规模：560,916 QC-passed T cells；10x 5' scRNA-seq + paired V(D)J/TCR-seq
- 关键标签：MANAFEST/ViraFEST 找到 MANA-specific、influenza-specific、EBV-specific 等 TCRs，再在 scRNA/TCR atlas 中追踪
- 核心目标：比较 neoantigen-specific TIL 在 anti-PD-1 后的 transcriptional programs，并解释 MPR 与 non-MPR 中 tumour-specific T cells 的功能差异

## 算法贡献
- 把 antigen specificity 变成 T-cell state inference 的监督信号，而不是事后 marker 推断。
- 使用 TCR barcode tracking 在 tumour、normal lung、TDLN 和 metastasis 中定位同一 specificity 的 T cells。
- 比较 MANA-specific 与 virus-specific T cells，区分 tumour antigen-driven programs 与一般 viral memory programs。
- 使用 pseudobulk、permutation、pseudotime/dynamic-gene analyses 比较 tumour-specific programs。
- 作者脚本覆盖 clustering、PCA/CCA、Raisin differential abundance/DE、Monte Carlo 和 pseudotime analyses。

## 文章中的算法/分析流程
### 1. 外部实验定义 antigen-specific TCR labels
- MANAFEST 用患者突变相关 neoantigen peptide stimulation 识别 mutation-associated neoantigen (MANA)-specific T-cell clones。
- ViraFEST/viral peptide stimulation 识别 influenza/EBV 等 virus-specific TCRs。
- 输出是 antigen-specific TCR CDR3 barcode table；这些 barcode 是后续 single-cell atlas 里的监督标签。

### 2. 大规模 scRNA/TCR atlas 与 barcode 回填
- 10x 5' scRNA-seq 和 V(D)J 同时获取 expression profile 与 paired TCR。
- 通过 TCR CDR3 match，把 MANAFEST/ViraFEST 中的 antigen-specific clonotypes 映射到 single-cell clusters。
- 这个流程把算法任务从 `cluster annotation` 改成 `specificity-labelled state/program estimation`。

### 3. 差异程序与 response-aware comparison
- 在 MANA-specific T cells 内比较 MPR 与 non-MPR tumours。
- 使用 differential expression、pseudobulk comparison、cluster proportion/permutation tests 等分析 antigen-specific programs。
- 重点程序包括 tissue-resident memory、effector activation、checkpoint expression、IL-7 response 相关状态。

### 4. Pseudotime / RNA velocity / dynamic genes
- 对包含 MANA-specific cells 的相关 T-cell clusters 做 trajectory/pseudotime 分析。
- 使用 SAVER-imputed expression、B-spline temporal trend 和 permutation-based testing 查找 pseudotime dynamic genes。
- 算法意义是把 antigen-specific cells 放入状态连续变化框架，而不是只比较 cluster labels。

## 对算法工作的启发
1. **specificity-supervised representation learning**：功能验证 TCR label 可作为弱监督/强监督信号，训练 tumour-reactive T-cell classifier。
2. **rare clone inference**：MANA-specific cells 数量少，适合发展 borrow-strength 的 hierarchical Bayesian model。
3. **antigen abundance + TCR signal + state joint model**：目前 antigen specificity、TCR sequence、transcriptome state 和 response outcome 仍是分析层拼接。
4. **跨癌种 transfer**：NSCLC specificity labels 可与 melanoma/CRC neoantigen-specific datasets 联合，训练 pan-cancer tumour-reactive T-cell program。
5. **response-aware causality**：MPR/non-MPR 差异不能简单解释为 T-cell-intrinsic，需要模型纳入 tumour mutation、STK11/KRAS context、TME 和 sampling site。

## 数据可用性
- 文章 DOI：https://doi.org/10.1038/s41586-021-03752-4
- processed single-cell GEO：`GSE176022`，链接 <https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE176022>
- BioProject：`PRJNA725225`
- raw scRNA/TCR EGA：`EGAS00001005343`
- bulk TCR GEO：`GSE173351`，链接 <https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE173351>
- ImmuneACCESS DOI：https://doi.org/10.21417/JC2021N
- 数据性质：20 名 neoadjuvant nivolumab-treated NSCLC patients；human tumour/normal lung/TDLN/metastasis T cells；560,916 QC-passed T cells；10x scRNA + paired TCR；bulk TCR；MANAFEST/ViraFEST specificity label
- 数据访问限制：processed/de-identified single-cell 数据在 GEO；raw human single-cell/TCR 数据在 EGA controlled access；bulk TCR 可通过 GEO/ImmuneACCESS 获取
- 论文网页：<https://www.nature.com/articles/s41586-021-03752-4>
- PMC 入口：<https://pmc.ncbi.nlm.nih.gov/articles/PMC8338555/>

## 代码可用性
- 代码仓库：<https://github.com/BKI-immuno/neoantigen-specific-T-cells-NSCLC>
- 代码输入：
  - processed single-cell expression/TCR objects
  - cluster/state metadata
  - antigen-specific TCR barcode tables from MANAFEST/ViraFEST
  - response labels such as MPR/non-MPR
  - pseudotime branch objects and gene-set filters
- 代码输出：
  - clustering and cell-state figures
  - PCA/CCA comparisons
  - Raisin differential cluster/proportion results
  - Monte Carlo/permutation test outputs
  - pseudotime dynamic gene results and manuscript plots
- 模型结构与意义：
  - 仓库是 analysis scripts，不是可直接调用的通用预测包。
  - 方法结构可以概括为 `functional specificity assay -> TCR barcode table -> scRNA/TCR atlas mapping -> state/program/response comparison`。
  - 对新算法而言，最有价值的是真实 specificity labels 和 rare antigen-specific cell benchmark。

## 新算法空间
1. Specificity-supervised T-cell representation
2. Rare antigen-specific clone state inference
3. Antigen abundance/TCR signalling/state joint model
4. Response-aware tumour-specific TIL classifier
5. 跨 NSCLC、melanoma、CRC 的 specificity-labelled pan-cancer model

## 对新算法贡献程度
- 直接算法创新：**中等**。主要是整合式分析流程，而非提出通用包。
- 数据/标签贡献：**极高**。功能验证的 antigen-specific TCR labels 与大规模 scRNA/TCR atlas 结合非常稀缺。
- 新算法开发价值：**极高**。适合作为 specificity-supervised、rare-clone-aware、therapy-response-aware 模型的核心 benchmark。

## 可作为我们 method 报告里的位置
建议放在“antigen specificity-aware T-cell algorithms”小节。它证明已有研究已经能把真实 specificity label 接入 single-cell state analysis，但尚未形成统一的 TCR sequence、antigen abundance、cell state 和 clinical response 联合模型。

## 一句话结论
`053` 把 true neoantigen specificity 接入大规模 single-cell TIL map，是 antigen-aware tumour T-cell algorithms 的高价值数据锚点。
