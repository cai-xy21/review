# 004. Cross-tissue immune cell analysis reveals tissue-specific features in humans

## 基本信息
- 年份：2022
- 期刊：Science
- DOI：https://doi.org/10.1126/science.abl5197
- PMID：35549406
- 主题：跨组织免疫细胞图谱；组织适应性；免疫细胞状态比较
- 可信度：高

## 文章定位
这篇文章比通用 atlas 更聚焦免疫系统本身，核心问题是：同一免疫谱系在不同组织中如何被局部微环境重塑。它不是简单把不同组织中的免疫细胞合并展示，而是试图回答“哪些免疫状态是跨组织共享的，哪些是局部生态位驱动的”。对 T 细胞和整体人群免疫研究都很关键，因为很多所谓“免疫底盘差异”并不能只靠外周血完整观察。

## 数据与设计概览
- 研究对象：12 名 deceased organ donors 的健康/非病灶组织免疫细胞。
- 取样范围：16 个组织，论文摘要给出来自 tissue 与 blood 的 >360,000 个 immune cells，使循环与组织驻留免疫状态可被并置比较。
- 数据类型：以单细胞转录组为核心，重点关注免疫细胞谱系。
- 设计重点：在跨组织背景下建立免疫细胞统一比较框架，而不是把每个组织单独分析后再做松散对照。

## 亮点
1. 不只看 PBMC，而是明确比较多组织中的免疫细胞状态。
2. 揭示了共享免疫谱系与组织特异程序并存的结构。
3. 对淋巴细胞、髓系细胞、组织驻留免疫细胞的组织适应性给出了系统证据。
4. 强调“组织环境”不是噪声，而是免疫状态解释中的关键变量。
5. 为健康状态下 cross-tissue immune reference 建立了高质量对照背景。

## 核心贡献
- 证明跨组织背景下，免疫细胞状态具有强烈而系统性的组织印记。
- 显示同一免疫谱系在不同器官中既保留共享身份，也呈现显著的局部适应程序。
- 为“血液免疫表型能否代表全身免疫状态”提供了重要边界与限制条件。
- 为后续 tissue-aware annotation、组织迁移研究、局部免疫图谱比较和疾病偏移分析提供基础。
- 让免疫 atlas 从“列举细胞类型”进一步走向“解释组织生态位如何塑造状态差异”。

## 对免疫研究的直接价值
- 说明免疫研究不能默认外周血是全身免疫状态的完整代理。
- 为组织驻留 T 细胞、局部髓系细胞、黏膜免疫细胞等提供健康参照框架。
- 有助于区分炎症相关异常状态与正常组织适应状态。
- 对研究感染、肿瘤、自免和衰老中的局部免疫重塑特别有帮助。

## 与 T 细胞—人群免疫力的关系
T 细胞在不同组织中的激活、驻留、效应和调节状态差异，是理解个体免疫能力的重要组成部分。这篇文章的重要性在于：它说明了 T 细胞状态不能脱离组织语境解读。换句话说，同一类 T 细胞在血液、黏膜和实体器官中的表达程序可能遵循不同规则；因此做人群免疫差异分析时，必须区分组织效应与真正的个体免疫差异。

从方法论文写作角度，它可支持以下论述：
- tissue effect 是免疫状态建模中的主变量，而不是简单 batch；
- cross-tissue comparison 需要保留共享谱系，同时解析局部适应模块；
- 研究 T 细胞功能异质性时，单一外周血参考往往不够。

## 主要分析框架
- 对多组织免疫细胞进行统一单细胞质控、整合与聚类分析。
- 在共享嵌入空间中识别主要免疫谱系及其组织特异亚状态。
- 比较相同谱系在不同组织中的转录差异，分离共享身份与局部适应程序。
- 结合经典 marker、组织来源与表达模块对细胞状态进行解释。
- 将跨组织免疫比较组织成可复用参考框架，而不仅是单篇文章的描述性结果。

## 算法/分析贡献
- 主要贡献是**cross-tissue comparative single-cell analysis** 的分析框架。
- 方法学上强调：共享谱系识别、组织特异状态分解、跨组织差异比较。
- 直接算法贡献是开发并使用 `CellTypist`：一个面向 scRNA-seq cell-type annotation 的监督式机器学习工具，核心为基于 logistic regression、可用 stochastic gradient descent 优化的大规模分类器。它把人工 marker 注释的一部分工作转化为可训练、可迁移、可多数投票校正的 label-transfer/classification 问题。
- `CellTypist` 的典型输入是标准化后的 scRNA-seq 表达矩阵或 `h5ad` 对象，以及预训练或自定义训练的 reference model；输出是每个细胞的 predicted label、置信度/概率矩阵、majority-voting 后的标签和可视化/结果表。
- 它的重要价值在于定义了一个核心任务：如何在共享细胞谱系上分离 tissue effect、donor effect 和 technical effect。
- 因此它不是纯粹资源论文：既有 cross-tissue immune atlas，也有可复用的自动注释工具。对 tissue-aware integration、local-state transfer 和 immune representation learning 都提供了非常清晰的问题设定。

## 局限性
- 健康供者数量有限，难完全覆盖人群层面的自然变异。
- 不同组织的细胞回收效率和解离偏差会影响某些免疫群体的可见度。
- 转录状态并不等于功能状态，仍需结合空间、蛋白和功能实验解释。
- 某些组织中的稀有驻留细胞可能因采样深度不足而被低估。

## 对新算法开发的启发
1. 需要 tissue-aware integration，而不是只做 batch correction。
2. 需要把局部组织环境显式纳入 T-cell state model。
3. 适合开发跨组织 immune manifold 对齐算法。
4. 适合发展区分 shared identity 与 local adaptation 的 disentangling 模型。
5. 可用于构建血液到组织的 reference transfer 与偏移评估框架。

## 数据可用性
- 数据：公开存档，论文与期刊页面提供访问路径。
- 数据性质：人类跨组织 immune scRNA-seq；12 名 donor、16 个组织、>360,000 个 blood/tissue immune cells；适合 shared-vs-local immune program、B-cell maturation 与 T/NK tissue adaptation 分析。
- 物种/器官：Homo sapiens；blood 与多组织 immune compartment。
- 文章/项目公开入口：CELLxGENE collection <https://cellxgene.cziscience.com/collections/62ef75e4-cbea-454e-a0ce-998ec40223d3>
- accession 级别信息：论文 Data availability 给出 EGA `EGAS00001005791`
- 代码：论文 Data availability 给出公开分析仓库 <https://github.com/Teichlab/TabulaSapiens>；CellTypist 工具仓库 <https://github.com/Teichlab/celltypist>
- 代码输入/输出：分析仓库输入为 Tabula Sapiens immune-compartment 表达矩阵、组织/donor metadata 与注释；输出为 cross-tissue immune embedding、subset annotations、tissue-comparison figures 与 tissue-adaptation analyses。CellTypist 输入为 scRNA-seq matrix/`h5ad` 与 reference model，输出为 per-cell label、probability/confidence、majority-voted annotation 和结果表/图。
- 模型/流程结构：`Scanpy`/`scVI` 相关 cross-tissue comparative workflow + `CellTypist` logistic-regression classifier；前者服务 atlas 构建，后者是可复用的自动细胞类型注释模型。
- 复用价值：高，尤其适合 tissue-aware 免疫建模与健康参考对照。

## 影响因子/可信度综合判断
- 期刊层面：Science
- 生物学问题重要性：高
- 图谱与后续方法价值：高
- 综合判断：**高可信度、高方法启发价值**

## 一句话结论
这篇文章是“组织特异免疫状态”方向的关键锚点，对 T 细胞组织适应与人群免疫异质性建模都非常重要。
