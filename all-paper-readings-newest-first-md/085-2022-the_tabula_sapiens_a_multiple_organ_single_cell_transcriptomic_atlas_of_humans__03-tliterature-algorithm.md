# Algorithm Report 002

## Paper
The Tabula Sapiens: A multiple-organ, single-cell transcriptomic atlas of humans

## 算法视角定位
这篇论文本质上是高价值 atlas/resource paper，而不是以方法创新为主的算法论文。它的算法价值主要体现在：把多器官、多供者单细胞数据放进统一处理、统一整合、统一注释、统一发布的流程中，构建出可直接作为 reference backbone 的人体健康图谱。

## 核心算法贡献
- 没有提出一个全新的基础模型或全新优化目标。
- 主要贡献是**跨器官整合、层级注释、统一参考构建与资源化发布工作流**。
- 它把“不同组织数据如何在同一标准下可比较”这个问题系统化处理，为 reference mapping、cross-tissue transfer、cell embedding 和 representation learning 提供了高质量训练底座。
- 它强调的不只是 batch correction，而是如何在保留组织生物学差异的同时建立共享坐标系。
- 对 T/B 细胞算法特别有用的增量，是把 10x/Smart-seq2 表达图谱与 BraCeR/TraCeR 免疫受体结果放在同一多器官 reference 中，使“同一 donor 中克隆是否跨组织共享、组织是否塑造同一谱系的表达状态”成为可计算问题。

## 关键方法学价值
- 统一质控与预处理，降低不同组织和不同供者之间的技术噪声差异。
- 跨组织整合后进行聚类与层级化注释，形成从粗粒度大类到细粒度状态的参考结构。
- 在共享嵌入空间中同时解释 cell identity、tissue context 和 donor variation。
- 通过门户化发布提升复现性与可移植性，使结果可直接用于外部数据映射与注释。

## 相比既有工作的改进
与早期单组织 atlas 相比，这篇文章真正推进的是“统一比较框架”而不是单点发现；与简单的数据拼盘式整合相比，它更强调成体系的标准化流程和跨组织可比性。因此它把跨组织共享细胞状态与组织特异状态放到同一张图谱里，适合作为后续方法评测基准。

## 适合抽象出的计算任务
- healthy reference mapping
- cross-tissue label transfer
- tissue-aware cell state alignment
- donor-aware embedding learning
- healthy baseline deviation scoring
- disentanglement of tissue effect vs donor effect vs technical effect

## 数据/代码可用性
- DOI：https://doi.org/10.1126/science.abl4896（已联网核对）
- 数据：Tabula Sapiens portal、CZ CELLxGENE、figshare 和 GitHub 公开可获取。
- 当前可明确核实的数据规模：Science 2022 原文约 483,152 个细胞、24 个组织/器官、15 名正常供者；当前 v2/AWS 资源扩展为 24 名正常人供者、28 个器官、超过 1.1 million cells。
- 物种/组织：Homo sapiens；多器官 cross-tissue atlas。原文和当前资源版本规模不同，引用时需分开表述。
- 公开入口：portal <https://tabula-sapiens.sf.czbiohub.org/>；完整 atlas 的 CZ CELLxGENE 入口 <https://cellxgene.cziscience.com/e/53d208b0-2cfd-4366-9866-c3c6114081bc.cxg/>；immune compartment <https://cellxgene.cziscience.com/e/c5d88abe-f23a-45fa-a534-788985e93dad.cxg/>；collection <https://cellxgene.cziscience.com/collections/e5f58829-1a66-40b5-a624-9046778e74f5>；processed data figshare <https://figshare.com/projects/Tabula_Sapiens/100973>
- 原始数据获取：GitHub README 明确给出 S3 bucket `czb-tabula-sapiens`，可见版本目录 `TabulaSapiens_v1_Science2022` 与 `TabulaSapiens_v2`
- 模态/文件类型：10x、Smart-seq2，以及 immune repertoire 结果（BraCeR、TraCeR 输出）
- 独立代码仓库：<https://github.com/czbiohub-sf/tabula-sapiens>
- 仓库输入：原始或派生的各组织单细胞表达矩阵、donor/tissue 元数据、10x/Smart-seq2 处理结果、BraCeR/TraCeR repertoire 结果
- 仓库输出：paper1/paper2 分析结果、figures、processed atlas、cellxgene/portal 浏览资源、dissociation 相关分析结果，以及支持跨组织免疫克隆解释的派生文件
- 仓库结构与意义：以 analysis-resource repo 为主，而不是一个独立打包的算法软件；其意义在于提供可复现的 atlas 构建与再分析底座
- accession 级别信息：论文 Data availability 给出 raw sequencing EGA accession `EGAS00001005791`
- 复用性：高

## 对新算法开发的贡献程度
- 评级：**低（直接算法创新）/很高（算法底座价值）**
- 原因：它不是提出新损失函数或新模型结构的算法论文，但它提供了跨器官、跨供者、可资源化发布的健康人参考底座，特别适合用于 cross-tissue reference mapping、tissue-aware representation learning、healthy baseline deviation scoring 和预训练/迁移学习。

## 对我们方法论文的启发
- 可作为健康 reference backbone，而非仅仅作为背景文献引用。
- 适合发展 cross-tissue immune representation learning。
- 适合做 healthy baseline deviation modeling。
- 应显式建模 tissue-aware integration，而不是把组织差异都当作 batch effect 消去。
- 可将其用于预训练，再对疾病队列做 adapter/fine-tuning 式迁移。

## 方法局限对建模的提醒
- 健康供者规模有限，不能把它直接等同于完整人群分布。
- 组织覆盖与细胞深度不均衡，会影响长尾细胞类型学习。
- 解离应激和采样流程会给某些状态带来系统偏差，因此算法评测时应避免把 reference 当作绝对真值。

## 总结
Tabula Sapiens 的价值在于“提供高质量参考底座并定义跨组织可比分析框架”，而不是提出全新计算方法。对算法研究来说，它更像高质量 benchmark、reference atlas 和 pretraining resource 的结合体。
