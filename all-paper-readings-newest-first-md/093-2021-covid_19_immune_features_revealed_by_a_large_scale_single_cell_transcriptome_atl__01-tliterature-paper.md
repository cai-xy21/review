# 076. COVID-19 immune features revealed by a large-scale single-cell transcriptome atlas

## 基本信息
- 年份：2021
- 期刊：Cell
- DOI：https://doi.org/10.1016/j.cell.2021.01.053
- Erratum DOI：https://doi.org/10.1016/j.cell.2021.10.023
- 主题：COVID-19 large-scale atlas；host-pathogen scRNA；clinical association；T-cell states

## 为什么重要
这是 COVID-19 单细胞图谱中规模很大的资源型论文之一：284 个 human samples、196 名 COVID-19 patients and controls、约 1.46 million cells，覆盖 PBMC、BALF、sputum、pleural fluid 等样本类型。它适合作为 large-scale disease atlas、cross-tissue infection state transfer 和 host-pathogen modeling 的算法 benchmark。

## 数据与研究设计
- 样本对象：196 名 COVID-19 patients and controls
- 样本规模：284 human samples；QC 后 1,462,702 cells
- 样本类型：249 PBMC-related samples；35 respiratory-system samples，包括 12 BALF、22 sputum、1 pleural-fluid mononuclear-cell sample
- 物种/器官：Homo sapiens；peripheral blood、bronchoalveolar lavage fluid、sputum、pleural fluid
- 模态：scRNA-seq；部分 5' V(D)J/TCR/BCR-seq；cytokine measurements；IHC validation；viral RNA read detection
- 研究目标：构建 COVID-19 大规模单细胞图谱，关联 severity、stage、age、sex、sample tissue 和 SARS-CoV-2 RNA-positive cells

## 核心亮点
1. **百万级 disease atlas**：细胞数量和样本类型丰富，适合 scalable annotation 和 metacell/foundation representation。
2. **跨组织感染图谱**：PBMC 与 respiratory samples 同框分析，可研究 blood-lung immune state transfer。
3. **viral RNA-positive cell detection**：在 single-cell reads 中检测 SARS-CoV-2 RNA，提供 host-pathogen joint modeling 任务。
4. **clinical-feature association**：将 cell-state abundance/expression 与 severity、stage、age、sex 等临床变量关联。

## 核心贡献
- 注释 COVID-19 相关 major lineages 和 64 个 finer clusters，包括多种 T-cell、B/plasma、myeloid、NK、epithelial states。
- 用 observed/expected enrichment ratio 分析 cell clusters 对 tissue、clinical group 或 disease stage 的偏好。
- 识别 hyper-inflammatory monocyte/macrophage/megakaryocyte/T-cell subtypes 与 cytokine/inflammatory scores。
- 构建公开 portal、GEO/GSA-Human/Mendeley 数据资源，便于大规模二次分析。

## 与 T 细胞-人群免疫力的关系
该文包含丰富 CD4/CD8 T-cell clusters，并将其与 severity、stage 和 inflammatory programs 关联。它说明 T-cell state 在 COVID-19 中必须同时考虑 tissue compartment、viral exposure、age、severity 和 myeloid/cytokine environment。

## 文章中的算法/分析流程
### 1. Large-scale scRNA quantification and annotation
作者使用 kallisto/bustools 对 reads 计数，经过 UMI/gene/mitochondrial/erythrocyte filters 后构建百万级 atlas，并进行 major lineage 和 fine cluster annotation。

### 2. Observed/expected enrichment
使用 observed-to-expected cell number ratio (`R_O/E`) 评估 clusters 在 tissue 或 clinical groups 中的富集。该方法直观适合 atlas 展示，但没有显式处理 donor random effects 和 compositional dependency。

### 3. Clinical-feature association
将 cell subtype proportions/expression 与 severity、stage、age、sex 等变量关联，定义 `cell state x clinical covariate` 的 population-immunity mapping task。

### 4. Viral RNA-positive analysis
构建包含 SARS-CoV-2 genome 的 custom reference，检测 viral RNA-positive cells，比较 viral-positive vs viral-negative cell profiles。算法上可发展为 viral-read-aware cell calling 和 ambient viral RNA correction。

## 对算法工作的启发
1. **Million-cell scalable annotation**：metacell、reference mapping、foundation model 都可用该 atlas 压力测试。
2. **Donor-aware association model**：替代简单 `R_O/E`，用 mixed/compositional models 做 clinical covariate inference。
3. **Host-pathogen joint model**：同时建模 viral reads、ambient contamination、host cell state 和 tissue compartment。
4. **Clone-state-severity model**：利用 V(D)J 子集将 TCR/BCR expansion 与 severity/stage/state 联合。

## 数据可用性
- Processed gene expression GEO：GSE158055
- Raw GSA-Human：HRA001149
- GSA-Human 链接：<https://ngdc.cncb.ac.cn/gsa-human/browse/HRA001149>
- BioProject：PRJNA663865
- HCA project：<https://explore.data.humancellatlas.org/projects/5f607e50-ba22-4598-b1e9-f3d9d7a35dcc>
- Visualization portal：<http://covid19.cancer-pku.cn>
- Supplemental data：Mendeley Data <https://doi.org/10.17632/dvp4y5ttd5.1>
- 数据性质：human PBMC and respiratory samples；196 individuals；284 samples；1.46M cells；severity/stage labels；部分 V(D)J
- 代码：Data Availability 未定位独立完整 GitHub；主流程需按 Methods 复现
- 代码输入/输出（按论文流程抽象）：
  - 输入：raw FASTQ、human GRCh38 + SARS-CoV-2 custom reference、sample/clinical metadata、V(D)J calls where available
  - 输出：annotated atlas、cluster enrichment statistics、viral-positive cell profiles、cytokine/inflammatory scores、portal visualizations

## 可信度评估
- 期刊层面：Cell，高可信度
- 可复现性：processed/raw accession、portal、supplement 明确；raw human data 访问可能受控；viral-read detection 需严格复现 alignment/filtering
- 局限：多中心/多组织 confounding；clinical inference 应以 donor/sample 为单位；TCR/BCR 信息未形成统一 clone-state model
- 综合判断：**资源价值很高，直接算法创新中等，host-pathogen and large-atlas algorithm value 很高**

## 一句话结论
这篇文章应作为 million-cell COVID disease atlas 的核心 benchmark：它提供了跨组织、临床变量和 viral-read-aware analysis 的真实复杂场景。
