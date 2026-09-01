# Algorithm Report 053

## Paper
Transcriptional programs of neoantigen-specific TIL in anti-PD-1-treated lung cancers

## 算法视角定位
`053` 的核心算法价值是把 antigen specificity 做成 single-cell state supervision。作者先用 MANAFEST/ViraFEST 找到 MANA- or virus-specific TCRs，再把 TCR 当 barcode 回填到大规模 `scRNA-seq + TCR-seq` atlas，从而比较 true tumour-specific 与 virus-specific TIL programs。

## 题录与数据
- 年份：2021
- 期刊：Nature
- DOI：https://doi.org/10.1038/s41586-021-03752-4
- 物种/器官：`Homo sapiens`；anti-PD-1-treated NSCLC tumour, adjacent normal lung, tumour-draining lymph node and one metastasis
- clinical cohort：20 neoadjuvant nivolumab NSCLC patients；single-cell samples include TIL `n=15`, normal lung `n=12`, TDLN `n=3`
- single-cell scale：560,916 QC-passed T cells
- processed GEO：`GSE176022`
- raw EGA：`EGAS00001005343`
- bulk TCR GEO：`GSE173351`
- ImmuneACCESS bulk TCR DOI：`https://doi.org/10.21417/JC2021N`
- scripts：https://github.com/BKI-immuno/neoantigen-specific-T-cells-NSCLC

## 详细算法贡献
### 1. Specificity-labelled T-cell atlas
- MANAFEST-derived neoantigen-specific TCRs and viral TCRs turn clonotype into a label for biological supervision。
- This changes the task from `dysfunctional cluster annotation` to `specificity-linked program estimation`。

### 2. Large-scale scRNA/TCR barcode tracking
- Coupled scRNA/TCR data allow known antigen-specific TCRs to be tracked inside 15-cluster tumour/normal/TDLN T-cell map。
- It provides an unusually large benchmark for low-frequency antigen-specific clone states under therapy。

### 3. Pseudobulk, permutation and pseudotime analysis
- Nature methods use cluster-level pseudobulk profiles to compare tumour versus normal and MPR versus non-MPR samples。
- Repo includes `PCA_CCA`, `Raisin` differential cluster analysis, Monte Carlo simulation and `Pseudotime` code。
- Methods further fit B-spline expression trends along pseudotime after SAVER imputation and test dynamic genes by permutation.

### 4. Response-aware specificity comparison
- The work separates MANA-specific cells in responding versus non-responding tumours and relates signalling/checkpoint/TRM programs to outcome context。
- It exposes the algorithm need to model antigen specificity, treatment response and tissue-resident programs jointly.

## 代码专项
- Inputs: processed single-cell expression/TCR objects, cluster/state metadata, antigen-specific TCR barcode tables, pseudotime branch objects and gene-set filters。
- Outputs: cluster DE, PCA/CCA comparisons, pseudotime dynamic genes, Monte Carlo tests and manuscript figures。
- Repository is figure/analysis scripts, not a packaged predictive model。
- Model structure meaning: specificity labels are externally validated by functional assay, then used to supervise downstream state/program inference.

## 对新算法贡献程度
- Direct new general algorithm：**中等**
- Specificity-supervised task value：**极高**
- Data/benchmark value：**极高**
- 综合判断：**P1 antigen-specific single-cell TIL benchmark**

## 数据可用性评估
- Raw scRNA-TCR：EGA controlled `EGAS00001005343`
- Processed/de-identified single-cell：GEO `GSE176022`
- Bulk TCR：GEO `GSE173351` and ImmuneACCESS DOI `https://doi.org/10.21417/JC2021N`
- Code repository publicly available
- Article has a 4 October 2021 Author Correction；报告 DOI 使用正式 article DOI。

## 新算法空间
1. Specificity-aware multimodal representation trained with functional labels and weak unlabeled clonotypes。
2. Donor/treatment-aware rare clone program estimation。
3. Integrate TCR signalling strength, antigen abundance and transcriptome state。
4. Calibrated transfer from MANA-specific NSCLC TIL to other tumours/therapies。

## 最终判断
`053` 是“真正 tumour-specific T cells 应如何进入 single-cell算法”的关键条目。它的数据和代码把 specificity supervision 提供出来，正好支撑后续新算法立项。
