# 《Integrating single-cell RNA and T cell/B cell receptor sequencing with mass cytometry reveals dynamic trajectories of human peripheral immune cells from birth to old age》精读

## 论文信息

- 第一作者：Yufei Wang、Ronghong Li、Renyang Tong、Taiwei Chen（共同第一作者）
- 期刊：*Nature Immunology*，2025；26: 308–322
- DOI：10.1038/s41590-024-02059-6
- 原文：[Nature Immunology](https://www.nature.com/articles/s41590-024-02059-6)
- 开放全文：[PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11785523/)
- 原始数据：[GSA-Human HRA009014](https://ngdc.cncb.ac.cn/gsa-human/browse/HRA009014)
- 处理数据：[Synapse syn61609846](https://www.synapse.org/Synapse:syn61609846)
- 模型代码：[GitHub—siAgeModel](https://github.com/shanzha9/siAgeModel)

## 一句话结论

研究以出生至 90 岁以上的健康人 PBMC 为对象，联合 scRNA、scTCR/scBCR、CyTOF 和功能实验，发现 T 细胞是受年龄影响最强的循环免疫谱系，并揭示早年与老年两次效应记忆 T 细胞克隆扩增高峰、CD4/CD8 naive T 细胞不同的衰减轨迹及青春期 MAIT 细胞高峰。

## 1. 研究问题

既往免疫衰老研究多集中于成年人或老年人，难以区分“发育建立”“成年稳态”和“衰老退化”。本文试图建立贯穿全生命周期的外周免疫基线，同时回答细胞组成、转录状态、细胞间通讯和抗原受体库如何随年龄共同变化。

## 2. 数据护照

| 维度 | 规模/内容 | 解释 |
|---|---:|---|
| 上海浦东健康队列 | 220 人 | 年龄从脐带血新生儿至 ≥90 岁；分为 13 个年龄点 |
| scRNA + scTCR/scBCR | 61 人，538,266 个 QC 后 PBMC | 10x 5′；同一细胞连接表达与受体序列 |
| 细胞注释 | 11 个大类、25 个亚群 | CD4、CD8、γδ T、NK、B、浆细胞、单核/DC 等 |
| αβ TCR 覆盖 | 超过 90% 的 αβ T 细胞具有匹配 TCR 与转录组 | 适合进行年龄、状态与克隆扩增联合分析 |
| CyTOF 验证 | 70 名健康人；41-plex 抗体面板 | 验证细胞比例和蛋白表达轨迹 |
| bulk RNA-seq | 34 名健康人 | 验证年龄相关表达趋势 |
| 其他功能验证 | 55 名健康人 | 流式及 MAIT 抗菌等实验 |
| 外部 siAge 验证 | 89 人 | 33 名健康人 + 56 名免疫功能受扰者 |

注意：538,266 是全部 PBMC，不是全部 T 细胞；论文正文没有给出一个可直接引用的“全部 T 细胞总数”。220 名内部健康参与者分配至不同实验队列，不能把各模态当成同一批人全部做了所有检测。

## 3. 技术与分析框架

1. 10x Genomics 5′ scRNA-seq 同时构建 TCR/BCR V(D)J 文库。
2. scVIIntegration 校正批次，分两轮注释为 25 个 PBMC 亚群。
3. 以单调差异基因描述随年龄持续增加或减少的状态程序。
4. 以配体—受体网络分析全生命周期细胞间通讯和免疫检查点重排。
5. 以 STARTRAC expansion、克隆大小和多种 diversity index 分析 TCR repertoire。
6. 以 25 类细胞的表达特征和随机森林建立单细胞免疫年龄模型 siAge。

## 4. T 细胞与 TCR 的核心发现

### 4.1 T 细胞是年龄影响最强的谱系

年龄相关差异基因最多的前十个亚群中有八个是 T 细胞。CD4_Naive_CCR7、CD4_TCM_AQP3 和 CD8_Naive_LEF1 的单调年龄相关变化最多；随年龄增强的程序偏向 IFN-II、NF-κB 和炎症，下降程序涉及端粒组织与细胞骨架。

### 4.2 克隆扩增呈“双峰”而非只在老年升高

高水平克隆扩增在 2–12 岁和 70–90 岁各出现一次高峰。克隆大小 ≥50 的 high-TCE 细胞只集中在三个效应记忆群：CD8_TEM_GNLY、CD4_TEM_GNLY 和 CD8_TEM_CMC1，其中 CD8_TEM_GNLY 占比最高。

两个高峰的生物学不同：早年扩增的 CD8_TEM_GNLY 偏细胞毒、氧化磷酸化和增殖；老年扩增则偏 IFN、抗病毒及抗凋亡程序。因此“克隆扩增”本身不能直接等同于免疫衰老。

### 4.3 naive CD4 与 naive CD8 不是同一条衰老曲线

- naive CD4 T 细胞在生命早期下降较快，成年后趋缓。
- naive CD8 T 细胞早期下降较缓，成年和老年阶段下降加速。
- TCR 多样性也呈对应差异：CD4 naive 在早期丢失更明显，CD8 naive 在晚年坍缩更突出。

### 4.4 MAIT 细胞在青春期达到高峰

CD8_MAIT_SLC4A10 的比例与 TCR 多样性在青春期、尤其约 18 岁达到高峰；CD161/KLRB1 蛋白和抗菌实验提供正交支持。该结果提示青春期不是简单的“成人缩小版”，而是存在特定的先天样 T 细胞成熟窗口。

## 5. siAge 的含义

siAge 以 25 个细胞亚群的表达信息构建，最终由分布于 T 细胞亚群的 21 个关键基因驱动。训练集中预测年龄与实际年龄高度相关；独立健康队列仍保持较高相关性，而疾病队列中相关性下降。

这说明 siAge 可作为免疫稳态偏离的研究指标，但不能直接解释为临床“生物年龄诊断”。模型训练人群来自单一地区，疾病验证队列规模和病种有限，必须做跨种族、跨平台和前瞻性验证。

## 6. 数据获取与复用

| 数据 | 入口 | 备注 |
|---|---|---|
| 原始 scRNA/scTCR/scBCR | [HRA009014](https://ngdc.cncb.ac.cn/gsa-human/browse/HRA009014) | 人遗传数据，受控申请；BioProject PRJCA027606 |
| 处理后 scRNA Seurat 对象 | [Synapse syn61609846](https://www.synapse.org/Synapse:syn61609846) | 论文 Data availability 的正式入口 |
| bulk RNA | [Synapse syn61853526](https://www.synapse.org/Synapse:syn61853526) | 独立验证数据 |
| CyTOF | [Synapse syn61765870](https://www.synapse.org/Synapse:syn61765870) | 单细胞蛋白数据 |
| siAge 代码 | [GitHub](https://github.com/shanzha9/siAgeModel) | 随机森林模型及相关脚本 |
| 在线模型 | [siAge web app](https://pu-lab.sjtu.edu.cn/shiny/siage-model/) | 服务状态可能变化 |

复用时以“人”为统计单位，不应把 538,266 个细胞当作独立样本。年龄既有 13 个采样点，也被合并为 8 个生命阶段；复现论文结果必须确认所用分组。TCR 分析还需区分细胞数、独特 clonotype 数和 repertoire diversity，三者不能混用。

## 7. 推荐图版

- **Fig. 1**：队列、模态、25 个 PBMC 亚群和年龄组成变化；介绍数据规模首选。
- **Fig. 4**：TCR 克隆扩增双峰、V/J 使用及效应记忆 T 细胞扩增；讲 TCR 首选。
- **Fig. 5**：naive CD4/CD8 不同衰减轨迹及青春期 MAIT 高峰；讲 T 细胞生命周期首选。
- **Fig. 7**：siAge 构建与外部验证；讲转化工具时使用。
- **Extended Data Fig. 6a**：各 αβ T 亚群的 TCR 检出率；说明受体数据覆盖时最直接。

若只能选一张放入 T 细胞图谱章节，选 **Fig. 5**；若重点是“状态 + 克隆”，选 **Fig. 4**。

## 8. PPT 单页格式

**标题**：生命周期基线揭示 T 细胞状态与克隆库的非线性变化

**正文**：61 人、538,266 个 PBMC；超过 90% 的 αβ T 细胞连接转录组与 TCR。效应记忆 T 细胞克隆扩增在早年和老年出现双峰，naive CD4 与 CD8 T 细胞具有不同的数量及 repertoire 衰减轨迹。

**配图**：Fig. 1a + Fig. 4a/c 或 Fig. 5a–j。

**页脚引用**：Nature Immunology 2025, Wang。

## 9. 在 T 细胞图谱中的定位

本文提供的是**健康人外周血的年龄基线**。它适合在整合肿瘤、感染、衰老或细胞治疗队列时控制年龄混杂，也适合验证某个 T 细胞状态是否超出正常生命周期范围；它不能替代组织 T 细胞、TIL、胸腺发育或空间图谱。

## 10. 局限性与避免误读

- 仅测外周血，无法直接描述组织驻留 T 细胞或局部免疫生态位。
- 横断面年龄队列不能证明同一个人的细胞随时间如何变化。
- 年龄组并非连续均匀采样，LOESS 曲线不等于真实个体轨迹。
- TCR 克隆扩增不等同于已知抗原特异性，也不能仅凭克隆大小判断保护性或衰老性。
- MAIT 青春期高峰与结核流行病学的一致性属于关联，功能实验支持抗菌能力，但不证明其决定人群疾病发生率。
- siAge 是研究性模型，不是临床诊断工具。

## 11. 可直接用于综述

> 健康人外周免疫系统并非随年龄单向衰退：效应记忆 T 细胞克隆扩增在早年和老年出现具有不同功能程序的双峰，而 naive CD4 与 CD8 T 细胞从出生起即沿不同的数量和 TCR 多样性轨迹变化（Nature Immunology 2025, Wang）。

