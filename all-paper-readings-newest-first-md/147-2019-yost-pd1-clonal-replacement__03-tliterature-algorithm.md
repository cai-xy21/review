# Algorithm Report 054

## Paper
Clonal replacement of tumor-specific T cells following PD-1 blockade

## 算法视角定位
`054` 是 therapy-time clone-state tracking anchor。它不提出 packaged method，但 paired pre/post anti-PD-1 tumours、paired scRNA/TCR and bulk TCR readouts 把一个关键问题写清楚：checkpoint response 可能是 novel tumour-entering clonotypes 的 replacement，而不是仅 reinvigorating pre-existing exhausted TILs。

## 题录与数据
- 年份：2019
- 期刊：Nature Medicine
- DOI：https://doi.org/10.1038/s41591-019-0522-3
- 物种/器官：`Homo sapiens`；site-matched basal cell carcinoma and squamous cell carcinoma skin tumours pre/post anti-PD-1
- paired single-cell scale：79,046 cells with RNA/TCR context
- GEO：`GSE123814`
- exome SRA BioProject：`PRJNA533341`
- bulk TCR ImmuneACCESS DOI：https://doi.org/10.21417/KY2019NM

## 详细算法贡献
### 1. Paired therapy-time clone map
- Pre/post samples and site matching define clonal persistence versus replacement as a measurable task。
- Exact TCR clonotypes link transcriptional dysfunction states with therapy-time dynamics。

### 2. Novel-clone versus pre-existing-clone decomposition
- Expanded post-treatment exhausted CD8 clonotypes are compared to clones observed before therapy。
- The result focuses algorithm attention on repertoire turnover, not just cell-state activation scores。

### 3. Multiscale data linkage
- scRNA gives phenotype, scTCR gives clone identity, bulk TCR deepens repertoire observation, exome contributes tumour context。
- This is a template for longitudinal multimodal immune monitoring even without a new latent model。

## 代码专项
- Nature Medicine code statement: all custom code available from corresponding authors on reasonable request。
- Public reusable inputs: GEO ensemble/scRNA matrices and metadata, single-cell TCR information from study records/supplements, bulk TCR ImmuneACCESS data, exome SRA。
- Outputs to reproduce: CD8 CD39/dysfunction state summaries, pre/post clone sharing, expanded-clone novelty and clonal replacement comparisons。
- Report boundary: no public author analysis repository was located in the article code statement.

## 对新算法贡献程度
- 直接算法创新：**低到中**
- longitudinal clone-state task definition：**很高**
- immunotherapy benchmark value：**高**
- 综合判断：**P1 pre/post clonal replacement benchmark**

## 数据可用性评估
- GEO：https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE123814
- Exome SRA/BioProject：`PRJNA533341`
- Bulk TCR public ImmuneACCESS DOI link above
- Raw/processed single-cell availability follows GEO record; article does not expose public code repository

## 新算法空间
1. Longitudinal clone replacement model with sampling/dropout correction。
2. Distinguish immigration, local expansion and phenotypic conversion。
3. Combine pre/post tumour, blood and antigen specificity for mechanism inference。
4. Donor-level response prediction from repertoire turnover uncertainty。

## 最终判断
`054` 最重要的算法贡献是定义 therapy-time clonal turnover problem。现有分析可证 replacement，后续算法应估计 replacement 的不确定性和机制。
