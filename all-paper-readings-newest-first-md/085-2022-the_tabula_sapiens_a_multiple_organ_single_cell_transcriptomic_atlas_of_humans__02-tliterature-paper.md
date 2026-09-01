# 002. The Tabula Sapiens: A multiple-organ, single-cell transcriptomic atlas of humans

## 基本信息
- 年份：2022
- 期刊：Science
- DOI：https://doi.org/10.1126/science.abl4896
- 主题：多器官人类单细胞图谱；健康基线 atlas；跨组织免疫比较
- 可信度：高

## 文章定位
Tabula Sapiens 是 Human Cell Atlas 方向最有代表性的健康人多器官单细胞参考资源之一。它的核心价值不在于某个单点生物学发现，而在于用统一采样、统一处理和统一注释框架，建立“健康人体多组织细胞状态坐标系”。虽然论文不是专门做 T 细胞或免疫学机制解析，但对后续所有需要健康 baseline 的免疫研究、疾病偏移分析和 reference mapping 都具有基础设施意义。

## 数据与设计概览
- 研究对象：Science 2022 原文构建自 15 名正常成人供者，其中多个组织来自同一供者；项目后续 v2 资源扩展到 24 名正常人供者、28 个器官和超过 110 万个细胞。
- 取样范围：原文覆盖 24 个组织/器官，约 483,152 个细胞；后续 portal/AWS v2 是更大规模的扩展版本，写作时应区分“论文原始 atlas”和“当前资源版本”。
- 测序策略：同时使用 10x droplet scRNA-seq 和 Smart-seq2；部分 donor/组织提供 BraCeR、TraCeR 等免疫受体派生结果，因此可支持有限的 T/B clonotype 跨组织分析。
- 设计重点：尽量在同一供者内完成多组织采样和统一处理，降低跨中心拼盘式 atlas 中常见的批次、供者和处理流程混杂。

## 亮点
1. 不是单器官 atlas 的简单拼接，而是在同一项目框架下构建跨器官、跨供者、跨细胞谱系的统一参考。
2. 同时覆盖上皮、基质、内皮、免疫等主要细胞大类，使免疫细胞能够被放回其真实组织生态位中理解。
3. 明确区分“跨组织共享的细胞身份”与“被局部微环境重塑的组织适应状态”。
4. 不仅可用于细胞类型 catalog，还可用于研究细胞状态连续谱、组织特异基因程序和供者间差异。
5. 资源化程度高，后续被广泛用于健康 reference、label transfer、cell state comparison 和算法预训练场景。

## 核心贡献
- 提供了健康人跨器官单细胞参考图谱，为疾病、衰老、感染、肿瘤和自免研究提供统一基线。
- 证明同一细胞谱系在不同组织中会呈现稳定但可塑的转录适应性，提示“细胞身份”与“组织状态”需要分层理解。
- 展示了多种细胞类型在器官间共享核心表达程序，同时保留明显的组织特异模块。
- 为细胞类型注释、跨组织 label transfer、reference mapping 和新 atlas 的外部对照提供公共底座。
- 为后续从“器官局部图谱”走向“全身系统图谱”的研究路线提供了范式。

## 对免疫研究的直接价值
- 给免疫细胞提供了跨组织健康参照，不再局限于 PBMC 或单一器官切片式观察。
- 能帮助判断某个免疫状态究竟是全身性免疫底盘的一部分，还是局部组织生态位诱导的状态。
- 使研究者可以把循环免疫细胞、组织驻留免疫细胞和器官相关基质/上皮细胞放在同一背景中解释。
- 对分析炎症、感染、衰老时“正常组织适应”与“病理偏移”之间的边界尤其重要。

## 与 T 细胞—人群免疫力的关系
这篇文章对 T 细胞研究的价值，不是提供最细的 T 细胞功能分群，而是提供一个判断背景的坐标系：T 细胞状态中哪些差异主要来自组织微环境，哪些差异更可能反映个体免疫底盘、年龄、生理状态或采样来源。对后续做人群免疫研究、疾病偏移建模和跨队列比较，这个区分非常关键。

从方法论文写作角度，它还能支撑这样一类论述：
- 仅用外周血并不能代表全身免疫状态；
- 同一 T 细胞谱系在不同组织中的表达程序存在稳定偏移；
- 健康 reference 应该显式建模 tissue effect，而不是把它全部当成 batch effect 消除。

## 主要分析框架
- 统一质控、细胞过滤、去除低质量细胞与潜在双细胞。
- 在跨供者、跨组织条件下构建整合表达空间并进行聚类与层级注释。
- 结合经典 marker、组织来源和全局嵌入结果做细胞身份判定。
- 在同一参考框架中比较不同组织中的共享细胞谱系与组织特异状态。
- 通过门户化资源输出，使 atlas 不只是论文结果，而是可查询、可映射、可复用的数据资产。

## 算法/分析贡献
- 贡献重点不在提出一个全新的基础模型，而在建立**大规模健康人多器官 atlas 的标准分析范式**。
- 核心方法价值包括：标准化预处理、跨器官整合、层级化注释、组织效应解释和可复用 reference 构建。
- 论文的重要方法学贡献是把“多组织可比较性”本身变成一个一等公民问题，而不是把不同组织数据简单拼接后直接聚类。
- 对免疫算法而言，原文还给出两个容易被忽略的任务：一是同一供者不同组织之间的 T 细胞克隆分布比较，二是 B 细胞体细胞突变/组织特异受体谱分析。这些分析不是新算法，但把 receptor repertoire 引入多器官健康 reference，是后续 clone-aware cross-tissue modeling 的直接前身。
- 对后续算法研究的最大意义是：提供了可用于 reference mapping、cross-tissue transfer、cell embedding、representation learning 和健康偏移评分的高质量训练底座。

## 局限性
- 健康供者数量相对有限，难以完整覆盖人群层面的年龄、性别、种族和生活方式差异。
- 并非所有器官都达到同样的细胞覆盖深度，稀有细胞与脆弱细胞类型可能仍被低估。
- 单细胞解离和组织处理流程会影响某些细胞状态，特别是对易受应激影响的免疫细胞。
- 论文主体以转录组为核心，功能状态、空间位置和克隆信息仍需结合其他模态补充。

## 对新算法开发的启发
1. 可发展跨组织、跨供者的 donor-aware reference mapping。
2. 可探索 tissue-aware T-cell state transfer learning，而不是把不同组织 T 细胞强行映射到单一状态轴上。
3. 可构建“健康基线偏离度”模型，用于量化疾病、年龄或干预导致的免疫偏移。
4. 可发展 disentangling 模型，显式分离 cell identity、tissue context、donor effect 与 technical effect。
5. 可将 Tabula Sapiens 作为预训练参考，再对特定疾病队列进行少样本适配。

## 数据可用性
- DOI 已联网核对正确：https://doi.org/10.1126/science.abl4896
- 数据：公开可获取，可通过 Tabula Sapiens portal、CZ CELLxGENE、figshare 项目页与 GitHub 资源仓库访问。
- 数据性质：Science 2022 原文为健康人多器官单细胞参考图谱，约 483,152 个细胞、24 个组织/器官、15 名正常供者；Tabula Sapiens 当前 v2/AWS 资源扩展为 24 名正常人供者、28 个器官、超过 110 万个细胞。
- 物种/器官：Homo sapiens；原文 24 个组织/器官，当前扩展资源为 28 个器官/组织的 cross-tissue atlas。
- 文章/项目公开入口：Portal <https://tabula-sapiens.sf.czbiohub.org/>；完整 atlas 的 CZ CELLxGENE 入口 <https://cellxgene.cziscience.com/e/53d208b0-2cfd-4366-9866-c3c6114081bc.cxg/>；immune compartment 入口 <https://cellxgene.cziscience.com/e/c5d88abe-f23a-45fa-a534-788985e93dad.cxg/>；collection 页面 <https://cellxgene.cziscience.com/collections/e5f58829-1a66-40b5-a624-9046778e74f5>；processed data figshare <https://figshare.com/projects/Tabula_Sapiens/100973>；GitHub <https://github.com/czbiohub-sf/tabula-sapiens>
- 原始数据获取：GitHub README 明确给出公开 S3 bucket `czb-tabula-sapiens`，并列出版本目录 `TabulaSapiens_v1_Science2022` 与 `TabulaSapiens_v2`；README 同时说明原始数据包含 10x FASTQ、Smart-seq2 数据及 immune repertoire 结果（BraCeR、TraCeR 输出）
- accession 级别信息：论文 Data availability 明确给出全部 raw sequencing data 已存于 EGA `EGAS00001005791`
- 代码/资源：存在 consortium GitHub 公开分析资源，包含 paper1、paper2、dissociation_affected_genes 等目录
- 代码输入/输出：典型输入为各组织单细胞表达矩阵、donor 与 tissue 标签，以及部分 10x / Smart-seq2 / immune repertoire 派生结果；输出为整合后的 reference、细胞注释、分仓 compartment atlas、可视化门户与论文分析结果
- 模型/流程结构：以标准单细胞预处理、跨组织整合、聚类与层级注释为主，而非单一新模型
- 复用价值：高，尤其适合作为健康参考、外部注释基准与方法评测底座

## 影响因子/可信度综合判断
- 期刊层面：Science，顶级综合期刊
- 数据资源价值：极高
- 方法透明度：较高
- 综合判断：**高可信度、高复用价值 atlas 论文**

## 一句话结论
如果你的文章要讨论“已有算法依赖什么样的高质量人类免疫参考”，Tabula Sapiens 是必须纳入的方法学背景资源。
