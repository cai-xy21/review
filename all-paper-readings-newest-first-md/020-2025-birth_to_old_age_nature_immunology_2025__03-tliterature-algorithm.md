# Algorithm Report 001

## Paper
Integrating single-cell RNA and T cell/B cell receptor sequencing with mass cytometry reveals dynamic trajectories of human peripheral immune cells from birth to old age

## 算法视角定位
这篇论文更偏向**高质量多模态图谱构建 + 生命周期动态分析**，算法贡献主要体现在“整合式分析框架”和“年龄相关免疫状态量化”，而不一定是提出全新的通用基础算法。对我们来说，它的价值在于：定义了问题、组织了多模态数据、给出了适合进一步算法开发的任务结构。

## 数据模态
- scRNA-seq
- scTCR-seq
- scBCR-seq
- mass cytometry (CyTOF)
- bulk RNA-seq / flow cytometry / 功能验证

## 数据任务定义
- 物种/样本：`Homo sapiens` peripheral blood / PBMC，覆盖脐带血到 >90 岁
- 主问题：在 cohort-level lifecycle axis 上联合回答细胞组成、表达程序、互作网络和 adaptive receptor repertoire 如何变化
- 算法产物层次：
  - cell-level atlas：细胞类型与 subset 状态
  - subset-level dynamics：年龄相关 abundance、DEG 与 receptor diversity 轨迹
  - donor-level prediction：`siAge` immune-age regression

## 关键算法问题
1. 如何在跨年龄、跨个体的背景下构建统一免疫图谱
2. 如何连接细胞表达状态与 TCR/BCR 克隆信息
3. 如何识别沿生命周期变化的细胞状态轨迹
4. 如何从单细胞多模态数据中构建免疫年龄或状态评分

## 论文中的算法贡献
### 1) 多模态整合式分析框架
论文把 RNA、TCR/BCR、CyTOF 放进统一研究设计中，虽然这不一定是新的基础算法，但它是一种高价值的分析范式：
- RNA 负责状态空间
- TCR/BCR 负责克隆历史与扩增信息
- CyTOF 负责蛋白层验证与补充

**意义**：为后续开发“真正的联合模型”提供训练/验证场景。本文本身更接近 analysis-level integration，而非端到端 multimodal representation learning。

### 2) 生命周期动态统计
文章把 13 个年龄组放到连续生命周期背景下比较，不止做青年/老年二分类：
- 在 immune cell subset 层比较相对丰度与年龄模式
- 在基因层提取 age-associated DEGs 与表达轨迹
- 在 repertoire 层拟合年龄相关多样性变化
- 在 interaction 层比较年龄相关 cell-cell interaction rewiring

**方法学价值**：
- 把年龄视为连续生物学主轴
- 提供 lifecycle curve/trajectory problem，而不等同于真正有纵向追踪观测的 lineage transition model
- 为 age-aware trajectory inference 和 normative aging model 提供任务原型

### 3) TCR/BCR repertoire algorithm layer
- TCR/BCR 由 `Cell Ranger V(D)J` 标注；保留 productive/high-confidence receptor calls
- TCR clonotype 由 alpha/beta 配对定义，BCR clonotype 由 heavy/light pairing 定义；多链情况下取 dominant chain
- 克隆扩增离散为八个 expansion bins，并映射回 cell metadata
- repertoire diversity 使用 Shannon entropy、inverse Simpson 与 Chao1；年龄曲线用 LOESS/相关统计描述

**算法意义**：
- 把 transcriptomic subset 与 clonal expansion/diversity 连接起来
- 形成 T-cell population immunity 研究里常见的 `state annotation -> clonotype aggregation -> diversity/dynamics` 管线
- 但 clonotype 仍主要是后验统计特征，不是转录表示学习中的结构先验

### 4) siAge immune-age model
这是本文最明确的模型产物：
- 候选特征：各 cell subset 的 DEGs
- 基模型：random-forest regression
- 模型比较：25 个 cell-type-specific models + 1 个跨 25 subset 的 integrated model
- 特征筛选：10-fold cross-validation 与 random forest importance；最终 21 key genes
- 预测目标：donor/sample-level immune age

**算法意义**：
- 从描述性图谱迈向预测性建模
- 提供 donor-level immune age estimation 的范式
- 为后续构建 immune resilience / immune fitness 指标提供先例

## 不是它解决得很好的问题
1. **clone-aware trajectory 的深层联合建模** 可能仍然不足
   - 常见做法是把 clonotype 作为注释或分组统计，而非真正进入状态转移模型
2. **donor-aware hierarchical modeling** 可能不足
   - 单细胞研究常偏重 cell-level，个体层方差结构建模不充分
3. **跨模态联合表示学习** 可能仍是松耦合
   - 很多工作是先分别分析，再做结果对照，而非端到端联合嵌入
4. **跨年龄分布偏移与泛化问题** 不一定被系统处理
   - 不同年龄段细胞组成差异会影响整合与注释稳定性
5. **siAge 的模型边界** 仍较窄
   - `siAge` 是基于 PBMC scRNA 特征的 random forest regression，不直接吸收 CyTOF、TCR/BCR sequence graph、暴露史或 longitudinal observations

## 我们可借此提炼的算法机会
### A. Donor-aware multimodal representation learning
目标：从细胞层到个体层建立层级表示，分离年龄、个体、批次、技术平台、真实免疫状态等因素。

可做方向：
- hierarchical VAE / graph representation
- domain adversarial age-batch disentanglement
- donor-conditioned atlas mapping

### B. TCR-state joint model
目标：把克隆结构直接纳入状态空间，而不是后处理附着。

可做方向：
- clonotype graph + transcriptome graph 联合学习
- 同克隆跨状态迁移建模
- clone expansion 与 state plasticity 的联合估计

### C. Age-aware trajectory inference
目标：既保留经典伪时序思想，又显式建模年龄作为外部连续变量。

可做方向：
- chronological prior constrained trajectory
- age-conditioned transition kernels
- cross-sectional to pseudo-longitudinal trajectory reconstruction

### D. Immune-age / immune-baseline scoring
目标：比单一年龄预测更进一步，估计免疫底盘、免疫韧性、异常偏移。

可做方向：
- uncertainty-aware donor score
- multimodal calibration
- normative modeling for population immunity

## 对新算法贡献程度评估
- **定义任务价值**：很高
- **提供训练数据潜力**：高
- **直接提出全新算法的程度**：中
- **对后续算法研究的启发程度**：很高

综合评估：**高价值应用型/资源型方法论文；中等强度直接算法创新，极高启发价值**

## 数据可用性评估
- DOI：https://doi.org/10.1038/s41590-024-02059-6（已联网核对）
- Web portal：<https://pu-lab.sjtu.edu.cn/shiny/lifespan>
- PubMed/PMC：PMID 39881000；PMCID PMC11785523
- 临床注册：NCT05206643
- 可确认数据规模：研究总计 309 名参与者；主 cohort 220 名健康个体；scRNA/scTCR/scBCR 61 人、538,266 个高质量细胞；CyTOF 70 人、10,887,899 个单细胞事件；bulk RNA 验证 34 人；另有 55 人用于其他验证；外部 siAge 验证 89 人
- 物种/器官：人；外周血/PBMC，覆盖脐带血到高龄队列
- Raw human data accession：Genome Sequence Archive for Human `HRA009014`
- Processed accessions：
  - Synapse `syn61597322`：processed scRNA-seq
  - Synapse `syn61609778`：CyTOF
  - Synapse `syn61609846`：annotated reference data
- 文章提供的公开获取链接：门户、preprint、GSA-Human accession、Synapse accessions
- 代码仓库：<https://github.com/shanzha9/siAgeModel>
- 代码入口：
  - `1.predicteCellType.R`：Seurat reference/query label transfer
  - `2.calculateCP10K.R`：从带 cell-type label 的 Seurat object 形成按样本和 cell type 聚合的 CP10K/CPM 特征
  - `3.predictsiAGE.R`：加载 `all_selectedModel.rds` 对 `cp10k.csv` 预测并写出 `siAGE_prediction.csv`
- 模型结构与意义：`Seurat anchor transfer -> cell-type-aware feature aggregation -> randomForest regression`；把 lifecycle scRNA atlas 投影成 donor-level immune-age readout
- 适合方法复现/二次开发程度：**高于此前初稿判断；processed data 与模型代码可定位，但 raw human data 访问和前处理严格对齐仍是复现风险**

## 适合纳入我们 method report 的表述
可将这篇归为：
- **Population-scale multimodal immune atlas**
- **Age-aware immune trajectory analysis**
- **A stepping stone toward donor-aware and clonotype-aware models**

## 建议引用句式
- 现有工作已经开始通过整合 scRNA-seq、TCR/BCR 测序与 CyTOF，在生命周期尺度描绘人类外周免疫系统动态重塑，但大多仍以描述性图谱和松耦合分析流程为主。
- 下一步值得发展的，是显式联合 donor、age、clonotype 与 multimodal phenotype 的统一模型。

## 最终判断
这篇文献对“已有算法做了什么”的贡献，主要不是发明了一个广泛替代性的新算法，而是把问题设置到了一个足够真实、足够复杂、足够值得做统一建模的位置上。对你们后续方法论文而言，它非常适合作为“算法机会从哪里冒出来”的出发点。
