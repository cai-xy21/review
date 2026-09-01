# Algorithm Report 056

## Paper
Defining T cell states associated with response to checkpoint immunotherapy in melanoma

## 算法视角定位
`056` 是 response-associated T-cell state discovery 的早期核心论文。它不发明一个新 integration model，而是用 responder/non-responder context 把 tumour scRNA states、TCR clonality、epigenetic support 和 independent cohort validation 连成 workflow，提出 CD8 state balance 和 `TCF7`-linked response readout。

## 题录与数据
- 年份：2018
- 期刊：Cell
- DOI：https://doi.org/10.1016/j.cell.2018.10.038
- 物种/器官：`Homo sapiens`；melanoma tumour immune infiltrates under checkpoint inhibitors
- scRNA scale：16,291 CD45+ immune cells from 48 melanoma tumour samples
- protocol：Smart-seq2
- GEO：`GSE120575`
- raw dbGaP：`phs001680.v1.p1`
- BioProject：`PRJNA493623`

## 数据任务定义
1. 在 checkpoint therapy tumours 中找 response-associated immune/T-cell states。
2. 比较 CD8 cell-state balance 与 tumour regression/progression。
3. 从 single-cell transcriptomic observation 提炼可在 fixed tissue/independent cohort 验证的 marker。
4. 将 state discovery 连接潜在 combination targets。

## 详细算法贡献
### 1. Outcome-conditioned state discovery
- Rather than cluster melanoma TILs without clinical axes, the paper compares cell states with response/progression context。
- Two CD8 states and their balance become patient-relevant features, not merely cell-level cluster labels。

### 2. Marker compression
- `TCF7` reduces a high-dimensional favourable CD8-like state into an interpretable marker validated by tissue imaging and survival/outcome analyses。
- For algorithms this is a feature distillation problem: multi-gene latent state to clinically deployable readout。

### 3. Multi-evidence state interpretation
- Summary states the work delineates epigenetic landscape and clonality for the CD8 states and tests exhaustion-linked combination targets。
- It demonstrates that transcriptome state discovery benefits from orthogonal lineage/regulatory evidence。

## 代码专项
- 本轮在 GEO record 和 article landing page 未定位到作者公开 analysis repository。
- Inputs available to reusers: GEO TPM matrix, patient/cell metadata, pre/post sample labels, response context, dbGaP raw reads under controlled access。
- Outputs to reproduce: cell clusters, CD8 responder-associated state scores, `TCF7` readout comparisons and state-associated marker lists。
- Boundary: it is a clinically framed analysis workflow, not a public model package.

## 对新算法贡献程度
- 直接算法创新：**低到中**
- therapy-response state task value：**很高**
- translational marker value：**高**
- 综合判断：**P1 checkpoint-response T-cell state anchor**

## 数据可用性评估
- GEO：https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE120575
- GEO supplementary: TPM matrix and patient-ID/cell files
- GEO record states raw human files submitted to dbGaP `phs001680.v1.p1`
- Samples in GEO include pre and post checkpoint therapy tumour records

## 新算法空间
1. Donor-aware response model from state abundance and state intensity rather than cell pooling。
2. Marker distillation with calibration and spatial validation。
3. Joint state-clonality-epigenetic response representation。
4. Cross-cohort response-state transfer beyond melanoma。

## 最终判断
`056` 适合用来说明 existing algorithms can derive response-associated T-cell states, but the field still lacks calibrated patient-level models that transport across therapy cohorts.
