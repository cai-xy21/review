# Algorithm Report 023

## Paper
Analyzing the Mycobacterium tuberculosis immune response by T-cell receptor clustering with GLIPH2 and genome-wide antigen screening

## 题录与资源
- DOI：https://doi.org/10.1038/s41587-020-0505-4
- 输入 repertoire：58 名 latent Mtb infected individuals，`19,044` unique TCR beta sequences
- validation screen：`3,724` Mtb proteins，约覆盖 `95%` protein-coding genes
- 代码：Nature Supplementary Code 中 two compiled standalone versions of GLIPH2

## 算法定位
GLIPH2 是 TCR repertoire specificity-grouping algorithm。它面向跨 clonotypes 的 receptor convergence：哪些 CDR3 beta sequences 因共享 motif、global similarity 与 cohort evidence 而可能识别相似 antigen。

## 输入
- TCR beta CDR3 sequences
- V gene / repertoire fields
- subject-level metadata 与 counts
- 可用于 group enrichment 的 HLA 或 cohort metadata

## 输出
- candidate specificity groups
- local motif / global similarity grouping results
- group-level statistical evidence，如 convergence、V/HLA enrichment
- 可接 antigen testing 的 prioritized TCR groups

## 算法贡献拆解
1. **从 clonotype 到 specificity group**：解决不同序列是否可能收敛到共同 antigen recognition。
2. **sequence evidence + cohort evidence**：不把 clustering 只交给字符串距离。
3. **scale**：论文将 GLIPH2 定位为可处理 millions of TCR sequences 的改进版本。
4. **functional closure**：用 Mtb proteome-scale antigen screen 检验算法生成的 hypotheses。

## 与单细胞算法的关系
- 单细胞 VDJ 数据可为 GLIPH2 提供 sequence 与 cell metadata。
- GLIPH2 输出的 groups 可回填到 transcriptome/ADT state atlas。
- 但该方法本身不学习 scRNA latent state，也不把 paired alpha-beta receptor 与 cell phenotype 一起建模。

## 对新算法贡献程度
- 直接算法创新：**高**
- T 细胞问题直接性：**很高**
- 单细胞 multi-omics 直接性：**中**
- 数据复用性：**中高**

## 数据与代码可用性
- Supplementary Tables 给出 Mtb-specific TCR sequences、VDJdb sequences、specificity groups 与 Mtb ORF screen gene list
- 未在论文页面定位到主要 GEO/SRA repertoire accession
- code 是 compiled supplementary executables，工程上可用但不等同于易审计现代 package

## 新算法空间
1. paired alpha-beta sequence model with GLIPH-like cohort statistics
2. clone/specficity group-aware RNA-protein latent space
3. HLA/donor-aware confidence calibration
4. antigen-specific immune-response predictors across infection/vaccination cohorts

## 最终判断
GLIPH2 是“受体序列如何进入人群 T-cell algorithm”这一段必须写的代表。它已经能生成 specificity hypotheses，尚未解决这些 hypotheses 如何与单细胞 phenotype 和 donor-level immunity 统一建模。
