# Algorithm Report 037

## Paper
Longitudinal analyses reveal immunological misfiring in severe COVID-19

## 题录与数据
- 年份/期刊：2020, Nature
- DOI：https://doi.org/10.1038/s41586-020-2588-y
- Cohort：serial immune profiling of `113` moderate/severe COVID-19 patients
- Public study accession：ImmPort `SDY1655`
- Boundary label：longitudinal systems immunology, not scRNA-seq

## 算法视角定位
This is a **donor-time immune trajectory anchor**. It matters for a single-cell method report because it supplies the outcome/trajectory structure that cell-rich atlases often lack, but it should not be counted as a single-cell algorithm contribution.

## 输入、处理与输出
### 输入
- serial cytokine/chemokine measurements
- immune-cell and T-cell profiling summaries
- viral and clinical metadata

### 处理
1. unsupervised immune-signature clustering
2. longitudinal association with viral load, clinical course and severity/outcome
3. interpretation of adaptive/innate misfiring patterns

### 输出
- immune signature groups
- patient/disease trajectory associations
- outcome-linked hypotheses for severe COVID-19 immune misfiring

## 详细算法贡献
### 1. Outcome-aware framing
The reusable lesson is the modeling target: donor-level trajectories and outcome-linked signatures, not only static differential cell states.

### 2. Boundary with scRNA work
Single-cell methods can use this paper to motivate:
- temporal donor aggregation
- multimodal immune state scores
- early disease-course prediction

But this paper itself does not provide count matrices, clonotypes or cell-state embeddings.

## 对新算法贡献程度
- 任务定义：高
- single-cell data resource：低
- direct new algorithm：低
- method-report framing value：高

## 可开发空间
1. **Cell-to-trajectory model**：aggregate cell states into donor-time features.
2. **Early-warning immune trajectory classifier** with calibrated uncertainty.
3. **Joint sparse multimodal model** for cytokines, scRNA, TCR and clinical metadata.

## 数据与代码评估
- Accession：ImmPort `SDY1655`
- Author code：no public dedicated analysis repo located in this pass
- 可复用性：**high for longitudinal outcome framing**；**low for single-cell/TCR benchmark**

## 可纳入 method report 的一句话
Longitudinal COVID systems-immunology studies define the donor-time outcome target that static single-cell immune atlases still struggle to model.
