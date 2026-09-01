# Algorithm Report 004

## Paper
Cross-tissue immune cell analysis reveals tissue-specific features in humans

## 算法视角定位
这篇文章主要是跨组织免疫图谱分析工作，同时包含一个明确可复用的算法产物 `CellTypist`。它的价值不只是定义任务，还把免疫细胞注释从人工 marker curation 推进到可训练、可迁移的监督式分类框架。

## 核心算法贡献
- 主要贡献是**cross-tissue immune comparative analysis pipeline** + `CellTypist` 自动细胞类型注释工具。
- 它把多组织免疫细胞放入统一比较框架，系统分析 tissue-resident 与 circulating states 的差异。
- `CellTypist` 是基于 logistic regression classifier 的 scRNA-seq annotation 工具，可用 stochastic gradient descent 支持大规模训练，并通过 majority voting 在局部过聚类上提高标签稳定性。
- 它强调跨组织整合的目标不是“抹平差异”，而是“在共享坐标系里保留可解释的组织差异”。

## 关键方法学价值
- 为 tissue-aware integration 提供清晰问题设定。
- 为 shared lineage identification 与 local adaptation decomposition 提供真实场景。
- 使 immune manifold alignment 不再停留在抽象任务，而有明确健康数据背景可验证。
- 为评估 blood-to-tissue transfer、组织偏移建模和局部驻留状态识别提供基准资源。

## 相比既有工作的改进
早期研究多停留在单组织或单疾病背景；即便涉及多个组织，也常缺乏统一比较框架。该文改进之处在于：实现了匹配式跨组织比较，使组织适应性成为可系统研究、可建模、可迁移的对象，而不是只能做定性描述。

## 适合抽象出的计算任务
- tissue-aware integration
- blood-to-tissue reference transfer
- shared-vs-local program disentanglement
- cross-tissue T-cell manifold alignment
- donor-aware comparative embedding
- healthy tissue deviation scoring

## 数据/代码可用性
- DOI：https://doi.org/10.1126/science.abl5197
- 数据：12 名 deceased organ donors、16 个组织、>360,000 个 tissue/blood immune cells。
- 物种/组织：`Homo sapiens` blood 与 cross-tissue immune compartments。
- 正式 accession：EGA `EGAS00001005791`。
- 资源入口：CELLxGENE collection <https://cellxgene.cziscience.com/collections/62ef75e4-cbea-454e-a0ce-998ec40223d3>
- 代码：analysis repo <https://github.com/Teichlab/TabulaSapiens>；CellTypist repo <https://github.com/Teichlab/celltypist>
- 代码输入/输出：analysis repo 输入 Tabula Sapiens immune expression objects、donor/tissue labels 与 annotation；输出 cross-tissue immune state embeddings、subset comparison、B/T/NK tissue-feature analyses 与论文图表。CellTypist 输入 scRNA-seq matrix/`h5ad` 与 reference model；输出 predicted cell labels、probability/confidence matrix、majority-voted labels 和 annotation result files。
- 复用性：高。

## 对新算法开发的贡献程度
- 评级：**中等（直接算法创新）/高（任务定义与评测价值）**
- 原因：CellTypist 是可复用的监督式自动注释工具，属于明确算法产物；同时该文还提供 tissue-aware integration、state transfer 和 cross-tissue representation learning 的真实任务场景。它不是深度生成模型类的基础算法，但在 single-cell annotation workflow 中有实际方法贡献。

## 对我们方法论文的启发
- 需要 tissue-aware integration，而非仅 batch correction。
- 需要显式区分 tissue effect 与 donor effect。
- 适合发展 cross-tissue T-cell manifold alignment。
- 应允许“共享身份 + 局部状态”的双层表示，而不是强制单层聚类解释。
- 可把血液视为部分可观测代理，而非全身免疫真值。

## 方法局限对建模的提醒
- 组织样本不均衡会影响跨组织比较的稳定性。
- 某些所谓组织差异可能混入解离与处理流程偏差。
- 血液与组织之间存在明显 domain gap，算法不应假设完全可逆映射。

## 总结
这篇文章最重要的算法价值，是把“跨组织免疫状态比较”变成一个可建模、可验证的问题，而不是只停留在描述性生物学上。对方法研究来说，它是 tissue-aware immune modeling 的关键任务定义论文之一。
