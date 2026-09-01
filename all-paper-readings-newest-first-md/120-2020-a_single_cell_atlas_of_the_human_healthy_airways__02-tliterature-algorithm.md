# Algorithm Report 044

## Paper
A Single-Cell Atlas of the Human Healthy Airways

## 算法视角定位
`044` 是健康呼吸道 reference atlas，算法增量主要在**沿 anatomy axis 的 cohort atlas construction**，不是 T-cell 新算法。它适合支撑 method report 中的一个边界观点：组织 immune modeling 不能把 anatomical site 和 sampling method 简化成普通 batch covariate。

该文主体是 airway epithelium；immune cells 只占少数。因此它与 T cells 的连接偏“健康组织基线”和“mucosal sampling/domain shift”，不是受体组或 T-cell state discovery。

## 题录与数据
- 年份：2020
- 期刊：American Journal of Respiratory and Critical Care Medicine
- DOI：https://doi.org/10.1164/rccm.201911-2199OC
- PMID：32726565
- 物种/器官：`Homo sapiens`；nasal and tracheobronchial airways from nose to 12th airway division
- 主模态：10x 3' scRNA-seq
- 样本规模：10 healthy living volunteers；35 airway samples；77,969 cells
- 细胞组成：89.1% epithelial、6.2% immune、4.7% stromal
- processed GEO：`GSE143868`
- BioProject：`PRJNA601924`
- raw EGA study：`EGAS00001004082`
- raw EGA dataset：`EGAD00001005714`

## 数据任务定义
1. 在健康人 living airway sampling 中建立 proximal-distal anatomy reference。
2. 比较同类 epithelial cell types 在 nose 与 tracheobronchial airway 的 transcriptional domain shift。
3. 描述 rare epithelial states 与 sampling-site-specific proportions。
4. 为 disease airway datasets 的 reference mapping 提供 healthy baseline。

## 关键算法问题
1. airway location、brush/biopsy method 与 donor effect 如何区分。
2. anatomy-driven biological difference 与 technical batch correction 如何避免互相抵消。
3. 多区域 atlas 中同一 cell type 的 stable core program 与 site-specific program 如何分离。
4. 少量 immune cells 在 epithelial-dominated atlas 中应如何用于 reference，而不被过度解释成专门 immune atlas。

## 详细算法贡献
### 1. Anatomy-aware single-cell reference design
- 论文不是只取 lung tissue 一个点，而是沿 nose、trachea/carina、intermediate bronchi、distal brushings 建 reference。
- GEO 记录显示 35 brushings/biopsies；EGA 进一步区分 forceps 与 brush biopsy cells。
- 这种结构天然形成 domain adaptation benchmark：同一 organ system 中 location 与 sampling protocol 都会改变 observed manifold。

### 2. 同类细胞的 region-specific DE
- 作者比较鼻腔与 tracheobronchial airway 中 suprabasal、secretory 和 multiciliated cells 的 region-specific expression。
- 算法意义是 atlas 不能只有 cell-type label，还应保留 `cell type x anatomy` conditional signatures。

### 3. rare-state refinement
- 文中改进了 ionocyte、pulmonary neuroendocrine、brush/NREP-positive populations 和 KRT13-associated hillock/squamous-like states 的描述。
- 对算法开发，这属于 rare-state discovery 与 anatomy-conditioned reference annotation 的压力测试。

### 4. 对 immune modeling 的间接贡献
- healthy airway immune compartment 只占 6.2%，但它说明 mucosal immune cells 与 epithelial context 同时受 anatomical site 影响。
- 后续感染或 airway T-cell studies 如果直接把鼻腔、气管、支气管 samples 混合整合，容易把真实 tissue adaptation 当 batch 去掉。

## 代码专项
- 本轮未在论文公开入口、GEO 与 EGA 记录中定位到作者专用 analysis repository。
- 文章公开可复用对象主要是：
  - 输入：10x count matrices、35 sample metadata、location/sampling-method annotations
  - 输出：healthy airway cell atlas、cell-type annotation、anatomy-conditioned DE signatures 与 rare epithelial state descriptions
- 因此它应在报告中标成“数据资源/analysis benchmark”，不要写成单独模型实现。

## 对新算法贡献程度
- 直接算法创新：**低**
- healthy anatomy reference 价值：**高**
- 对 mucosal domain-shift 建模启发：**中高**
- 对 T-cell 专门算法启发：**中等**
- 综合判断：**P2 atlas resource；对 site-aware integration 有方法学价值**

## 数据可用性评估
- DOI：https://doi.org/10.1164/rccm.201911-2199OC
- GEO：https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE143868
- GEO accession：`GSE143868`
- GEO processed supplement：`GSE143868_RAW.tar` 提供 processed supplementary matrices；GEO 记录注明 raw reads 不在 GEO
- EGA study：https://ega-archive.org/studies/EGAS00001004082
- EGA study accession：`EGAS00001004082`
- EGA dataset：https://ega-archive.org/datasets/EGAD00001005714
- EGA dataset accession：`EGAD00001005714`
- EGA 数据性质：10 healthy volunteers、35 samples、77,969 10x profiles；forceps cells 46,791、brush biopsy cells 31,178
- 可复用性：processed GEO 可直接复用；raw human sequencing 需 EGA controlled access

## 新算法空间
1. **Site-aware integration**
   - 在保留 anatomy-specific biology 的前提下校正 sampling and donor effects。
2. **Healthy-to-disease airway mapping**
   - 输出 cell-type label、site prediction 和 out-of-reference disease state uncertainty。
3. **Multicompartment mucosal modeling**
   - 将 epithelial barrier states 与稀疏 immune compartment 的 state changes 联合表示。
4. **Sampling-method robustness**
   - 对 brush versus biopsy 的 cell recovery bias 做 compositional calibration。

## 最终判断
`044` 的方法学价值来自健康 anatomy reference 和采样/domain-shift 问题。它不应被写成 T-cell 关键算法，但应被保留为 airway tissue studies 的 baseline 与 site-aware integration 例子。
