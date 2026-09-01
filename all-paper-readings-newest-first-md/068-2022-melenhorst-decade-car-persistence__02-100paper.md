# 《Decade-long leukaemia remissions with persistence of CD4+ CAR T cells》精读

## 论文信息

- 作者：J. Joseph Melenhorst、Gregory M. Chen、Zhengshan Wang 等
- 期刊：*Nature*
- 年份：2022；602: 503–509
- DOI：[10.1038/s41586-021-04390-6](https://doi.org/10.1038/s41586-021-04390-6)
- PubMed：[PMID 35110735](https://pubmed.ncbi.nlm.nih.gov/35110735/)；[PMC9166916](https://pmc.ncbi.nlm.nih.gov/articles/PMC9166916/)
- 数据：[dbGaP phs002931.v1.p1](https://www.ncbi.nlm.nih.gov/projects/gap/cgi-bin/study.cgi?study_id=phs002931.v1.p1)；BioProject `PRJNA841739/PRJNA841740`
- 更正：[10.1038/s41586-022-05376-8](https://doi.org/10.1038/s41586-022-05376-8)

## 一句话结论

两名 CLL 患者在 CD19-CAR T 治疗后维持十年缓解；早期由 CD8⁺/γδ T 细胞参与的反应逐渐转为少数高度扩增、持续增殖且具细胞毒程序的 CD4⁺ CAR T 克隆主导。

## 数据护照（先看这一表）

| 维度 | 数值/内容 | 分析提醒 |
|---|---:|---|
| 患者 | 2 | 深度纵向个案，不是疗效预测队列 |
| 随访 | 约 10 年与 9 年 | 晚期 CAR T 占全部 T 细胞约 0.8%/0.1% |
| LVIS | 7,930 / 3,406 个独特位点 | 含输注品及纵向样本 |
| CyTOF | >45,000 个 CAR⁺ 细胞 | 40 抗体 panel；每位患者 5 个可分析时间点 |
| 晚期 CITE-seq | 患者1：1,437 个 T 细胞；患者2：153 个 CAR⁺ T | 患者1含 1,149 CAR⁺ 和 288 CAR⁻ |
| 数据权限 | dbGaP controlled access | 2 subjects、6 samples；不能匿名直接下载 FASTQ |

## 1. 研究要解决的问题

CAR T 在完成肿瘤清除后能否维持十年？长期存活的是何种细胞状态和克隆？作者将整合位点、CyTOF、CITE-seq 与单细胞 TCR 联用，重建两名最早接受 CTL019 的患者从输注到十年的细胞生态。

## 2. 方法框架

- qPCR/流式追踪 CAR T 持续性与 B 细胞再生；
- 慢病毒整合位点测序（LVIS）追踪克隆；
- 40 参数 CyTOF 观察长期表型变化；
- 10x 5′ GEX + TotalSeq-C ADT + V(D)J 联测；
- 对关键时间点做克隆、细胞周期、细胞毒与耗竭程序分析。

## 3. 数据规模与图谱组成

### 3.1 数据到底是什么

这是两个患者的超长纵向多组学轨迹，而不是横截面大队列：

1. **输注产品**：作为初始克隆与表型基线；
2. **早期反应阶段**：观察 CD8⁺、γδ 与 CD4⁺ CAR T 的更替；
3. **长期维持阶段**：在 9–10 年时间点用 CITE-seq/TCR 深描残存 CAR T；
4. **克隆层**：LVIS 和 TCR 提供两种克隆标识；
5. **状态层**：RNA、表面蛋白、细胞周期和 CyTOF 联合刻画增殖性 CD4⁺ 细胞毒状态。

### 3.2 多大规模、覆盖哪些时间点

| 数据层 | 患者1 | 患者2 | 说明 |
|---|---:|---:|---|
| 全程独特 LVIS | 7,930 | 3,406 | 包括输注和随访样本 |
| 输注品独特 LVIS | 3,378 | 1,216 | 反映制造产品克隆复杂度 |
| 晚期 CAR T 比例 | 约全部 T 的 0.8% | 约 0.1% | 低频但持续存在 |
| 晚期高质量细胞 | 1,437 个 T | 153 个 CAR⁺ T | 患者1为 9.3 年，患者2为 6.5 年 |
| 患者1细分 | 1,149 CAR⁺ + 288 CAR⁻ | — | 可作同一样本内参照 |

补充的 CITE-seq 时间点包括：患者2第 3 月 552 个、3 年 242 个验证 CAR 细胞；患者1第 12 月 113 个、第 15 月 93 个 CAR 细胞。患者1 9.3 年时共识别 27 个 CAR clonotypes，最大的 3 个占 CAR T 的 90% 以上；约 30% 的 CAR T 位于 S/G2/M，而同样本 CAR⁻ T 低于 7%。

CyTOF 使用 40 抗体 panel，总计分析超过 45,000 个 CAR⁺ 细胞；每位患者有 5 个至少含 100 个 CAR⁺ 细胞的时间点。这里的“>45,000”是跨时间点细胞事件数，不是独立样本数。

### 3.3 CITE-seq 数据结构

- 10x Genomics 5′ gene expression；
- TotalSeq-C 抗体标签（ADT）；
- 配对 TCR V(D)J；
- NextSeq 550 测序；目标深度约为 GEX 50,000 reads/cell，TCR 与 ADT 各 5,000 reads/cell；
- 论文过滤大致为 200–5,000 个检测基因、线粒体 reads <5%，并要求可检测 TCR。

这使每个保留细胞可同时连接转录状态、表面蛋白和 clonotype，但低细胞数的历史时间点仍受采样噪声影响。

### 3.4 dbGaP 数据包与下载方式

论文初版称数据将提交 dbGaP；随后更正明确 accession 为 **phs002931.v1.p1**。dbGaP 页面记录：

- 2 subjects；
- 6 samples；
- 原始 5′ CITE-seq/TCR 测序；
- 管理 BioProject `PRJNA841739`，数据 BioProject `PRJNA841740`；
- SRA Study Summary 当前显示 48 个已释放 runs。48 是模态/文库/run 层级，不是 48 份生物学样本。

获取原始数据需要 dbGaP 授权账户、机构签署与数据使用申请。获批后通过 dbGaP/SRA 下载 run；建议先下载 sample/run metadata，再按受试者、时间点和 library type 建立 manifest。论文还说明去标识化处理后数据存在独立公共入口，但公开正文未给出可稳定核实的具体 URL，本报告不推测链接。

### 3.5 下载后先做什么

1. 将 `subject × timepoint × modality × run` 分开建表；
2. 检查 GEX、ADT 与 VDJ 是否能以 10x barcode 对齐；
3. 以患者内 clonotype 为单位追踪，绝不跨患者直接合并相同 TCR；
4. 分析极低频晚期群体时报告绝对细胞数和抽样深度；
5. 将 LVIS clone 与 TCR clonotype 当作互补但定义不同的克隆单位。

## 4. 主要发现

早期反应包含 CD8⁺ 和 γδ CAR T；长期阶段则由少数 CD4⁺ 克隆主导。这些细胞表达 Ki-67、GZMA/GZMK 等增殖和细胞毒程序，并非简单的静息记忆或完全衰竭终点。

## 5. 与“状态导航”最相关的分析

该研究证明治疗成功可伴随明显的状态接力：早期效应群体并不一定就是十年后维持群体。实时优化系统因此不能只最大化输注后峰值扩增，还应追踪克隆多样性、CD4/CD8 构成、增殖与长期抗原依赖。

## 6. 推荐图版

- Fig. 1：十年临床轨迹与 CAR T 持续性。
- Fig. 2：LVIS 克隆更替。
- Fig. 3/4：CyTOF 与单细胞多组学揭示长期 CD4⁺ 状态。

## 7. 创新价值

将十年级别随访与多模态单细胞和两类克隆追踪手段结合，直接展示 CAR T 生态随时间重构。

## 8. 局限性

仅两名超长缓解者；缺乏失败对照；历史样本量和保存质量不均；状态与疗效之间仍可能受持续抗原、宿主环境和治疗史共同驱动。

## 9. 对本章节的作用

适合支撑“状态是动态路径而非静态标签”，并作为 live tracking 需要跨年、多模态和克隆层 readout 的例证。

## 10. 可直接用于综述的观点

> 两名 CLL 患者在 CD19-CAR T 治疗后维持十年缓解，早期 CD8⁺/γδ 反应逐渐被少数持续增殖的细胞毒性 CD4⁺ CAR T 克隆取代，显示长期疗效依赖动态克隆—状态重构（Nature 2022, Melenhorst）。

## 11. 避免误读

- 不要把 2 人研究写成“CD4 CAR T 普遍优于 CD8 CAR T”。
- 不要把激活/细胞毒基因表达简单归类为耗竭。
- 不要把 dbGaP 的 48 runs 当成 48 个患者或时间点。

