# Cable theory 与 edNEG —— 资料笔记

> 整理日期：2026-09-18
> 定位：两个都属于**计算神经科学 / 生物物理建模**。cable theory 是理解神经元电信号传播的经典骨架；edNEG 是在这个骨架上补上"离子浓度 + 胶质细胞 + 胞外扩散"的现代扩展。
> **它们是一条线上的一前一后，不是两个并列的话题。**

---

## 一、Cable theory（电缆理论）

### 一句话

把神经纤维（树突、轴突）当成一根**漏电的电缆**：内部是导电的液体核心，外面裹着一层既漏电（膜电阻 R_m）又储电（膜电容 C_m）的膜 —— 由此解出膜电位沿纤维方向的时空变化。

### 思想来路

| 时间 | 谁 | 做了什么 |
|---|---|---|
| 1855 | Lord Kelvin（开尔文勋爵） | 海底电报电缆的信号衰减理论 —— **电缆理论是从电报工程借来的，不是类比，是同一套数学** |
| 1907 | Wilhelm Hermann | 首次把神经纤维比作电缆 |
| 1952 | Hodgkin & Huxley | 用实验确认离子流的关键作用（动作电位） |
| **1959** | **Wilfrid Rall** | **正式提出神经电缆理论**，解出任意分支被动树突的稳态问题 |
| 1960 | Rall | 用 Laplace 变换解瞬态问题，估计膜时间常数 |
| 1962 | Rall | *Theory of physiological properties of dendrites* |
| 1964 | Rall | **房室建模（compartmental modeling）** —— 把连续电缆方程离散成一串等电位小房室 |
| 1995 | Segev, Rinzel & Shepherd | 编成 *The Theoretical Foundation of Dendritic Function*（Rall 论文集 + 评论） |

> **一段史料**：Rall 1958 年把手稿投给 *Journal of General Physiology* 被拒，审稿人是 **Eccles** —— 当时的主流观点认为树突只是"管道"，Eccles 的"标准运动神经元"模型把树突作用严重低估了（他算出的膜电阻率小了十倍，空间常数小了 3.2 倍）。后来 *Experimental Neurology* 的编辑鼓励 Rall 把稿子扩成 1959 和 1960 两篇。Rall 估计的树突/胞体输入电导比是 21–35，而 Eccles 的模型只给 2.3。

### 核心方程

被动电缆方程（V 为偏离静息电位的量）：

```
λ² · ∂²V/∂x²  =  τ_m · ∂V/∂t  +  V
```

两个常数决定一切：

| 参数 | 定义 | 物理含义 |
|---|---|---|
| **λ 空间常数** | λ = √(R_m / R_a)（单位长度量） | 信号传播多远才开始显著衰减 |
| **τ_m 膜时间常数** | τ_m = R_m · C_m（单位面积量） | 信号被抹平（低通滤波）多少 |
| **L 电紧张长度** | L = 纤维长度 / λ | 无量纲，判断树突"电学上有多长" |

### 为什么重要

三件事是靠 cable theory 才立住的：

1. **突触位置是计算的一部分。** 同一个突触，长在远端树突 vs 近端，传到胞体的 EPSP 更小、更慢、更钝。在这之前，"突触位置"被认为只是实现细节。
2. **电极记录到的东西已经被细胞自己的几何低通滤波过了。** 你在胞体记到的波形，不是突触处的波形。
3. **等效圆柱（equivalent cylinder）** —— Rall 发现某些分支树突在数学上等价于一根有限圆柱，把复杂树突简化成可解问题。

第 1 条直接催生了 **dendritic computation**（树突计算）这个领域，算现代计算神经科学的地基之一。

---

## 二、edNEG

### 全称与出处

**e**lectrodiffusive **n**euron-**e**xtracellular-**g**lia model
= **电扩散 神经元–胞外–胶质细胞 模型**

> Sætra MJ, Einevoll GT, Halnes G (2021). *An electrodiffusive neuron-extracellular-glia model for exploring the genesis of slow potentials in the brain.*
> **PLoS Computational Biology** 17(7): e1008143.
> DOI: **10.1371/journal.pcbi.1008143** ｜ PMID: 34270543 ｜ PMCID: PMC8318289
> 单位：Simula Research Laboratory（Oslo）、University of Oslo、NMBU
> 资助：挪威研究理事会 DigiBrain（248828）、EU HBP SGA3（945539）

### 它要解决什么问题

标准胞外电位模型 = **多房室模型**（描述神经电动力学）+ **容积导体理论**（volume conductor theory）。

**这套组合算不了慢电位。** 原因有两条：

1. 慢成分依赖**离子浓度动力学**，而标准模型假设离子浓度恒定；
2. 漏掉了**胞外空间的离子扩散**和**胶质细胞的缓冲电流**这两个贡献项。

作者在"作者总结"里说得更直白：**常规电生理记录通常只保留高于 0.1–1 Hz 的成分，把慢电位直接滤掉了** —— 所以标准记录和标准模型都看不见慢电位这块。

### 模型结构

**六个房室**（一维系统）：

| 域 | 房室 | 初始体积分数 |
|---|---|---|
| 神经元 | 胞体层、树突层 | **0.4** |
| 胞外空间 ECS | 两个 | **0.2** |
| 胶质细胞 | 胞体层、树突层 | **0.4** |

**四种离子**：Na⁺、K⁺、Ca²⁺、Cl⁻
**框架**：**KNP**（Kirchhoff–Nernst–Planck）电扩散框架 —— 同时解离子浓度与电位

**膜上有什么**：

| 膜 | 通道/转运体 |
|---|---|
| 神经元–胞外膜 | Na⁺/K⁺/Cl⁻ 漏通道、3Na⁺/2K⁺ 泵、K⁺/Cl⁻ 与 Na⁺/K⁺/2Cl⁻ 共转运体、Ca²⁺/2Na⁺ 交换体 |
| 胞体 | Na⁺、K⁺ 延迟整流通道 |
| 树突 | 电压依赖 Ca²⁺ 通道、电压依赖 K⁺ 后超极化通道、Ca²⁺ 依赖 K⁺ 通道 |
| 胶质–胞外膜 | Na⁺/Cl⁻ 漏通道、内向整流 K⁺ 通道、3Na⁺/2K⁺ 泵 |

**规模**：共 **34 个常微分方程** = 22 个离子动力学 + 6 个门控变量（Hodgkin–Huxley 形式）+ 6 个体积动力学（渗透压引起的肿胀）

### 主要结论

胞外电位梯度拆成三项：

```
Δφ_e  =  Δφ_e,n  +  Δφ_e,g  +  Δφ_e,diff
         ↑神经     ↑胶质      ↑扩散
```

- **三者量级相当** —— 这是核心结论，说明忽略胶质和扩散的模型会系统性出错
- **谁主导取决于刺激条件**
- 当 Δφ_e,n 和 Δφ_e,g 按标准容积导体理论从膜电流源算出来时，扩散项 Δφ_e,diff 相当于一个**"修正项"**：电流回路要靠扩散才能闭合（`I_n + I_g = Ĩ_n + Ĩ_g + Ĩ_diff`，所以 `I_n ≠ Ĩ_n`、`I_g ≠ Ĩ_g`）

### 后续发展

| 年份 | 工作 | 说明 |
|---|---|---|
| 2021 | Sætra et al., PLoS Comput Biol | 原型 |
| ~2023 | *edNEG model with **somatodendritic interactions*** | 加入胞体–树突相互作用（`CINPLA/edNEGmodel` 现在实现的版本，v2.0.0） |
| 2024 | **Signorelli L, Manzoni A, Sætra MJ**, PLoS ONE 19(5): e0303822 | **不确定性量化（UQ）与全局敏感性分析（GSA）**。用代理模型降低计算成本，分离影响静息态的参数量。DOI: 10.1371/journal.pone.0303822 |
| — | Sætra 另一条线：**CMRO₂ 皮层分层定量** | 面向 BOLD fMRI 信号的分层建模 —— 与 fNIRS/BOLD 直接相关 |

---

## 三、两者的关系（一句话讲清）

```
Lord Kelvin 1855 海底电缆
        ↓
   Rall 1959 电缆理论          ← 单域：只解胞内 V(x,t)，胞外当等电位
        ↓
   Rall 1964 房室模型           ← 把电缆离散化，可放非线性通道
        ↓
   多房室 + 容积导体理论        ← 标准胞外电位模型（算不了慢电位）
        ↓
   Sætra 2021 edNEG            ← 三域：神经元 + 胞外 + 胶质，KNP 电扩散
```

**edNEG 的神经元部分仍然建在多房室（= 电缆）框架上** —— 论文原话："Standard models of extracellular potentials are based on a combination of **multicompartmental models** describing neural electrodynamics and **volume conductor theory**"。edNEG 干的事是**在这个基础上再加两个域**。

所以：**cable theory 是 edNEG 的神经元侧理论基础；edNEG 是 cable theory 向"组织层面"的扩展。**

---

## 四、代码资源

| 仓库 | 语言 | 说明 | 最后推送 |
|---|---|---|---|
| **[CINPLA/edNEGmodel](https://github.com/CINPLA/edNEGmodel)** | Python | **官方核心实现**。KNP 连续性方程，一维六房室。Zenodo DOI: 10.5281/zenodo.10775264。有 Travis CI | 2024-03-03 |
| **[CINPLA/edNEGmodel_analysis](https://github.com/CINPLA/edNEGmodel_analysis)** | Python | 复现 Sætra et al. (2021) 全部结果。需 v1.0.0。`bash run_all.sh` → **在普通电脑上要跑好几天** | 2023-11-17 |
| **[letiziasignorelli/edNEGmodel_UQSA](https://github.com/letiziasignorelli/edNEGmodel_UQSA)** | Python | Signorelli et al. (2024) 的 UQ/GSA 代码。需 **v2.0.0**。Windows 10 + Python 3.8.16 | 2024-07-29 |
| **[ModelDBRepository/267116](https://github.com/ModelDBRepository/267116)** | Python | ModelDB 收录条目（Sætra et al. 2021），便于与其他模型对比 | 2024-01-13 |
| **[MakotoMiyakoshi/cableTheoryDemo](https://github.com/MakotoMiyakoshi/cableTheoryDemo)** | MATLAB | 电缆理论演示（教学向，最简洁的入手点） | 2025-09-18 |
| **[F10r1an/PNPSolverSpine](https://github.com/F10r1an/PNPSolverSpine)** | Jupyter | *Parameters of Cable Theory Are Mostly Unaffected by the Geometry of Dendritic Spines* —— 树突棘几何对电缆参数的影响 | 2024-04-15 |
| **[DanTurner-Evans/SingleNeuronSimulations](https://github.com/DanTurner-Evans/SingleNeuronSimulations)** | Jupyter | 果蝇单神经元电缆模型，从 FIBSEM 电镜数据集提取 | 2020-07-02 |

### ⚠️ 官方勘误（论文里已标注）

> Sætra et al. 2021 的 **Table 1** 写胞外横截面积 A_e = 3.08×10⁻¹¹ m²，
> **但模拟里实际用的是 6.16×10⁻¹¹ m²**（差了整整一倍）。
> 由 Eirill Hauge 和 Letizia Signorelli 发现。引用参数表时注意。

### 运行环境

- 核心库：Python 3.6（`python3 setup.py install`）
- 仓库用 **git LFS** 存 `initial_values.npz`，拉不下来要先 `git-lfs pull`
- UQSA 需要 edNEGmodel **v2.0.0**（不是 v1.0.0）
- `edNEGmodel.py` 单文件 221 KB、`edNEGmodel_params.py` 223 KB —— 参数表塞得很满

---

## 五、关键文献

### Cable theory

1. **Rall, W. (1959).** Branching dendritic trees and motoneuron membrane resistivity. *Experimental Neurology*, 1(5), 491–527. —— **奠基作，稳态问题**
2. **Rall, W. (1960).** Membrane potential transients and membrane time constant of motoneurons. *Experimental Neurology*, 2(5), 503–532. —— 瞬态问题
3. **Rall, W. (1962).** Theory of physiological properties of dendrites. *Annals of the New York Academy of Sciences*, 96, 1071–1092.
4. **Rall, W. (1977).** Core conductor theory and cable properties of neurons. In *Handbook of Physiology*, Vol. 1, 39–97.
5. **Segev I, Rinzel J, Shepherd GM (1995).** *The Theoretical Foundation of Dendritic Function: Selected Papers of Wilfrid Rall with Commentaries.* MIT Press. —— **想一次读透，读这本**
6. **Koch, C. (1999).** *Biophysics of Computation: Information Processing in Single Neurons.* Oxford UP. —— 教材级
7. **Dayan P & Abbott LF (2001).** *Theoretical Neuroscience*, Ch. 6. MIT Press. —— 电缆方程的标准教科书推导
8. **Sterratt D, Graham B, Gillies A, Willshaw D.** *Principles of Computational Modelling in Neuroscience.* Cambridge UP. —— 上手做建模的首选
9. Scholarpedia: **"Rall model"** 条目 —— 免费、含史料（http://www.scholarpedia.org/article/Rall_model）

### edNEG

1. **Sætra MJ, Einevoll GT, Halnes G (2021).** PLoS Comput Biol 17(7): e1008143. DOI 10.1371/journal.pcbi.1008143 —— **必读**
2. **Signorelli L, Manzoni A, Sætra MJ (2024).** Uncertainty quantification and sensitivity analysis of neuron models with ion concentration dynamics. *PLoS ONE* 19(5): e0303822. DOI 10.1371/journal.pone.0303822
3. **Signorelli L (2023).** *Efficient Uncertainty Quantification and Sensitivity Analysis of Electrodiffusive Neuron Models.* 米兰理工博士论文 —— 含 edNEG 完整数学推导（附录 A/B 有全部参数），**免费全文**，是读懂 2021 论文的最好辅助
4. Sætra et al. *edNEG model with somatodendritic interactions* —— 扩展版

---

## 六、与你的研究的关联

先说判断：**这两个都离你的日常（临床语言学、行为实验）比较远，属于"需要时才深挖"的层级。** 但如果落到具体研究上，有两处接得上：

### 接点 1：慢电位 ↔ fNIRS / BOLD

你手上有 fNIRS 数据（PFC / TPJ，HbO/HbR）。edNEG 处理的正是**慢信号**：作者明确指出常规电生理记录把 0.1–1 Hz 以下的成分滤掉了，而这一块恰恰和血氧动力学的时间尺度重叠。

链条是这样的：

```
神经元活动 → K⁺ 大量外流 → 胞外 K⁺ 浓度上升 → 胶质细胞 K⁺ 缓冲（消耗能量）
                                              ↓
                                    血流/血氧响应（神经血管耦合）
                                              ↓
                                          fNIRS 信号
```

**edNEG 把"胶质 K⁺ 缓冲"这一环显式建模了**，而标准模型忽略了它。如果你的研究要论证 fNIRS 信号和神经活动的关系，这是一条可以引的机制链。Sætra 本人的另一条线（CMRO₂ 皮层分层建模）更直接 —— 就是为 BOLD fMRI 信号解释服务的。

### 接点 2：听觉/言语节律 ↔ 离子浓度动力学

你在听觉与言语节律这条线上。节律性刺激下，**离子浓度本身会以慢时间尺度涨落**（edNEG 的核心变量），这提供了"节律刺激 → 慢电位 → 神经血管响应"的机制解释。而要谈这个，edNEG 是目前少数能算慢电位的模型。

### 接点 3（写专著时可能用得上）

Cable theory 是**生物物理层面的经典**。如果《临床语言学》里有"神经机制"性质的章节要交代"神经信号如何在单个神经元层面传播"，Rall 1959 是绕不过去的一笔（而且它是**理论先于实验**的典型案例，科普性好）。edNEG 层次太深，除非专门写"神经信号生成建模"，否则不必进正文。

### 临床相关的一点

edNEG 的扩展动机之一是**扩散性抑制（spreading depression）**这类病理条件 —— 离子浓度剧变、胞外电位梯度巨大、胶质缓冲过程。这类机制与偏头痛先兆、癫痫、脑缺血相关，而**失语症与癫痫/脑损伤的语言障碍**是你失语症康复那条线上的内容。要深挖，这是接口。

---

## 七、一句话总结

- **Cable theory**：把神经纤维当漏电电缆，解出 V(x,t)。**1959 年 Rall 提出，是树突计算和房室建模的地基。**
- **edNEG**：在电缆/多房室模型上加电扩散，把脑组织拆成**神经元 / 胞外 / 胶质**三个域，追踪 Na⁺ K⁺ Ca²⁺ Cl⁻ 四种离子的浓度与电位，共 34 个 ODE。**2021 年 Sætra 等人提出，是目前少数能算脑内慢电位的模型。**
- **关系**：前者是后者的神经元侧基础；后者是前者向组织层面的扩展。
