# 90｜非激活快速 CAR-T 制造：24 小时内完成转导并保留原始状态

## 论文信息

- **题目**：Rapid manufacturing of non-activated potent CAR T cells
- **作者**：Ghassemi S, Durgin JS, Nunez-Cruz S, et al.
- **期刊 / 年份**：Nature Biomedical Engineering, 2022（在线发表于 2022）
- **DOI**：[10.1038/s41551-021-00842-6](https://doi.org/10.1038/s41551-021-00842-6)
- **PubMed**：[35102352](https://pubmed.ncbi.nlm.nih.gov/35102352/)
- **全文**：[PMC8860360](https://pmc.ncbi.nlm.nih.gov/articles/PMC8860360/)
- **出版方数据页**：[Nature article and supplementary information](https://www.nature.com/articles/s41551-021-00842-6)
- **本文定位**：通过优化慢病毒进入静息 T 细胞的条件，取消传统预激活与多日扩增，在约 24 小时内制备 non-activated CAR-T；是“最小化 ex vivo 状态扰动”的快速制造代表作。

## 一句话结论

**短时 serum starvation、deoxynucleosides 和 IL-7/IL-15 可支持慢病毒直接转导非激活 T 细胞，使约 24 小时制造的 CAR-T 保留更原始的状态，并在白血病模型中以极少实际 CAR+ 细胞产生强体内扩增和抗肿瘤效力。**

---

## 数据护照（先看数据是否真的可取得）

| 数据层级 | 内容 | 规模 | 公开状态 | 获取方式 |
|---|---|---:|---|---|
| 正文/补充实验 | 转导优化、表型、整合、功能、动物实验 | 多数健康供者实验 n=6；动物按图 n=7–10 等 | 图中汇总 | PMC/Nature 正文与 SI |
| Source Data Fig. 3 | NALM6 相关肿瘤生长数据 | 一个 XLSX，约 9.7 KB | **公开** | Nature MOESM3 |
| Source Data Fig. 5 | 低剂量 CAR-T 压力测试 | 一个 XLSX，约 9.5 KB | **公开** | Nature MOESM4 |
| Source Data Fig. 6 | CAR33/AML 等对应数据 | 一个 XLSX，约 9.6 KB | **公开** | Nature MOESM5 |
| Source Data Extended Data Fig. 1 | 扩展图肿瘤数据 | 一个 XLSX，约 9.7 KB | **公开** | Nature MOESM6 |
| Supplementary information | 扩展方法、表格、附图 | PDF，约 660 KB | **公开** | Nature MOESM1 |
| Reporting summary | 研究设计与统计报告 | PDF，约 90 KB | **公开** | Nature MOESM2 |
| 原始流式/整合位点/测序 | FCS、FASTQ、整合位点坐标、完整动物原始数据 | — | **未见公共仓库/accession** | 文中称其他数据可向作者请求 |

> **关键辨析**：Nature 提供的四个 Source Data XLSX 主要对应肿瘤生长曲线，不是整篇论文的完整原始数据。论文虽然进行了 vector integration site 相关测定，但公开页面未给出 FASTQ、整合位点表或 GEO/SRA accession。

---

## 1. 论文解决的核心问题

即使把传统 9–14 天制造缩到 3 天，T 细胞仍需强烈激活，状态已被改变。作者希望更进一步：

1. 能否在不进行传统 TCR/CD3-CD28 预激活的情况下，用慢病毒有效转导静息 T 细胞？
2. 能否把制造时间压缩到约一天，同时保留足够的 CAR 表达和安全可控的载体整合？
3. 低转导率、低绝对 CAR+ 数量的产品，是否能依靠体内扩增产生治疗效力？
4. 这一策略能否从 CD19 白血病扩展到 AML 和患者来源 T 细胞？

---

## 2. 实验与技术路线

### 2.1 静息细胞转导优化

作者围绕静息 T 细胞的限制因素优化多个变量：

- 转导前 **serum starvation 3 小时**；
- 加入 **deoxynucleosides（dNs）50 μM**；
- 加入 **IL-7 和 IL-15，各 10 ng/mL**；
- 慢病毒转导约 **20 小时**；
- 代表性优化条件使用 **MOI 5**，部分机制/剂量实验包括不同 MOI。

产品不经历传统 beads 预激活和多日扩增，通常在约 24 小时完成主要制造。

### 2.2 对照产品

- non-activated、快速制造产品（常称 day 1）；
- 传统激活并扩增约 9 天的产品（day 9）；
- 未转导 T cells 或 vehicle 等对照。

部分实验的 day 1 和 day 9 不是同一供者配对，解释时必须注意供者差异。

### 2.3 多层验证

- CAR 转导率和表面表达；
- naive/memory/activation 分化表型；
- vector copy/integration kinetics 和整合位点相关分析；
- 体外杀伤、细胞因子、增殖；
- NALM6 白血病、低剂量压力测试、CAR33 AML 模型；
- 一名 DLBCL 患者来源 T 细胞的 proof-of-concept。

---

## 3. 数据规模与图谱组成（重点）

### 3.1 状态—工艺比较框架

论文的数据图谱可拆成四层：

| 层级 | 工艺/模型 | 主要读数 |
|---|---|---|
| 工艺优化 | starvation、dNs、cytokines、MOI、时间 | 转导率、CAR 表达、细胞活率 |
| 状态表征 | day 1 non-activated vs day 9 activated | 分化/激活表型、扩增潜能 |
| 基因递送安全性 | vector integration kinetics/site analysis | 整合拷贝、位点分布相关指标 |
| 治疗功能 | CD19-NALM6、CAR33-AML、患者来源产品 | 肿瘤 BLI、生存、体内 CAR-T 扩增 |

这不是一个单一组学矩阵，而是制造变量到体内功能的多实验链。

### 3.2 供者与重复规模

- 多数关键健康供者体外实验为 **n=6 独立供者**。
- 图注中 n 会随具体比较改变，应逐图确认。
- 初始 day 1 versus day 9 NALM6 对比约 **10 只/组**，但 day 1 与 day 9 产品来自不同供者，这构成重要混杂。
- 优化工艺后的 non-activated 产品来自 **3 名供者**，在非白血病 NSG 环境中测得平均转导率约 **8%（范围 6%–16%）**。
- 患者来源验证仅 **1 名 DLBCL 患者**，属于可行性示例而非临床队列。

### 3.3 低剂量 NALM6 压力测试

关键剂量组包括：

- day 1 non-activated：**2×10^6、0.7×10^6、0.2×10^6 总 T cells**；
- day 9 activated：约 **3×10^6 总 T cells**；
- 每个主要剂量组约 **n=8 小鼠**。

由于快速产品 CAR 转导率较低，0.2×10^6 总细胞中的实际 CAR+ T cells 估计仅约 **12,000–32,000**，仍可在 NALM6 模型中产生显著清瘤效应。该估计取决于该批次转导率，不能把“0.2×10^6 总 T cells”写成“0.2×10^6 CAR+ cells”。

### 3.4 CAR33/AML 与患者样本

- CAR33 AML 模型使用约 **5×10^6 T cells**，主要分组约 **n=10**。
- 该实验受到异种反应、实验截断及 COVID-19 shutdown 等现实限制，不能与完整 NALM6 长期实验等量解释。
- DLBCL 患者来源 CD19 CAR-T 验证：图中约 day 1 n=7、day 9 n=10、non-transduced n=10 的小鼠规模；长期控制按可评估动物计算，non-activated 组约 **3/6** 显示持久控制。需按原图说明动物损失/可评估数，而不是简单报告 3/7。

### 3.5 公开 Source Data 文件清单

Nature 的静态资源基址为：

```text
https://static-content.springer.com/esm/art%3A10.1038%2Fs41551-021-00842-6/MediaObjects/
```

| 文件 | 内容 | 实测大小 |
|---|---|---:|
| `41551_2021_842_MOESM1_ESM.pdf` | Supplementary Information | 659,797 bytes |
| `41551_2021_842_MOESM2_ESM.pdf` | Reporting Summary | 90,418 bytes |
| `41551_2021_842_MOESM3_ESM.xlsx` | Source Data Fig. 3 | 9,658 bytes |
| `41551_2021_842_MOESM4_ESM.xlsx` | Source Data Fig. 5 | 9,452 bytes |
| `41551_2021_842_MOESM5_ESM.xlsx` | Source Data Fig. 6 | 9,592 bytes |
| `41551_2021_842_MOESM6_ESM.xlsx` | Source Data Extended Data Fig. 1 | 9,654 bytes |

这些 XLSX 体积很小，与其内容相符：它们主要给出作图用肿瘤生长/发光相关数值，不包含全套流式事件或测序原始数据。

### 3.6 直接下载链接

- [Supplementary Information PDF](https://static-content.springer.com/esm/art%3A10.1038%2Fs41551-021-00842-6/MediaObjects/41551_2021_842_MOESM1_ESM.pdf)
- [Reporting Summary PDF](https://static-content.springer.com/esm/art%3A10.1038%2Fs41551-021-00842-6/MediaObjects/41551_2021_842_MOESM2_ESM.pdf)
- [Source Data Fig. 3 XLSX](https://static-content.springer.com/esm/art%3A10.1038%2Fs41551-021-00842-6/MediaObjects/41551_2021_842_MOESM3_ESM.xlsx)
- [Source Data Fig. 5 XLSX](https://static-content.springer.com/esm/art%3A10.1038%2Fs41551-021-00842-6/MediaObjects/41551_2021_842_MOESM4_ESM.xlsx)
- [Source Data Fig. 6 XLSX](https://static-content.springer.com/esm/art%3A10.1038%2Fs41551-021-00842-6/MediaObjects/41551_2021_842_MOESM5_ESM.xlsx)
- [Source Data Extended Data Fig. 1 XLSX](https://static-content.springer.com/esm/art%3A10.1038%2Fs41551-021-00842-6/MediaObjects/41551_2021_842_MOESM6_ESM.xlsx)

命令行批量下载示例：

```bash
base='https://static-content.springer.com/esm/art%3A10.1038%2Fs41551-021-00842-6/MediaObjects'
for i in 1 2 3 4 5 6; do
  wget "$base/41551_2021_842_MOESM${i}_ESM.$([ $i -le 2 ] && echo pdf || echo xlsx)"
done
```

Windows PowerShell 可逐链接使用 `Invoke-WebRequest -Uri <URL> -OutFile <filename>`。

### 3.7 下载后如何检查 Source Data

建议：

1. 打开每个 XLSX，先查看 sheet 名、第一行标题、单位和 treatment label。
2. 将 `mouse_id`、treatment、CAR dose、timepoint、tumor signal 分别转成长表。
3. 与主图核对：动物数、缺失时间点、是否对数尺度、mean/SEM 定义。
4. 不要把不同图的 mouse 编号默认视为同一批动物。
5. Source Data 没有覆盖的图，不应从其他 XLSX 推断原始值。

### 3.8 未公开数据的明确边界

当前没有找到以下公共文件：

- 流式 `.fcs`；
- 单细胞或 bulk RNA-seq 表达矩阵；
- vector integration site FASTQ；
- 可复用的整合位点坐标表；
- 所有动物的原始 IVIS 图像；
- 完整分析代码。

论文的数据可用性说明允许合理请求其他数据，但这不是无需申请的公共下载，也不能保证长期、无条件获得。

---

## 4. 主要结果

### 4.1 非激活 T 细胞也可被有效慢病毒转导

通过短时饥饿、补充 dNs 与 IL-7/IL-15，作者绕过静息 T 细胞对逆转录/整合不利的代谢限制，在不进行传统强激活的情况下获得可检测且具有功能的 CAR 表达。

### 4.2 保留状态比追求高转导率更重要

non-activated 产品转导率明显低于传统 day 9 产品，但细胞更少经历状态扰动，进入体内后扩增能力强。结果把评价重点从“收获时 CAR% 最大化”转向“实际 CAR+ 细胞的体内扩增和效力”。

### 4.3 极低数量 CAR+ 细胞仍能清除白血病

低剂量压力测试估计仅 1.2万–3.2万 CAR+ T cells 即可控制 NALM6，突出产品状态对单位细胞效力的巨大影响。

### 4.4 平台具有跨靶点和患者来源潜力，但证据仍初步

CAR33 AML 和单例 DLBCL 患者来源实验支持可扩展性；但样本量和模型中断等限制意味着它们主要是 proof-of-concept。

---

## 5. 分子机制与状态导航变量

### 5.1 可能机制

- serum starvation 改变转导窗口和细胞代谢环境；
- dNs 为病毒逆转录提供底物；
- IL-7/IL-15 提供生存/代谢支持而不等同于强 TCR 激活；
- 减少 CD3/CD28 强刺激和连续体外分裂，保留 naive/early-memory 潜能。

### 5.2 可操作参数

| 参数 | 代表性条件 | 作用 |
|---|---|---|
| starvation | 3 h | 提升静息细胞转导可行性 |
| dNs | 50 μM | 支持逆转录所需底物 |
| IL-7 / IL-15 | 各 10 ng/mL | 支持存活与早期状态 |
| MOI | 代表性 MOI 5，实验中亦测试其他剂量 | 平衡转导率、成本与整合风险 |
| 转导时间 | 约 20 h | 形成约 24 h 的快速流程 |
| 是否预激活 | 无传统 TCR 预激活 | 最大限度减少状态扰动 |

---

## 6. 最值得复用的图与数据

1. **工艺优化矩阵**：starvation、dNs、cytokines 对转导的组合效应。
2. **day 1 versus day 9 状态图**：展示制造步骤对细胞状态的扰动。
3. **低剂量 NALM6 曲线（Source Data Fig. 5）**：最强单位细胞效力证据。
4. **整合动力学/位点分析**：用于讨论快速转导的安全性，但底层测序数据未公开。
5. **患者来源验证**：说明平台潜力，同时应明确单病例限制。

---

## 7. 创新点

- 将快速制造从“缩短扩增”推进到“取消传统预激活”。
- 针对静息细胞转导的代谢/底物限制提出可操作的组合方案。
- 用实际 CAR+ 细胞数量而非总输注 T 细胞数解释极低剂量效力。
- 同时考虑制造周期、细胞状态、载体整合和体内治疗效果。
- 提供部分主图 Source Data，使关键肿瘤曲线可以直接读取，而非只能数字化图片。

---

## 8. 局限性

- 转导率偏低且有供者间波动，绝对 CAR+ 产量对淋巴细胞极少患者可能成为瓶颈。
- 初始 day 1/day 9 体内比较使用不同供者产品，不能完全排除供者效应。
- DLBCL 患者仅 1 例；AML 模型又受到实验提前终止等影响。
- NSG 模型中人 T 细胞可发生异种反应，可能放大扩增或抗肿瘤效应。
- 公开 Source Data 仅覆盖部分肿瘤曲线；流式和整合位点底层数据未公开。
- 非激活制造的质量控制、载体成本、无菌放行和临床剂量下稳定性仍需临床验证。

---

## 9. 在综述架构中的位置

- **T cell is at the start point**：最大限度保留患者原始 T 细胞状态。
- **Techniques to perturb/manipulate states**：通过代谢底物、细胞因子和激活强度控制状态扰动。
- **Quantitatively characterizing phenotypes/functions**：同时测 CAR%、表型、整合和体内单位细胞效力。
- **Link transitions with molecular drivers**：dNs、IL-7/IL-15、TCR 激活缺失与状态保留相连接。
- **Optimize conditions**：starvation 时长、dNs、cytokine、MOI 和收获时点形成多参数优化问题。
- **Build real-time optimization systems**：这一小时级流程尤其需要在线监测转导、活率、CAR 表达和状态，以按批次决定是否直接放行或补救。

---

## 10. 可直接写入综述的表述

> Ghassemi 等通过 3 小时 serum starvation、50 μM deoxynucleosides、IL-7/IL-15 支持及约 20 小时慢病毒暴露，实现了无需传统 TCR 预激活的约 24 小时 CAR-T 制造。尽管 non-activated 产品的 CAR 转导率通常低于多日扩增产品，其更原始的状态赋予了显著的体内扩增优势；在低剂量 NALM6 模型中，估计仅约 1.2万–3.2万实际 CAR+ T cells 即可产生强抗肿瘤作用。该研究把制造目标从最大化收获时转导率和细胞数，转向最小化状态扰动并最大化单位 CAR+ 细胞的体内效力。

---

## 11. 避免误读

1. **0.2×10^6 是总 T cells，不是 CAR+ T cells**；实际 CAR+ 数量更低且依赖转导率估算。
2. **day 1 与 day 9 并非所有实验都供者配对**；首轮体内比较存在供者混杂。
3. **“non-activated”不等于完全没有外源信号**：细胞接受 starvation、IL-7/IL-15、dNs 和病毒暴露。
4. **有 Source Data 不等于所有原始数据开放**：XLSX 主要是肿瘤生长值。
5. **整合位点被分析不等于数据可下载**：未见 FASTQ 或坐标 accession。
6. **单例患者验证不能代表临床疗效**。

---

## 12. 数据获取清单

- PMC 全文：[PMC8860360](https://pmc.ncbi.nlm.nih.gov/articles/PMC8860360/)
- Nature 文章与附件：[Nature article](https://www.nature.com/articles/s41551-021-00842-6)
- Supplementary Information：[MOESM1 PDF](https://static-content.springer.com/esm/art%3A10.1038%2Fs41551-021-00842-6/MediaObjects/41551_2021_842_MOESM1_ESM.pdf)
- Reporting Summary：[MOESM2 PDF](https://static-content.springer.com/esm/art%3A10.1038%2Fs41551-021-00842-6/MediaObjects/41551_2021_842_MOESM2_ESM.pdf)
- Source Data：[Fig. 3](https://static-content.springer.com/esm/art%3A10.1038%2Fs41551-021-00842-6/MediaObjects/41551_2021_842_MOESM3_ESM.xlsx)、[Fig. 5](https://static-content.springer.com/esm/art%3A10.1038%2Fs41551-021-00842-6/MediaObjects/41551_2021_842_MOESM4_ESM.xlsx)、[Fig. 6](https://static-content.springer.com/esm/art%3A10.1038%2Fs41551-021-00842-6/MediaObjects/41551_2021_842_MOESM5_ESM.xlsx)、[Extended Data Fig. 1](https://static-content.springer.com/esm/art%3A10.1038%2Fs41551-021-00842-6/MediaObjects/41551_2021_842_MOESM6_ESM.xlsx)
- PubMed：[35102352](https://pubmed.ncbi.nlm.nih.gov/35102352/)

**推荐复用优先级**：优先下载四个 Source Data XLSX 复画动物曲线；若研究载体整合、逐细胞状态或完整批次差异，需要另向作者申请原始数据。
