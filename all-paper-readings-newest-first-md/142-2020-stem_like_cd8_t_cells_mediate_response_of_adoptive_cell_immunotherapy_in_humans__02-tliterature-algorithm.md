# Algorithm Report 057

## Paper
Stem-like CD8 T cells mediate response of adoptive cell immunotherapy against human cancer

## 算法视角定位
`057` is best treated as an adoptive-cell-therapy response-state anchor rather than a broadly reusable single-cell method paper. It uses high-dimensional cell-state analysis of TIL infusion products, including single-cell transcriptomic clustering/signatures, to contrast stem-like versus terminally differentiated ACT products.

## 题录与数据
- 年份：2020
- 期刊：Science
- DOI：https://doi.org/10.1126/science.abb9847
- PMID：33303615
- PMCID：PMC8883579
- 物种/器官：human tumour-infiltrating lymphocyte infusion products for adoptive cell immunotherapy, focused on metastatic melanoma context
- public article data statement located in current search: data are presented in the paper/supplementary materials; 本轮未定位 GEO/SRA/EGA/dbGaP accession

## 数据任务定义
1. 对 ACT infusion products 按 response context 比较 T-cell phenotypic states。
2. 从 CD39/CD69-defined products 提取 stem-like versus differentiated CD8 transcriptional programs。
3. 将 cell-state composition 与 post-transfer persistence/clinical response 连接。

## 详细算法贡献
### 1. Therapy-product state stratification
- ACT introduces a different population unit: manufactured/expanded infusion product rather than untreated tumour biopsy。
- The paper uses clustering and gene signatures to compare responder-enriched stem-like and non-responder-enriched differentiated states。

### 2. Compact phenotype linked to transcriptional program
- CD39- CD69- CD8 TIL product fraction is tied to a memory progenitor/stem-like program, whereas CD39+ CD69+ products are more terminal。
- Algorithmically this is another marker-panel compression of an underlying single-cell state manifold。

### 3. Response benchmark boundary
- The study is valuable for outcome framing, but public raw single-cell accession/code is much weaker than `051-056/058`。
- For a method paper, it motivates ACT product quality modeling more than it supplies a large downloadable benchmark.

## 代码专项
- 本轮未定位作者独立 code repository 或可安装 algorithm package。
- Practical inputs if reanalyzing article-level results: infusion-product cell phenotypes, single-cell cluster/signature tables, responder/non-responder clinical metadata and post-transfer persistence measures。
- Outputs: product state proportions, stem-like/differentiated signatures and response-associated comparisons。

## 对新算法贡献程度
- 直接算法创新：**低**
- therapy-product task definition：**高**
- open-data benchmark value：**低到中**
- 综合判断：**P1 biological motivation; weaker accession-level computational benchmark**

## 新算法空间
1. ACT product quality score from single-cell states with manufacturing covariates。
2. Pre-infusion state to post-transfer persistence model。
3. Robust transfer from markers such as CD39/CD69 to transcriptomic stemness with uncertainty。
4. Joint model across endogenous TIL, infusion product and post-ACT blood/tumour observations。

## 最终判断
`057` should remain in the method narrative as an ACT response-state motivation, but data openness and public code should be described cautiously.
