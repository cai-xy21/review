# Algorithm Report 036

## Paper
COVID-19 severity correlates with airway epithelium-immune cell interactions identified by single-cell analysis

## 题录与数据
- 年份/期刊：2020, Nature Biotechnology
- DOI：https://doi.org/10.1038/s41587-020-0602-4
- Cohort：`19` COVID-19 patients and `5` healthy controls
- Public data：raw EGA `EGAS00001004481`；processed count/metadata Figshare DOI `10.6084/m9.figshare.12436517`

## 算法视角定位
这是 **severity- and site-aware airway atlas with interaction interpretation**。它不提出新的 T-cell algorithm，但它把局部 epithelial-immune context变成必须建模的变量。

## 输入、处理与输出
### 输入
- airway single-cell transcriptomes
- severity labels：moderate/critical/control
- sampling site labels：nasopharyngeal vs lower airway
- longitudinal nasal sample metadata for a subset

### 处理链
1. QC and clustering of epithelial and immune compartments
2. marker-based cell/state annotation
3. severity and site stratified cell-state comparison
4. chemokine/cytokine ligand-receptor interpretation
5. visualization and processed exports for exploration

### 输出
- airway epithelial/immune state map
- condition/site composition summaries
- inferred epithelial-immune interaction patterns
- candidate severity-associated pathways

## 详细算法贡献
### 1. Defining the airway interface task
The algorithmic object is not only immune cells but a paired epithelial-immune system under disease severity and anatomical site shift.

### 2. Heterogeneous sample structure
The same study combines control/moderate/critical cases, upper/lower airway samples and repeated samples from some patients. That data structure is useful for method development around confounding-aware atlas comparison.

### 3. Interaction analysis remains descriptive
The paper uses cell-state annotation and interaction interpretation to generate hypotheses. It does not identify causal communication edges from perturbations or learn transferable cell-cell graph representations.

## 对新算法贡献程度
- 任务定义：高
- 数据资源：中高
- 直接新模型：低
- tissue-interface motivation：高

## 可开发空间
1. **Hierarchical sample model** for severity, patient, site and time.
2. **Condition-aware cell-cell interaction graph** with uncertainty for ligand-receptor edges.
3. **Cross-compartment immune representation** that compares airway and blood without erasing tissue biology.

## 数据与代码评估
- 物种/器官：`Homo sapiens`；airway mucosa
- Raw accession：`EGAS00001004481`
- Processed export：Figshare DOI `10.6084/m9.figshare.12436517`
- Author code：no dedicated public analysis repo located in this pass
- 可复用性：**medium-high** for tissue interaction benchmarks；**medium-low** for T-cell-specific or repertoire methods

## 可纳入 method report 的一句话
Airway COVID atlases make severity a local epithelial-immune interaction problem, exposing the need for site-aware and longitudinally aware single-cell models.
