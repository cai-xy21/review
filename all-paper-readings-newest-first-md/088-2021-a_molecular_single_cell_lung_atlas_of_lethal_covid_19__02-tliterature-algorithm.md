# Algorithm Report 035

## Paper
A molecular single-cell lung atlas of lethal COVID-19

## 题录与数据
- 年份/期刊：2021, Nature
- DOI：https://doi.org/10.1038/s41586-021-03569-1
- Processed data：GEO `GSE171524`；Broad portal `SCP1219`
- Raw data：DUOS `DUOS-000130`
- Code：<https://github.com/IzarLab/CUIMC-NYP_COVID_autopsy_lung>
- Dataset：`>116k` lung nuclei；`19` COVID autopsy decedents and `7` pre-pandemic controls

## 算法视角定位
这是 **tissue single-cell atlas + pathological circuit inference**。它对 T-cell methods 的直接创新一般，但对 severe infection modeling 很强，因为数据处在 organ damage site，而非 peripheral blood proxy。

## 输入、处理与输出
### 输入
- lung snRNA-seq reads/counts from COVID autopsy and control lung tissue
- human/SARS-CoV-2 reference
- cell signatures, metadata and histologic context

### 处理链
1. Cell Ranger alignment/count generation against joint reference
2. CellBender artifact/ambient count removal
3. Seurat QC and integration
4. iterative cell/state annotation using markers, reference signatures and manual curation
5. condition/state comparison
6. protein-activity and ligand-receptor network interpretation

### 输出
- lung atlas labels at multiple granularity levels
- COVID/control cell-fraction and state summaries
- pathological epithelial/fibroblast/immune programs
- candidate deleterious circuits and therapeutic hypotheses

## 详细算法贡献
### 1. Artifact-aware tissue atlas path
Autopsy lung nuclei are technically demanding. The paper makes artifact removal and iterative annotation part of the public atlas logic rather than hiding it behind final UMAPs.

### 2. Circuit interpretation beyond abundance
The relevant algorithmic object is not only cell type proportion but a tissue graph: immune, epithelial and stromal states connected by inferred activities and ligand-receptor edges.

### 3. Reproducibility artifacts
The code repository exposes code folders, signatures and a `code_overview.csv` map to figures/tables. That makes the atlas more reusable than papers with only static source data.

## 代码输入输出补充
- Repository input level：processed atlas objects/metadata/signatures plus figure-specific resources
- Repository output level：R/Python figure analyses and interpretation outputs
- Upstream reconstruction requires alignment/artifact removal/integration tools stated in the study and access to raw or processed matrices

## 对新算法贡献程度
- 任务定义：高
- 数据与代码资源：高
- 直接新模型：低到中
- tissue-aware immunity motivation：很高

## 可开发空间
1. **Tissue circuit representation learning**：以 cell-state graph joint model 替代后验 ligand-receptor tables。
2. **Outcome-conditioned tissue atlas**：把 decedent/control、pathology severity 与 donor covariates 进入 hierarchical model。
3. **Blood-to-lung transfer**：量化 peripheral T-cell signature 对 lung tissue state 的可迁移性和置信度。

## 数据可用性评估
- 物种/器官：`Homo sapiens`；lung tissue
- Processed：open GEO and portal
- Raw：controlled DUOS
- Code：public GitHub repo
- 可复用性：**高 for tissue atlas and circuit benchmarks**；**中 for T-cell-specific algorithm**, 因为缺 receptor modality

## 可纳入 method report 的一句话
Lethal COVID lung atlases show that severe infection algorithms must model organ context and cell-cell circuits, not only peripheral immune composition.
