# 037. Longitudinal analyses reveal immunological misfiring in severe COVID-19

## 基本信息
- 年份：2020
- 期刊：Nature
- DOI：https://doi.org/10.1038/s41586-020-2588-y
- 主题：longitudinal COVID systems immunology；cytokine signatures；T-cell count/function context；disease trajectories

## 为什么保留
这篇不是单细胞组学论文，保留它的理由是方法学边界清楚：单细胞 atlas 常缺真正 longitudinal donor trajectories，而这篇用 longitudinal immune profiling 把 severe COVID 的 disease course写成可分型的 immune-signature trajectories。它可以在 method report 中承担“为什么 cell-level atlas 还需要 donor-time outcome modeling”的参照角色。

## 数据与研究设计
- 物种/样本：`Homo sapiens`；serial patient blood/immune profiling
- cohort：serially analysed `113` patients with moderate or severe COVID-19
- 观测重点：plasma cytokines/chemokines、viral load、immune-cell/T-cell profiling and clinical course
- 数据开放：ImmPort study `SDY1655`
- 模态边界：不是 scRNA-seq，不提供 single-cell transcriptome atlas 或 TCR clonotype resource

## 文章中的算法/分析流程
### 1. Longitudinal immune signature discovery
- 论文用 unsupervised clustering 从 cytokine/chemokine measurements 中归纳 immune signatures。
- 签名与 disease trajectories、viral load and clinical outcomes 关联，形成 donor-time 视角。

### 2. T-cell readout 处于 systems context
- 文中对 T-cell counts/profiles的解释服务于 adaptive response misfiring，而非在单细胞表达空间发现 T-cell states。
- 因而它适合与 PBMC scRNA papers 对照：前者有细胞状态分辨率，后者有更明确的时间与 outcome 结构。

### 3. 对算法写作的意义
- 如果我们只综述 scRNA clustering/integration，会忽略真正影响“人群免疫力”的 patient-level disease trajectory。
- 这篇提示后续算法应把 single-cell features 聚合到 donor-time representation，再对 trajectory/outcome 建模。

## 与 T 细胞—人群免疫力的关系
- T-cell adaptive response 是 severe disease interpretation 的一部分。
- 但它不能当作 single-cell T-cell benchmark；正确角色是 longitudinal immune-outcome anchor。

## 数据可用性
- DOI 已更正为链接形式：https://doi.org/10.1038/s41586-020-2588-y
- Public data accession：ImmPort `SDY1655`
- 本轮未定位到作者发布的独立 analysis-code repository
- 数据输入/输出边界：
  - 输入：serial immune measurements、clinical metadata、viral measures
  - 输出：immune signatures、trajectory associations and outcome-linked summaries

## 算法贡献与不足
- 直接新算法：**低**；用的是聚类、correlation/association 与 longitudinal interpretation。
- 任务定义价值：**高**；把 immune state 连接到 course/outcome。
- 不足：
  - 非 single-cell transcriptomic
  - 不含 receptor sequence
  - 对“哪些具体 T-cell states 导致 trajectory”不能直接回答

## 对新算法开发的启发
1. 从 single-cell atlas构建 donor-time immune embedding。
2. 对 trajectory phenotype 进行 early prediction and uncertainty calibration。
3. 联合 cell composition、cytokines、repertoire and clinical metadata，而不是只做 cell-level differential expression。

## 可放入 method report 的表述
Lucas et al. 并非单细胞方法论文，但它把 immune dysregulation写成 longitudinal donor trajectories；这恰好说明单细胞 T-cell algorithms 若要讨论 population immunity，最终需要对接 patient-time and outcome structure。

## 一句话结论
这是一篇应被标注为边界文献的 longitudinal systems-immunology anchor，不应与 scRNA/TCR atlas 混写。
