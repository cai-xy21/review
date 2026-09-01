# Algorithm Report 040

## Paper
Single-cell analysis of Crohn's disease lesions identifies a pathogenic cellular module associated with resistance to anti-TNF therapy

## 题录与数据
- 年份/期刊：2019, Cell
- DOI：https://doi.org/10.1016/j.cell.2019.08.008
- PMID/PMCID：31474370 / PMC7060942
- scRNA data：GEO `GSE134809`；BioProject `PRJNA556461`
- Interactive resource：`scDissector` Martin resource
- GEO study page records `31` samples

## 算法视角定位
这是 **patient-level tissue module discovery and treatment-response association**。它的核心不是提出 clustering algorithm，而是把 cell states组合成跨 compartment lesion module，再向 therapy outcome迁移。

## 输入、处理与输出
### 输入
- lesion/non-lesion/blood single-cell expression profiles
- cell subtype annotations and patient/sample labels
- validation cohort expression/outcome data

### 处理链
1. tissue single-cell clustering and subtype/state annotation
2. cell-state program comparison across patients and tissue contexts
3. pathogenic module discovery across plasma, myeloid, activated T-cell and stromal compartments
4. ligand-receptor connectivity analysis
5. validation of module signature in independent iCD cohorts and anti-TNF outcome association

### 输出
- lesion cell-state atlas
- GIMATS module/signature
- inferred intercellular connectivity summaries
- patient stratification and anti-TNF resistance association

## 详细算法贡献
### 1. Cell atlas to patient module
The paper changes the computational target from “what clusters exist” to “which multicellular lesion program marks a subgroup of patients”.

### 2. Cross-compartment T-cell context
Activated T cells matter because they co-occur with inflammatory myeloid, plasma and stromal programs. That structure is a better target for tissue immunity algorithms than isolated T-cell markers.

### 3. Transfer across data scales
The module is evaluated beyond the discovery single-cell cohort in larger independent cohorts. This opens the algorithmic problem of transferring a cell-resolved module to bulk or heterogeneous clinical cohorts.

## 模型结构与意义
- No standalone model/package is introduced.
- Reusable structure：`single-cell subtype programs -> multicellular module -> interaction network -> clinical cohort validation`
- Meaning：a strong precedent for module-aware and outcome-aware tissue immune modeling.

## 对新算法贡献程度
- 任务定义：很高
- 数据资源：高
- direct algorithm novelty：中
- clinical modeling motivation：很高

## 可开发空间
1. **Multicellular module encoder**：learn lesion-level embeddings from cell mixtures and cell-state programs.
2. **Interaction-aware treatment predictor**：combine T-cell states, myeloid/stromal edges and patient covariates.
3. **Cross-platform calibration**：map scRNA modules into bulk/microarray/clinical cohorts with uncertainty.

## 数据与代码评估
- Species/organ：`Homo sapiens`；ileal Crohn's disease tissue and blood context
- Public data：GEO/BioProject and scDissector resource
- External validation references：paper cites bulk/microarray GEO cohorts including `GSE83687` and `GSE112366`
- Author code：no dedicated public repo located in this pass
- 可复用性：**high** for tissue-module benchmarks；**medium** for exact analysis reproduction because code is not packaged

## 可纳入 method report 的一句话
Crohn lesion single-cell analysis demonstrates that clinically relevant T-cell signals often live inside multicellular tissue modules, motivating module-aware immune algorithms linked to therapy outcome.
