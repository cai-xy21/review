# Algorithm Report 050

## Paper
Global characterization of T cells in non-small-cell lung cancer by single-cell sequencing

## 算法视角定位
`050` 继续把 tumor T-cell single-cell analysis 推向 **state, lineage and prognosis** 三层联动：
- expression clusters 描述 NSCLC T-cell functional states
- TCR-based lineage tracking 比较 inter-tissue T-cell relations
- independent survival analyses 把 pre-exhausted/exhausted and activated Treg signatures 接到 patient stratification

它没有提出新的通用 TCR algorithm，但它很适合说明“已有算法能把 clone/state signature 做到临床相关，仍缺 donor-aware predictive model 与 antigen-specific grounding”。

## 题录与数据
- 年份：2018
- 期刊：Nature Medicine
- DOI：https://doi.org/10.1038/s41591-018-0045-3
- 物种/器官：`Homo sapiens`；treatment-naive non-small-cell lung cancer tumor, adjacent normal lung tissue and peripheral blood
- 主数据：12,346 T cells from 14 NSCLC patients
- 主模态：deep Smart-seq2 scRNA-seq with TCR-based lineage context
- GEO：`GSE99254`
- BioProject：`PRJNA387726`
- raw controlled access：EGA `EGAS00001002430`

## 数据任务定义
1. 在 tumor、adjacent lung 和 peripheral blood 间刻画 NSCLC T-cell composition and functional states。
2. 用 expression and TCR lineage tracking 区分 inter-tissue effector states 与 tumor-local exhaustion。
3. 在 CD8 and Treg compartments 中提取 clinically relevant states。
4. 把 cell-state signatures 映射到 independent patient survival datasets。

## 关键算法问题
1. pre-exhausted 与 exhausted state 是否是稳健 state transition / prognostic feature，而非 cluster naming effect。
2. TCR lineage tracking 如何区分 migration、shared origin 与 convergent state。
3. cell-level signature 如何聚合成 patient-level stratification signal。
4. tumor Treg heterogeneity 与 activation markers 的双峰结构如何建模。

## 详细算法贡献
### 1. Expression-state atlas with inter-tissue design
- GEO 和 Nature 摘要均明确该数据覆盖 tumor、adjacent normal tissue 和 peripheral blood。
- 该设计让算法能比较 peripheral effector memory-like states、tumor exhaustion 和 regulatory programs。

### 2. Expression + TCR lineage tracking
- 文章以 combined expression and T-cell antigen receptor based lineage tracking 识别 inter-tissue effector cells 与 tumor-local dynamics。
- 算法意义在于 lineage context 能限制纯 expression similarity 的过度解释：两个 cells transcriptionally similar，不代表同一 clonal history。
- 但论文层面主要是 lineage-aware analysis，不是 sequence-to-state joint representation。

### 3. Pre-exhausted/exhausted signature ratio
- 作者把两个 exhaustion-preceding CD8 clusters 与 exhausted cells 区分，并把 high pre-exhausted/exhausted ratio 关联到 LUAD prognosis。
- 这把 single-cell state discovery 推进为 patient-level derived feature。
- 对 method report，应该指出它仍依赖 signature aggregation and external cohort transfer，尚未形成校准的 predictive survival model。

### 4. Activated tumor Treg heterogeneity
- 论文强调 TNFRSF9 bimodal activated Treg state，并提取包含 IL1R2 的 activated tumor Treg signature。
- 算法上可看作 within-lineage state discovery + clinical signature transfer。

## 代码专项
- 本轮未在 Nature article, GEO record and public project records 中定位到作者独立代码仓库。
- 可复用对象主要是：
  - 输入：Smart-seq2 count/TPM matrices、cell metadata、TCR typing supplement、tissue/patient labels
  - 输出：T-cell clusters、TCR lineage context、pre-exhausted/exhausted ratio concept、activated Treg signatures and prognosis associations
- 对后续算法开发，Supplementary Table 2 的 single-cell TCR typing 与 GEO count matrices 是最直接的 clone-state starting point。

## 对新算法贡献程度
- 直接算法创新：**低到中**
- clone-state clinical task definition：**高**
- processed dataset benchmark value：**高**
- 对 patient-level prediction 启发：**高**
- 综合判断：**P1 NSCLC T-cell state-lineage-prognosis anchor**

## 数据可用性评估
- DOI：https://doi.org/10.1038/s41591-018-0045-3
- GEO：https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE99254
- GEO accession：`GSE99254`
- BioProject：`PRJNA387726`
- Raw EGA accession：`EGAS00001002430`
- GEO rows/cells：12,346 cell metadata rows
- GEO processed files：
  - `GSE99254_NSCLC.TCell.S12346.count.txt.gz`
  - `GSE99254_NSCLC.TCell.S12346.TPM.txt.gz`
  - centered expression matrix for 11,769 cells
- Nature supplementary resource：patient/sequencing table、single-cell TCR typing table、cluster signature tables、exhausted/Treg differential gene tables
- 可复用性：processed expression and supplement tables are strong; raw reads require EGA access; code gap remains

## 新算法空间
1. **Lineage-aware clinical score learning**
   - 从 handcrafted ratio/signature 走向 donor-level calibrated prognosis or response score。
2. **TCR state migration model**
   - 区分 shared clonality across blood/lung/tumor 与 tissue-induced convergent exhaustion。
3. **Antigen-grounded tumor T-cell states**
   - 将 TCR clonotype、specificity evidence 与 transcriptome state 联合。
4. **Cross-cancer exhaustion transfer**
   - 学习 pre-exhausted/exhausted axes 的 conserved and tumor-specific components。

## 最终判断
`050` 是 tumor T-cell method narrative 中非常好用的一篇：已有分析已经把 state、lineage 和 survival signature 接起来，但新算法空间仍在 donor-aware、specificity-aware、uncertainty-aware 的统一模型。
