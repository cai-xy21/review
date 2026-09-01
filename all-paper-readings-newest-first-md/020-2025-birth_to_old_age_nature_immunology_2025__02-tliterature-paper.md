# 001. Integrating single-cell RNA and T cell/B cell receptor sequencing with mass cytometry reveals dynamic trajectories of human peripheral immune cells from birth to old age

## 基本信息
- 年份：2025
- 期刊：Nature Immunology
- DOI：https://doi.org/10.1038/s41590-024-02059-6
- PMID：39881000
- 主题：人群外周免疫图谱；生命周期免疫变化；单细胞多模态整合；T/B 细胞受体谱；T 细胞状态动态

## 为什么重要
这篇文章直接回答了“不同年龄阶段、不同个体为什么免疫底盘不同”的核心问题，是“单细胞组学 × 人群免疫力 × T 细胞相关性”方向非常强的基础图谱型工作。它把 scRNA-seq、TCR/BCR 测序和 CyTOF 联合到同一研究框架中，兼顾了转录状态、克隆谱系与蛋白表型，因而特别适合作为方法综述的锚点文献。

## 数据与研究设计
- 样本对象：Shanghai Pudong Cohort 中 220 名健康供者，13 个年龄组，覆盖脐带血/新生儿到 >90 岁；全文研究总计涉及 309 名参与者
- 样本类型：人类 peripheral blood / PBMC；不是跨器官 atlas，而是生命周期外周免疫基线资源
- scRNA/scTCR/scBCR 主队列：61 名供者，过滤后 538,266 个高质量单细胞
- CyTOF 队列：70 名供者，质控后 10,887,899 个单细胞事件
- 验证层：34 名供者 bulk RNA-seq；55 名供者参与额外 flow cytometry/功能验证；siAge 外部验证还纳入 89 名个体
- 核心模态：10x scRNA-seq、10x scTCR-seq、10x scBCR-seq、mass cytometry
- 研究目标：构建人类生命周期外周免疫细胞动态图谱，解析组成、转录程序、细胞互作、蛋白表型与适应性受体谱如何沿年龄轴重塑

## 核心亮点
1. **生命周期尺度的人群免疫单细胞图谱**：不是只比较青年与老年，而是试图连续刻画从出生到老年的外周免疫细胞动态。
2. **多模态联合**：不仅看 RNA，还把 TCR/BCR 克隆信息和 CyTOF 蛋白层信息串起来，提高对 T 细胞状态和免疫重塑的解释力。
3. **动态轨迹视角**：重点不只是“哪些细胞变多/变少”，而是关注免疫细胞如何沿年龄轴发生状态迁移。
4. **方法学可扩展性强**：这类设计天然适合后续发展 donor-aware、age-aware、multimodal 的新算法。

## 核心贡献
- 提供了高价值的人群免疫基础参考图谱，可作为年龄分层、免疫基线、健康状态对照的重要参考。
- 揭示了不同免疫细胞群在生命周期中的重塑趋势，尤其对 T 细胞群体状态变化具有基础意义。
- 通过受体组信息与表达状态结合，帮助区分“数量变化”“克隆扩增”和“功能状态变化”之间的关系。
- 为解释人群免疫异质性提供了更细粒度的细胞层证据。

## 与 T 细胞—人群免疫力的关系
- T 细胞是适应性免疫核心，年龄相关的 T 细胞初始态、记忆态、效应态、耗竭样状态变化，直接决定个体免疫底盘。
- TCR 信息使研究者能够把 T 细胞状态变化与克隆扩增、抗原经验间接联系起来。
- 如果后续关注“为什么不同人基础免疫能力不同”，这篇文章提供了生命周期主轴上的基线变化框架。

## 文章中的算法/分析流程
### 1. scRNA 图谱构建与年龄动态统计
- 论文先把 PBMC scRNA-seq 建成统一细胞状态图谱，再以 cell subset 为单位比较年龄相关组成和表达变化。
- 年龄不是只做 young/old 二分类。作者使用 13 个年龄组，并对多类连续年龄变化做拟合与峰形比较，因此它更接近 lifecycle dynamic atlas，而不是静态差异分析。
- 文中把差异表达、年龄相关表达轨迹、蛋白层验证和 cell-cell interaction 重塑串起来，形成“组成变化 + 状态变化 + 互作变化”的解释链。

### 2. TCR/BCR repertoire 与细胞状态耦合
- scTCR/scBCR 经 `Cell Ranger V(D)J` 处理后再过滤 productive/high-confidence receptor calls。
- TCR clonotype 定义使用 TCR alpha/beta 链组合；若单细胞出现多条链，使用表达量最高链作为 dominant chain。
- BCR clonotype 类似地由 heavy chain 与 light chain 配对定义。
- 克隆扩增被分成 `unique`, `2-5`, `6-10`, `11-20`, `21-30`, `31-50`, `51-100`, `>100` 八档，并回填到 Seurat metadata 做 T-cell subset 和年龄组比较。
- repertoire 多样性用 Shannon entropy、inverse Simpson 和 Chao1 指标评估，年龄趋势用 LOESS 与相关统计刻画。

### 3. 跨模态验证但不是端到端联合嵌入
- RNA 是主要状态空间；CyTOF 用于蛋白表型和年龄趋势交叉验证；bulk RNA 与 flow cytometry 支持外部确认。
- 这是一种分析层联动的 multimodal workflow，而不是类似 totalVI/WNN 那种明确提出新的联合 latent model。
- 因而它对算法研究最重要的价值是提供真实的多模态 population task，而不是提供一个可直接替代现有整合方法的新通用模型。

### 4. siAge 免疫年龄模型
- 作者把 cell-subset-specific DEGs 作为候选特征，先训练 25 个 cell-type-specific random-forest regression models 和 1 个跨 25 个 cell subset 的 integrated model。
- 特征选择使用 10-fold cross-validation 与 random forest 变量重要度排序，最终选出 21 个关键基因构建 `siAge`。
- `siAge` 的建模对象从 cell-level atlas 转到 donor-level immune age：输入是按模型要求整理的人类 PBMC scRNA 特征，输出是样本/个体层 immune age prediction。
- 论文使用外部 PBMC scRNA 数据测试泛化，并用免疫异常个体检验“预测免疫年龄偏离生理年龄”能否提示失衡状态。

## 对算法工作的启发
这篇文章最有价值的不只是发现本身，而是暴露了几个很明显的方法空间：
1. **年龄感知的单细胞表示学习**：把 donor、age、cell state、clone 统一放进一个表示空间。
2. **克隆型—状态联合建模**：当前很多工作只是把 TCR 当附加注释，还没有充分建模“同克隆跨状态迁移”。
3. **跨模态个体层建模**：从 cell-level 走向 donor-level，预测个体免疫年龄/免疫韧性。
4. **动态图谱对齐**：不是简单 batch correction，而是 age-structured manifold alignment。
5. **不确定性量化**：对稀有亚群、跨年龄映射、跨平台转移注释给出置信度会很关键。

## 可作为我们 method 报告里的位置
建议放在“人群基础图谱”与“多模态生命周期免疫重塑”两节开头，作为全局框架性文献引用。

## 数据可用性
- 配套网页入口：<https://pu-lab.sjtu.edu.cn/shiny/lifespan>
- PubMed/PMC 已确认文章公开入口：PMID 39881000；PMCID PMC11785523
- DOI 已联网核对正确：https://doi.org/10.1038/s41590-024-02059-6
- 临床注册号：NCT05206643
- 当前可明确提取的数据性质：研究总计纳入 309 名参与者；其中主 lifespan cohort 为 220 名健康志愿者，覆盖 0 岁到 >90 岁；scRNA/scTCR/scBCR 队列 61 人、过滤后 538,266 个高质量细胞；CyTOF 队列 70 人、QC 后 10,887,899 个单细胞事件；bulk RNA 验证队列 34 人；另有 55 人用于其他验证实验；外部 siAge 验证 89 人（33 健康，56 免疫异常）
- 样本类型：人类外周血/PBMC，覆盖脐带血、儿童、成人到高龄人群
- 物种/器官：Homo sapiens；peripheral blood / PBMC
- 模态：scRNA-seq、scTCR-seq、scBCR-seq、CyTOF、bulk RNA-seq、flow cytometry、部分功能实验
- 文章明确提供的公开访问链接：门户 <https://pu-lab.sjtu.edu.cn/shiny/lifespan>；预印本页面 <https://www.biorxiv.org/content/10.1101/2022.07.11.498621v2>
- 原始数据 accession：Genome Sequence Archive for Human `HRA009014`；论文说明 raw data 通过该 accession 获取
- 处理后数据 accession：
  - Synapse `syn61597322`：processed scRNA-seq data
  - Synapse `syn61609778`：CyTOF data
  - Synapse `syn61609846`：annotated reference data，siAge cell-type transfer 代码也依赖该 reference
- 代码链接：<https://github.com/shanzha9/siAgeModel>
- 代码输入：
  - `1.predicteCellType.R` 输入已预处理的 query Seurat object `scRNA-seqProcessedObject.rds` 与 labelled reference `scRNA-seqProcessedLabelledObject.rds`
  - `2.calculateCP10K.R` 输入带预测细胞类型标签的 `predictedSeuratObject.rds`
  - `3.predictsiAGE.R` 输入 `cp10k.csv` 与训练好的 random forest `all_selectedModel.rds`
- 代码输出：
  - 细胞类型转移后的 Seurat object 和 marker violin plot
  - cell-type/sample 聚合后的 CP10K/CPM 特征矩阵
  - `siAGE_prediction.csv`，给出样本层 immune-age prediction
- 模型结构与意义：Seurat anchor-based label transfer + CP10K 特征整理 + random-forest regression；它把生命周期 atlas 压缩为可迁移的 donor-level immune-age score，是本文最明确的算法产物
- 评价：**processed data、raw accession、门户和 siAge 代码均已给出；复现门槛主要在 raw human data 访问、训练前处理一致性和代码工程化程度**

## 可信度评估
- 期刊层面：Nature Immunology，期刊影响力高
- 研究层面：问题重要、模态丰富、设计完整
- 风险点：raw human data 不是无门槛直接下载；`siAgeModel` 代码提供了推理链路，但复现仍依赖 reference 与一致的 PBMC scRNA 前处理
- 综合可信度：**高**

## 对我们后续工作的具体启示
- 如果我们的文章核心是“已有算法综述 + 新算法机会”，这篇应作为图谱型基准论文。
- 可以围绕它提出：
  - donor-aware multimodal immune atlas model
  - TCR-state joint trajectory model
  - immune-age score with uncertainty
  - population immunity baseline decomposition
- 本文还适合作为“已有算法不足”的例子：它已有强 cohort、受体谱和预测模型，但跨模态 latent representation、clone-aware state transition 与跨队列 uncertainty calibration 仍未被统一解决。

## 一句话结论
这是一篇非常适合做总领性引用的高价值基础图谱论文：它把人类生命周期外周免疫系统变化放在单细胞多模态框架下统一刻画，为 T 细胞相关的人群免疫差异研究提供了强基线。
