# 036. COVID-19 severity correlates with airway epithelium-immune cell interactions identified by single-cell analysis

## 基本信息
- 年份：2020
- 期刊：Nature Biotechnology
- DOI：https://doi.org/10.1038/s41587-020-0602-4
- PMID：32591762
- 主题：airway mucosa；COVID severity；epithelial-immune interaction；nasopharyngeal and bronchial scRNA-seq

## 为什么纳入
这篇不是 peripheral T-cell atlas，而是 mucosal interface work。它的价值在于把 disease severity 和 airway epithelial-immune interaction 直接放在单细胞图谱里：算法问题从“PBMC 里哪类 T cells 改变”扩展为“组织界面上哪些 immune and epithelial states共同改变”。

## 数据与研究设计
- 物种/器官：`Homo sapiens`；upper/lower airway
- cohort：COVID-19 patients `n=19`，healthy controls `n=5`
- 采样：nasopharyngeal swabs；部分 critical patients 另有 protected specimen brush / bronchial lavage
- longitudinal aspect：`5` patients have longitudinal nasal samples
- 文中 nasopharyngeal comparison 记录 control samples `n=5`、moderate COVID samples `n=14`、critical COVID samples `n=13`
- 核心模态：airway scRNA-seq；不是 TCR/BCR paper

## 文章中的算法/分析流程
### 1. Airway cell-state map
- 论文在 epithelial 与 immune compartments 中进行 clustering、marker annotation 与 condition/severity comparison。
- 它的 atlas object 同时含 tissue interface cells 与 infiltrating immune cells，适合讨论“cell-type composition and state change are coupled to site of sampling”。

### 2. Interaction interpretation
- 作者关注 epithelial-immune cytokine/chemokine axes，尤其 severity-associated ligand/receptor patterns。
- 这一层是后验 interaction inference，不是新 causal cell-cell model；但它定义了很实际的 organ-interface graph task。

### 3. Longitudinal and site heterogeneity
- 同一研究里同时存在 moderate/critical severity、upper/lower airway 与 longitudinal nasal samples。
- 对算法来说，这天然要求层级 covariate modeling；若简单 merge 后做 case/control 差异，severity、site 与 time 会互相缠绕。

## 与 T 细胞—人群免疫力的关系
- 直接 T-cell specificity 不强：无 TCR；immune cells 与 epithelial cells 是共同对象。
- 对 method report 的价值是补足 mucosal immunity 场景：T-cell state modeling 若脱离 site-specific epithelial context，会丢失局部 inflammatory circuit。

## 数据可用性
- Raw controlled data：EGA `EGAS00001004481`
- Processed counts/metadata：paper reporting material points to Figshare DOI `10.6084/m9.figshare.12436517`
- Visualization/explorer：Magellan COVID-19 data explorer
- 本轮未定位到作者发布的独立 analysis-code repository
- 代码边界：
  - 输入：airway scRNA expression data、sample metadata、severity/site/time labels
  - 输出：airway cell-type/state map、severity comparisons、interaction summaries

## 算法贡献与不足
- 直接算法创新：**低到中**；主要是 scRNA atlas and interaction interpretation。
- 数据价值：**中高**；有 airway site and severity structure，processed exports可定位。
- 不足：
  - 没有 receptor sequence modality
  - raw human data受控
  - site、severity、longitudinal sampling 的 confounding 需要后续算法更严谨处理

## 对新算法开发的启发
1. **Site-aware disease atlas model**：jointly model time, severity and anatomical compartment。
2. **Immune-epithelial graph learning**：从 ligand-receptor tables 走向 condition-aware interaction representation。
3. **Mucosal-to-blood transfer**：量化 blood markers 对 airway immune pathology 的迁移误差。

## 可放入 method report 的表述
Chua et al. 提供了 airway immune-interface benchmark：现有单细胞分析能从 severity-stratified airway samples 推断 epithelial-immune interaction shifts，但 site、time 与 disease severity 的联合分解仍需要更系统的层级模型。

## 一句话结论
这篇适合放在 mucosal/tissue-aware immunity 小节，用来说明 severe infection algorithms 不能只围绕 PBMC cell states。
