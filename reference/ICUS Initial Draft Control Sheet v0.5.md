# ICUS Initial Draft Control Sheet v0.5

> 本文档是 ICUS 会议论文初稿前的控制蓝图。它不替代 `论文故事与创新点梳理.md`、`Introduction论证蓝图.md`、`Method论证链.md` 和 `实验链与证据链梳理v3.md`，而是把这些文档压缩成一份可以直接指导初稿写作的执行表。本文档的重点是控制论文结构、符号系统、公式深度、贡献表述、图表槽位、claim 边界和实验证据合同。

---

## 0. 依据材料与当前状态

### 0.1 核心文档

| 文档路径 | 当前作用 |
|---|---|
| `D:\PycharmProjects\ICUS\reference\论文故事与创新点梳理.md` | 论文主线与创新点来源，定义 ICUS 论文的故事：主线协作有代价，gap 是可协作发展的资源。 |
| `D:\PycharmProjects\ICUS\reference\Introduction论证蓝图.md` | Introduction 的问题链、Related Work 分类矩阵和 gap 表述来源。 |
| `D:\PycharmProjects\ICUS\reference\Method论证链.md` | Method 章节的逻辑链和小节结构来源。 |
| `D:\PycharmProjects\ICUS\reference\实验链与证据链梳理v3.md` | 当前 ICUS 实验章节的正式指导文档，取代 v2。 |
| `D:\PycharmProjects\CORMC\docs\one_step_algorithm\公式整理与分块.md` | 符号、公式链、闭式解、评分函数和轨迹生成的主要依据。 |
| `D:\PycharmProjects\ICUS\reference\S01场景算法对比分析报告.md` | S01 外部算法对比的已有数据和解释依据。 |

### 0.2 当前阶段判断

当前不是继续判断材料是否足够的阶段，而是进入初稿控制阶段。论文已经有稳定主线：

```text
匝道合流的关键不只是 MV 能否进入 gap，
而是为了让 MV 安全进入 gap，主线交通需要付出多少协作代价。

本文将候选 gap 视为可协作发展的交通资源，
先估计边界车辆在相邻安全余量约束下能贡献多少协作，
再用空间敏感二次优化估计最小必要协作调整，
随后用主线协作代价和 MV 自车代价联合评分选择 developed gap，
最后把选中 gap 转化为合流时间、合流位置和可行轨迹。
```

### 0.3 v0.5 的含义

`v0.5` 是初稿前控制版，锁定以下内容：

```text
论文结构
页数预算
三条贡献
符号表
假设边界
公式进入正文的层级
图表需求
实验证据合同
claim register
最小 citation sentence map
写作顺序
```

`v0.5` 不是最终规范。升级到 `v1` 的条件是：

```text
1. Problem Formulation + Method skeleton 已经写成初稿；
2. S01 feasibility 表、C 节机制表、D 节外部对比候选场景数据结构已经明确；
3. 图表最终数量基本确定；
4. 引文句子已经对应到真实文献 key。
```

---

## 1. Paper Identity

### 1.1 目标会议与篇幅

目标会议：ICUS 2026。

格式：IEEE full paper。

目标篇幅：

```text
不少于 5 页；
最好不超过 6 页；
正文、图表、参考文献共同挤占页面空间。
```

写作策略：

```text
不写成大而全系统报告；
不展开论文2内容；
不设置单独 Discussion；
Related Work 压缩进 Introduction；
Method 和 Experiments 是论文主体。
```

### 1.2 一句话论文主张

中文主张：

> 本文提出一种主线扰动感知的 cooperative gap development 方法，将候选 gap 的安全形成过程建模为有约束、有代价的局部主线协作资源分配问题，并通过相邻余量约束、空间敏感协作调整、扰动-效率联合评分和可行轨迹生成，实现低主线扰动的匝道合流决策。

英文写作时的方向：

```text
mainline-disturbance-aware cooperative gap development
developed candidate gap
adjacent-gap-preserving cooperation capacity
space-sensitive minimum-disturbance adjustment
disturbance-efficiency valuation
```

英文初稿中可以使用这些术语，但当前控制文档以中文解释为主。

---

## 2. Final Section Structure

最终论文结构采用五节：

```text
I. Introduction
II. Problem Formulation
III. Mainline-Disturbance-Aware Cooperative Gap Development
IV. Experiments
V. Conclusion
```

不单独设置 `Related Work`。原因：

```text
ICUS 篇幅短；
Related Work 的任务只是支撑 Introduction 中的 gap；
单独 Related Work 容易把论文拖成综述；
本文最需要篇幅的是符号、公式链和实验表。
```

### 2.1 I. Introduction

功能：

```text
1. 说明匝道合流的本质矛盾是主线协作代价；
2. 说明已有 CAV merging、trajectory optimization、gap selection、gap creation 方法的不足；
3. 引出本文视角：gap 不是静态找到的，而是在有限主线协作下发展出来的；
4. 给出三条贡献；
5. 简短说明实验验证结构。
```

建议段落数：

```text
4 到 5 段。
```

段落功能：

| 段落 | 功能 |
|---|---|
| P1 | 背景与本质矛盾：合流不只是 MV 进入 gap，而是主线交通要付出多少代价。 |
| P2 | 现有方法图谱：CAV 合流协调、轨迹优化、gap selection、gap creation。 |
| P3 | 隐性缺口：现有方法较少把 bounded mainline cooperation cost 作为候选 gap 评价核心。 |
| P4 | 本文重定义：cooperative gap development and valuation。 |
| P5 | 三条贡献与实验概览。 |

### 2.2 II. Problem Formulation

功能：

```text
定义局部单帧合流决策问题；
定义 candidate gaps、required gap、adjacent safety gap、boundary adjustment；
给出本文要解决的抽象问题；
声明本文范围和假设。
```

写作重点：

```text
不展开可达集搜索；
不写长时域 rolling；
不写多 MV；
不写 SUMO 闭环；
不写 CAV/HDV 不确定响应。
```

可用一句话轻轻带过：

```text
At a merging decision instant, the planner evaluates candidate gaps in the target mainline stream.
```

中文含义是：在一次合流决策时刻，算法评价主线车流中的候选 gap。不要过度强调“给定若干局部候选 gap”。

### 2.3 III. Mainline-Disturbance-Aware Cooperative Gap Development

建议小节：

```text
A. Adjacent-Gap-Preserving Cooperation Capacity
B. Space-Sensitive Minimum-Disturbance Adjustment
C. Disturbance-Efficiency Valuation for Developed Gaps
D. Terminal Time, Merging Location, and Feasible Trajectory
```

如果 Method 篇幅紧张，可以把 C 和 D 合并为：

```text
C. Developed-Gap Valuation and Trajectory Execution
```

但首选仍是四个小节，因为公式链更清楚。

### 2.4 IV. Experiments

采用 v3 实验结构：

```text
A. Experimental Setup
B. Feasibility Validation
C. Capacity and Adjustment Ablation
D. External Algorithm Comparison in Representative Scenarios
```

不设置 `E. Discussion`。

### 2.5 V. Conclusion

功能：

```text
1. 重申主线协作不是免费资源；
2. 总结本文方法链；
3. 简短总结实验发现；
4. 一句话说明后续工作：更复杂交通、闭环仿真、混行场景等。
```

不能把论文2内容写成当前论文的未完成缺陷。应写成自然扩展：

```text
Future work will extend the local longitudinal gap-development model to denser and mixed traffic conditions with closed-loop simulation.
```

正式英文初稿时再润色。

---

## 3. Page Budget

目标总篇幅：5.5 到 6 页。

参考页数分配：

| 部分 | 目标篇幅 | 控制要求 |
|---|---:|---|
| Title + Abstract + Keywords | 0.20 到 0.25 页 | 摘要 150 到 180 词，不写过多背景。 |
| I. Introduction | 0.75 到 0.90 页 | Related Work 压缩在其中。 |
| II. Problem Formulation | 0.45 到 0.55 页 | 符号和假设要清楚，但不堆满公式。 |
| III. Method | 1.60 到 1.85 页 | 公式链核心。必须优先保障。 |
| IV. Experiments | 1.50 到 1.85 页 | 三个实验小节，图表紧凑。 |
| V. Conclusion | 0.18 到 0.25 页 | 不写长 discussion。 |
| References | 0.45 到 0.70 页 | 目标 12 到 18 篇左右，避免堆引用。 |

若超页，删减顺序：

```text
1. 删除 Introduction 中过细的 Related Work 分类；
2. 压缩实验解释段落；
3. 把外部对比场景从 4 个减到 3 个；
4. 删除可选图，不删核心公式；
5. 删除不必要的速度/加速度小图，保留表格。
```

不得优先删除：

```text
相邻 gap capacity 公式；
space-sensitive QP；
developed-gap score；
selected-gap trajectory execution 公式；
candidate score table；
external comparison table。
```

---

## 4. Three Contributions

贡献点压缩为三条。三条贡献不是把原来的六个点机械合并，而是按论文逻辑分层：

```text
贡献 1：问题视角
贡献 2：协作发展机制
贡献 3：选择与执行链
```

### 4.1 贡献 1：主线扰动感知的 cooperative gap development 问题表述

中文表述：

> 提出一种主线扰动感知的 cooperative gap development 问题表述，将匝道合流中的候选 gap 从静态选择对象转化为可协作发展的交通资源，并显式把边界车辆协作视为有限且有代价的主线资源。

它融合的原始创新点：

```text
主线协作不是免费资源；
gap 不是静态找到的；
候选 gap 评价应考虑发展成安全 gap 所需的最小主线协作代价。
```

写作边界：

```text
可以说：本文重新表述局部匝道合流 gap 选择问题。
不要说：本文提出完整交通系统级全局调度框架。
```

### 4.2 贡献 2：相邻余量约束下的空间敏感最小扰动 gap development 机制

中文表述：

> 提出一种相邻 gap 余量约束下的空间敏感协作调整机制。该机制先由候选 gap 前后相邻空间计算边界车辆的最大安全协作能力，再通过空间敏感二次优化将 gap shortage 分配给前后边界车辆，从而使调整负担自动转向空间更充足的一侧。

它融合的原始创新点：

```text
adjacent-gap-preserving cooperation capacity；
space-sensitive adjustment weights；
minimum-disturbance convex QP；
closed-form projected solution。
```

写作边界：

```text
可以说：该机制验证了预期行为，即根据相邻空间余量分配调整。
不要说：该 QP 已经通过大规模消融证明在所有交通情况下全局最优。
```

### 4.3 贡献 3：扰动-效率联合 developed-gap valuation 与可执行轨迹生成

中文表述：

> 提出一种扰动-效率联合的 developed-gap valuation 方法，将每个候选 gap 的最小必要主线协作代价与 MV 自车合流代价统一评分，选择发展后的最优 gap，并进一步由协作后 gap 中心确定合流时间、合流位置和满足速度/加速度约束的闭式可行轨迹。

它融合的原始创新点：

```text
disturbance-efficiency score；
best developed gap selection；
cooperation-induced gap-center shift；
terminal time and terminal location determination；
quintic feasible trajectory generation。
```

写作边界：

```text
可以说：本文在评分意义下选择 best developed gap。
不要说：本文证明该选择在真实交通全局效率上最优。
```

### 4.4 贡献段落最终形态

Introduction 贡献段建议只写三条 bullet。中文逻辑如下：

```text
1. 提出主线扰动感知的 cooperative gap development 问题表述，将候选 gap 视为可协作发展的局部交通资源，而不是仅按当前长度或距离静态选择。

2. 提出相邻 gap 余量约束下的空间敏感最小扰动协作调整机制，通过状态相关协作容量和二次优化估计每个候选 gap 的最小必要主线协作代价。

3. 提出扰动-效率联合的 developed-gap valuation 与可执行轨迹生成链，将最优 developed gap 转化为合流时间、合流位置和满足速度/加速度约束的 MV 轨迹，并通过代表性场景验证低主线扰动效果。
```

---

## 5. Notation Register

Notation Register 是本论文的关键控制点。初稿中必须统一符号，不能在 `story`、`Method` 和代码文档之间来回换名。

### 5.1 场景与车辆状态

| 符号 | 含义 | 正文使用建议 |
|---|---|---|
| \(X=[x_1,\ldots,x_N]\) | 目标主线车道车辆初始纵向位置，按位置升序排列 | Problem Formulation 中定义 |
| \(x_m(0)\) | MV 初始纵向位置 | Problem Formulation 中定义 |
| \(v_{ref}\) | 主线参考速度，主线车辆初始按该速度匀速运行 | Problem Formulation 中定义 |
| \(v_m(t)\) | MV 速度 | trajectory 公式中使用 |
| \(a_m(t)\) | MV 加速度 | trajectory 公式中使用 |
| \(v_{\min}, v_{\max}\) | MV 速度下界和上界 | Method D 或 Problem Formulation 中定义 |
| \(a_{\min}, a_{\max}\) | MV 加速度下界和上界 | Method D 或 Problem Formulation 中定义 |
| \(a_{\lim}\) | 统一保守加速度幅值，\(a_{\lim}=\min(a_{\max},|a_{\min}|)\) | 合流时间约束中使用 |
| \(T\) | 候选 gap 运动学可达时间窗 | 不作为贡献展开，只作为候选筛选条件 |

注意：

```text
可达性不是 ICUS 论文贡献，不要把 \(T\)、\(t_{reach,i}\)、\(\mathcal{G}_{reach}\) 写成重点。
必要时只说候选 gap 需满足基本运动学可达条件。
```

### 5.2 Gap 定义

| 符号 | 含义 | 正文使用建议 |
|---|---|---|
| \(i\) | candidate gap index | 全文统一 |
| \(\mathrm{gap}_i=[x_{r,i},x_{f,i}]\) | 第 \(i\) 个候选 gap，由后边界车和前边界车定义 | Problem Formulation 中定义 |
| \(x_{r,i}\) | candidate gap 后边界车辆位置 | Method 中使用 |
| \(x_{f,i}\) | candidate gap 前边界车辆位置 | Method 中使用 |
| \(G_i=x_{f,i}-x_{r,i}\) | candidate gap 当前长度 | 必须定义 |
| \(c_i=(x_{r,i}+x_{f,i})/2\) | candidate gap 初始中心 | 必须定义 |
| \(D_h\) | 安全车头间距参数 | Setup 或 Problem Formulation 中定义 |
| \(l_m\) | MV 车辆长度 | Setup 或 Problem Formulation 中定义 |
| \(G^{req}=2D_h+l_m\) | MV 安全合流所需目标 gap 长度 | 必须定义 |
| \(G^{adj}=D_h+l_m\) | 相邻 gap 保护阈值 | 必须定义 |
| \(\Delta_i=\max(0,G^{req}-G_i)\) | candidate gap shortage | Method 核心公式 |

建议写法：

```text
\(G^{req}\) 是目标 gap 需要达到的合流安全长度；
\(G^{adj}\) 是边界车辆调整时不能压缩相邻 gap 低于的安全长度。
```

### 5.3 相邻 gap 余量与协作容量

| 符号 | 含义 | 正文使用建议 |
|---|---|---|
| \(G_{i-1}\) | candidate gap 后方相邻 gap 长度 | Method A 中定义 |
| \(G_{i+1}\) | candidate gap 前方相邻 gap 长度 | Method A 中定义 |
| \(G_{bound}\) | 边界虚拟 gap 长度 | 不进主文，或仅 footnote/implementation note |
| \(\bar{\delta}_{f,i}\) | 前边界车最大可前移量 | Method A 核心 |
| \(\bar{\delta}_{r,i}\) | 后边界车最大可后移量 | Method A 核心 |
| \(\mathcal{G}_{coop}\) | 可通过边界车辆协作发展为安全 gap 的候选集合 | 可定义但不要过度强调集合构造 |

核心公式：

```tex
\bar{\delta}_{f,i}=\max(0,G_{i+1}-G^{adj})
```

```tex
\bar{\delta}_{r,i}=\max(0,G_{i-1}-G^{adj})
```

```tex
\bar{\delta}_{f,i}+\bar{\delta}_{r,i}\geq \Delta_i
```

写作解释：

```text
前边界车前移会压缩它前方的相邻 gap，因此前移能力由前方相邻 gap 的剩余安全余量决定。
后边界车后移会压缩它后方的相邻 gap，因此后移能力由后方相邻 gap 的剩余安全余量决定。
```

### 5.4 空间敏感权重与协作调整

| 符号 | 含义 | 正文使用建议 |
|---|---|---|
| \(\delta_{f,i}\) | candidate gap 前边界车前移调整量 | QP 变量 |
| \(\delta_{r,i}\) | candidate gap 后边界车后移调整量 | QP 变量 |
| \(\delta_{f,i}^*\) | candidate-level 最优前边界调整估计 | 评分前计算，实际只对 selected gap 执行 |
| \(\delta_{r,i}^*\) | candidate-level 最优后边界调整估计 | 评分前计算，实际只对 selected gap 执行 |
| \(\delta_{ref}\) | 空间敏感权重参考调整尺度 | Setup 或 Method 中定义 |
| \(q\) | 空间敏感指数 | Setup 或 Method 中定义 |
| \(\gamma_{f,i}\) | 前边界调整权重 | QP 核心 |
| \(\gamma_{r,i}\) | 后边界调整权重 | QP 核心 |
| \(\varepsilon_\delta\) | 小调整量死区阈值 | 不进主文，除非需要解释 S07/S08 精确 0 调整 |

核心公式：

```tex
\gamma_{f,i}=\left(\frac{\delta_{ref}}{\bar{\delta}_{f,i}}\right)^q,\qquad
\gamma_{r,i}=\left(\frac{\delta_{ref}}{\bar{\delta}_{r,i}}\right)^q.
```

核心 QP：

```tex
\min_{\delta_{f,i},\delta_{r,i}}
C_i^{coop}
=
\gamma_{f,i}\delta_{f,i}^2+
\gamma_{r,i}\delta_{r,i}^2
```

subject to:

```tex
\delta_{f,i}+\delta_{r,i}\geq \Delta_i,\quad
0\leq\delta_{f,i}\leq\bar{\delta}_{f,i},\quad
0\leq\delta_{r,i}\leq\bar{\delta}_{r,i}.
```

因为代价随调整量增大而增大，最优解满足：

```tex
\delta_{f,i}+\delta_{r,i}=\Delta_i.
```

闭式投影解建议进入 Method 正文，因为它能体现方法轻量、可解释、不依赖数值求解器。

```tex
L_i=\max(0,\Delta_i-\bar{\delta}_{r,i}),\qquad
U_i=\min(\bar{\delta}_{f,i},\Delta_i)
```

```tex
\delta_{f,i}^{raw}
=
\frac{\gamma_{r,i}}{\gamma_{f,i}+\gamma_{r,i}}\Delta_i
```

```tex
\delta_{f,i}^*=\Pi_{[L_i,U_i]}(\delta_{f,i}^{raw}),\qquad
\delta_{r,i}^*=\Delta_i-\delta_{f,i}^*.
```

### 5.5 Developed gap 中心、合流时间和合流位置

| 符号 | 含义 | 正文使用建议 |
|---|---|---|
| \(\Delta c_i\) | candidate gap 协作后中心偏移 | 可定义 |
| \(d_i\) | 协作后 gap 中心相对 MV 参考匀速轨迹的目标位移 | Method D 核心连接变量 |
| \(S(\tau)\) | 五次平滑函数 | trajectory 公式核心 |
| \(K=120/7\) | 五次函数加速度能量系数 | \(C_i^{ego}\) 中使用 |
| \(t_{m,i}^0\) | 无约束能量-时间折中合流时间 | Method D 中可写 |
| \(t_{a,i}\) | 加速度约束给出的时间下界 | Method D 中可写 |
| \(t_{v,i}\) | 速度约束给出的时间下界 | Method D 中可写 |
| \(t_{m,i}^*\) | candidate gap 的最终合流时间 | 核心 |
| \(p_{m,i}\) | candidate gap 的合流位置 | 核心 |

中心偏移：

```tex
\Delta c_i=\frac{\delta_{f,i}^*-\delta_{r,i}^*}{2}
```

目标位移：

```tex
d_i=c_i+\frac{\delta_{f,i}^*-\delta_{r,i}^*}{2}-x_m(0)
```

五次平滑函数：

```tex
S(\tau)=10\tau^3-15\tau^4+6\tau^5,\qquad \tau=\frac{t}{t_m}.
```

时间计算：

```tex
t_{m,i}^{0}=\left(\frac{3Kd_i^2}{w_t}\right)^{1/4}
```

```tex
t_{a,i}=
\sqrt{
\frac{\frac{10}{\sqrt{3}}|d_i|}{a_{\lim}}
}
```

```tex
t_{v,i}
=
\begin{cases}
\dfrac{1.875|d_i|}{v_{\max}-v_{ref}}, & d_i\geq 0,\\
\dfrac{1.875|d_i|}{v_{ref}-v_{\min}}, & d_i<0.
\end{cases}
```

```tex
t_{m,i}^*=\max(t_{m,i}^{0},t_{a,i},t_{v,i})
```

合流位置：

```tex
p_{m,i}=x_m(0)+v_{ref}t_{m,i}^*+d_i
```

写作解释：

```text
如果 \(\delta_{f,i}^*>\delta_{r,i}^*\)，协作后 gap 中心前移，MV 目标位移增加。
如果 \(\delta_{r,i}^*>\delta_{f,i}^*\)，协作后 gap 中心后移，MV 目标位移降低。
```

### 5.6 评分与选择

| 符号 | 含义 | 正文使用建议 |
|---|---|---|
| \(C_i^{coop}\) | candidate gap 的最小必要主线协作代价 | Method C 核心 |
| \(C_i^{ego}\) | MV 自车合流代价 | Method C 核心 |
| \(w_c,w_e,w_t\) | 协作代价、自车代价、时间代价权重 | Setup 或 Method 中定义 |
| \(J_i\) | developed candidate gap 的总评分 | Method C 核心 |
| \(i^*\) | selected developed gap index | Method C/D 核心 |

协作代价：

```tex
C_i^{coop}
=
\gamma_{f,i}(\delta_{f,i}^*)^2
+
\gamma_{r,i}(\delta_{r,i}^*)^2
```

自车代价：

```tex
C_i^{ego}
=
\frac{K d_i^2}{(t_{m,i}^*)^3}
+
w_t t_{m,i}^*
```

总评分：

```tex
J_i=w_c C_i^{coop}+w_e C_i^{ego}
```

选择：

```tex
i^*=\arg\min_{i\in\mathcal{G}_{coop}}J_i
```

关键表述：

```text
评分动作在前，选择在后，执行最后。
所有候选 gap 只计算 candidate-level estimate。
实际物理调整只对 \(i^*\) 执行。
```

### 5.7 Selected gap 轨迹

只对 \(i=i^*\) 生成完整轨迹。

MV 轨迹：

```tex
x_m(t)=x_m(0)+v_{ref}t+d^*S\left(\frac{t}{t_m^*}\right)
```

```tex
v_m(t)=v_{ref}+\frac{d^*}{t_m^*}S'\left(\frac{t}{t_m^*}\right)
```

```tex
a_m(t)=\frac{d^*}{(t_m^*)^2}S''\left(\frac{t}{t_m^*}\right)
```

后边界车：

```tex
x_r(t)=x_r^*+v_{ref}t-\delta_r^*S\left(\frac{t}{t_m^*}\right)
```

前边界车：

```tex
x_f(t)=x_f^*+v_{ref}t+\delta_f^*S\left(\frac{t}{t_m^*}\right)
```

动态 gap 长度：

```tex
G^*(t)=G_{i^*}+(\delta_f^*+\delta_r^*)S\left(\frac{t}{t_m^*}\right)
```

终点：

```tex
G^*(t_m^*)=G_{i^*}+\delta_f^*+\delta_r^*\geq G^{req}
```

动态中心：

```tex
c^*(t)=c_{i^*}+v_{ref}t+\frac{\delta_f^*-\delta_r^*}{2}S\left(\frac{t}{t_m^*}\right)
```

终点中心：

```tex
c^*(t_m^*)=p_m^*
```

这几条公式可以压缩写，不必全部展开速度和加速度。如果篇幅紧张，主文保留 \(x_m(t)\)、\(x_r(t)\)、\(x_f(t)\)、\(G^*(t_m^*)\) 和 \(c^*(t_m^*)=p_m^*\)。

---

## 6. Assumptions and Scope Boundaries

### 6.1 必须声明的假设

| 假设 | 正文写法 |
|---|---|
| 局部单 MV 决策 | 本文研究一个 MV 在一次合流决策时刻的局部 gap development and selection 问题。 |
| 主线目标车道车辆位置可获得 | 车辆位置由 CAV 感知/通信或仿真场景给出。 |
| 主线车辆初始以 \(v_{ref}\) 行驶 | 用于建立解析轨迹和低扰动比较。 |
| 边界车辆可协作 | ICUS 版本默认候选 gap 的边界车辆为可控 CAV，HDV 不确定响应留给后续论文。 |
| 只考虑纵向协作调整 | 不考虑主线车辆向内侧换道构造 gap。 |
| 候选 gap 通过基本运动学可达检查 | 可达性不是贡献，只是避免评价物理上到不了的 gap。 |
| 安全长度由 \(G^{req}\) 和 \(G^{adj}\) 表示 | 一个用于目标 gap 合流安全，一个用于相邻 gap 保护。 |

### 6.2 不进入当前论文的内容

```text
多 MV rolling coordination
长匝道 action set
主线车向内侧换道辅助构造 gap
随机车流统计验证
SUMO 闭环仿真
CAV/HDV 混行和 HDV 服从概率建模
密集主线无纵向 gap 可发展时的 hybrid gap supply
```

这些内容不写成当前论文缺陷，而写成后续扩展方向。

---

## 7. Method Formula Depth

公式深度采用三级控制：

```text
Level 1: 必须进入正文。
Level 2: 根据篇幅和可读性选择进入正文或压缩为文字。
Level 3: 不进入正文，只作为实现细节或后续论文内容。
```

### 7.1 Level 1 必须进入正文

这些公式构成 ICUS 论文的理论骨架。

| 公式块 | 必须程度 | 原因 |
|---|---|---|
| candidate gap 定义 \(G_i,c_i\) | 必须 | Problem Formulation 基础。 |
| required gap 与 adjacent gap \(G^{req},G^{adj}\) | 必须 | 区分目标 gap 安全和相邻 gap 保护。 |
| gap shortage \(\Delta_i\) | 必须 | 后续 capacity 和 QP 的输入。 |
| 相邻余量容量 \(\bar{\delta}_{f,i},\bar{\delta}_{r,i}\) | 必须 | 贡献 2 的核心。 |
| 协作可行性 \(\bar{\delta}_{f,i}+\bar{\delta}_{r,i}\geq\Delta_i\) | 必须 | 说明不足 gap 是否可被发展。 |
| 空间敏感权重 \(\gamma_{f,i},\gamma_{r,i}\) | 必须 | 解释为什么调整会偏向空间充足侧。 |
| QP 目标与约束 | 必须 | Method 核心优化问题。 |
| 闭式投影解 | 强烈建议进入 | 体现方法轻量、解析、可解释。 |
| developed center offset \(d_i\) | 必须 | 连接 gap development 和 MV trajectory。 |
| \(t_{m,i}^*,p_{m,i}\) | 必须 | 连接 score 和可执行轨迹。 |
| \(C_i^{coop},C_i^{ego},J_i\) | 必须 | 贡献 3 的核心。 |
| \(i^*=\arg\min J_i\) | 必须 | 明确 best developed gap selection。 |
| selected-gap trajectory \(x_m(t),x_r(t),x_f(t)\) | 必须但可压缩 | 证明只对选中 gap 执行。 |

### 7.2 Level 2 可选进入正文

| 公式块 | 进入条件 | 处理方式 |
|---|---|---|
| reachability time \(t_{reach,i}\) 前后分支 | 若 Problem Formulation 需要解释候选筛选 | 不展开完整推导，只用一句话说明基本运动学可达检查。 |
| \(S'(\tau),S''(\tau)\) 完整导数 | 若要证明速度/加速度约束 | 可只给 \(S(\tau)\)、\(S'_{\max}=1.875\)、\(\max|S''|=10/\sqrt{3}\)。 |
| \(t_{a,i},t_{v,i}\) | 建议进入 Method D | 可以压缩成一行 \(\max(\cdot)\) 公式。 |
| \(G^*(t),c^*(t)\) | 若轨迹执行解释不够清楚 | 保留终点性质即可。 |
| Proposition 或 Property | 若篇幅允许 | 写一个短命题说明 QP 闭式解，不写长证明。 |

### 7.3 Level 3 不进入正文

| 内容 | 原因 |
|---|---|
| `gap_boundary_controllability` 四分支 | 当前 ICUS 默认 CAV 可控，不把 mixed traffic 写进来。 |
| 双侧不可控时 \(G_i>95m\) fallback | 工程实现细节，不适合论文主线。 |
| \(G_{bound}=G^{adj}+B_{bound}\) 边界虚拟 gap | 代码统一处理需要，主文可省略。 |
| \(\varepsilon_\delta\) 死区重分配完整规则 | 工程细节，除非需要解释实验表中的精确 0。 |
| tie-breaker 规则 | 实现细节，不属于论文贡献。 |
| rolling adapter、Stage2 summary、SUMO replay | 论文2或代码实现内容。 |

### 7.4 建议加入的理论性陈述

不建议写正式定理链。ICUS 篇幅短，写太多 theorem 会显得论文重心偏离工程方法。建议最多写一个短命题：

```text
Property 1: 对任意满足 \(\bar{\delta}_{f,i}+\bar{\delta}_{r,i}\geq\Delta_i\) 的候选 gap，空间敏感协作调整问题是凸二次规划；由于目标函数随调整量增大而增大，最优解满足 \(\delta_{f,i}+\delta_{r,i}=\Delta_i\)，并可由一维投影闭式求得。
```

正文处理方式：

```text
写成 prose + formula，不单独排 theorem 环境。
```

可选第二个性质：

```text
Property 2: 对 selected gap，五次平滑轨迹在终点满足 \(x_m(t_m^*)=p_m^*\)、\(v_m(t_m^*)=v_{ref}\)、\(a_m(t_m^*)=0\)，且 developed gap 长度满足 \(G^*(t_m^*)\geq G^{req}\)。
```

如果篇幅紧张，Property 2 不单独写，放在轨迹公式后的解释句。

---

## 8. Figure and Table Plan

图表计划必须服务 claim。不能为了好看放图，也不能通篇只有实验表。本文至少需要一张 Method 解释图和两到三张实验表。

### 8.1 总体图表预算

主文推荐图表：

| 编号 | 类型 | 必要性 | 放置位置 | 主要功能 |
|---|---|---|---|---|
| Fig. 1 | 方法概念图或两联图 | 必须 | Method 前半部分 | 解释 gap development、相邻余量、协作调整和算法流程。 |
| Table I | Candidate gap evaluation table | 必须 | Experiment B | 支撑 feasibility 和 valuation。 |
| Fig. 2 | S01 trajectory / time-space figure | 强烈建议 | Experiment B 或 D | 展示 selected developed gap 转化为实际轨迹。 |
| Table II | Capacity and adjustment ablation table | 必须 | Experiment C | 支撑 capacity + QP 机制。 |
| Table III | External comparison table | 必须 | Experiment D | 支撑 OneStep vs FIFO 低扰动结论。 |
| Fig. 3 | External disturbance bar chart | 可选 | Experiment D | 若表格太密，用图强化受影响车数、加速度能量、恢复时间。 |

如果页数超限，删除优先级：

```text
先删 Fig. 3；
再压缩 Fig. 2 的子图；
不删 Fig. 1；
不删 Table I/II/III，除非把 Table II 和 Table III 极度压缩。
```

### 8.2 Fig. 1 方法概念图

推荐形式：

```text
Fig. 1. Mainline-disturbance-aware cooperative gap development.
```

建议做成两联图：

```text
(a) Geometry and adjacent-gap-preserving capacities
(b) Candidate-level evaluation and selected-gap execution chain
```

Fig. 1(a) 内容：

```text
一条主线目标车道横向画成直线；
标出至少四辆主线车；
中间两辆形成 candidate gap i；
后边界车标为 \(x_{r,i}\)，前边界车标为 \(x_{f,i}\)；
当前 gap 长度标为 \(G_i\)；
目标安全长度标为 \(G^{req}\)；
前方相邻 gap 标为 \(G_{i+1}\)，后方相邻 gap 标为 \(G_{i-1}\)；
相邻安全阈值标为 \(G^{adj}\)；
前边界车向前箭头标为 \(\delta_{f,i}\)；
后边界车向后箭头标为 \(\delta_{r,i}\)；
可用协作容量标为 \(\bar{\delta}_{f,i}\)、\(\bar{\delta}_{r,i}\)；
协作后 developed gap 用半透明区域或虚线边界表示；
gap center shift 用 \(\Delta c_i\) 标出。
```

Fig. 1(a) 图例：

```text
MV: 合流车，用蓝色或黑色实心车辆图标；
Boundary CVs: selected candidate gap 的前后边界车，用橙色；
Other mainline vehicles: 灰色；
Original gap: 灰色括号或浅色区域；
Developed gap: 绿色或蓝色半透明区域；
Adjustment arrows: 实线箭头；
Protected neighboring margin: 红色或灰色虚线阈值。
```

Fig. 1(b) 内容：

```text
candidate gaps
-> adjacent-gap capacity
-> space-sensitive QP
-> \(C_i^{coop}\), \(C_i^{ego}\)
-> \(J_i\)
-> selected developed gap \(i^*\)
-> terminal time/location
-> trajectories
```

Fig. 1(b) 需要强调动作顺序：

```text
候选级估计和评分用于所有 candidate gaps；
实际协作调整和轨迹生成只对 selected gap 执行。
```

图中可用分隔线：

```text
Candidate-level estimation
Selected-gap execution
```

### 8.3 Table I Candidate gap evaluation and selection

放置位置：Experiment B。

数据来源：S01 feasibility 输出。

目标：

```text
证明本文不是选最近、最大、已有安全或 ego-only 最省力 gap；
而是先估计每个 candidate gap 的协作发展代价和自车代价，再选 best developed gap。
```

列设计：

| 列 | 含义 | 是否必须 |
|---|---|---|
| Gap | gap index 或 `[x_r,x_f]` | 必须 |
| \(G_i\) | 当前 gap 长度 | 必须 |
| \(\Delta_i\) | gap shortage | 必须 |
| \(\bar{\delta}_{f,i}/\bar{\delta}_{r,i}\) | 前后协作容量 | 必须 |
| \(\delta_{f,i}^*/\delta_{r,i}^*\) | candidate-level 调整估计 | 必须 |
| \(C_i^{coop}\) | 主线协作代价 | 必须 |
| \(C_i^{ego}\) | MV 自车代价 | 必须 |
| \(J_i\) | 总评分 | 必须 |
| Decision | selected / feasible / infeasible | 必须 |

如果表格过宽：

```text
\(\bar{\delta}_{f,i}/\bar{\delta}_{r,i}\) 合并为 `capacity`;
\(\delta_{f,i}^*/\delta_{r,i}^*\) 合并为 `adjustment`;
\(C_i^{coop}\)、\(C_i^{ego}\)、\(J_i\) 保留。
```

### 8.4 Fig. 2 S01 trajectory / time-space figure

放置位置：Experiment B 或 D。

首选放在 Experiment B，因为它服务 feasibility chain。

数据来源：

```text
S01 OneStep trajectory output；
如果与 FIFO 对比放在 D，则使用 comparison_trajectory.csv。
```

推荐形式：

```text
单栏或双栏 time-space plot。
```

图中元素：

```text
x-axis: time \(t\) in seconds；
y-axis: longitudinal position \(x\) in meters；
MV trajectory: 粗蓝线；
selected gap rear boundary vehicle: 橙色线；
selected gap front boundary vehicle: 红色线；
other mainline vehicles: 灰色细线；
developed gap region: selected rear/front boundary lines之间用浅色阴影；
terminal point: 标出 \((t_m^*,p_m^*)\)；
optional physical merge entry/exit: 若用于 D，可加 \(x=555m\)、\(x=600m\) 横线。
```

图例：

```text
MV；
selected rear boundary；
selected front boundary；
other mainline vehicles；
developed gap；
terminal merge point。
```

如果加入速度/加速度：

```text
只做小 inset，不另占一张图；
或在正文用一句话说明满足约束。
```

### 8.5 Table II Capacity and adjustment ablation

放置位置：Experiment C。

数据来源：

```text
S02, S06, S07, S08。
```

目标：

```text
证明 capacity + QP 机制符合预期行为：
无需协作时不扰动；
余量对称时近似对称调整；
前方余量更大时前边界车承担更多；
后方余量更大时后边界车承担更多。
```

列设计：

| 列 | 含义 |
|---|---|
| Case | C1 到 C4 |
| Scenario | S02/S06/S07/S08 |
| \(\Delta_i\) | gap shortage |
| Neighboring margins | 前后相邻余量，可写 `front/rear` |
| Equal split | \(\delta_f/\delta_r\) |
| Proposed | \(\delta_f^*/\delta_r^*\) |
| Min margin after proposed | proposed 调整后最小相邻安全余量 |
| Mechanism verified | no adjustment / symmetric / front preference / rear preference |

如果 S01 和 S07 完全重复：

```text
S07 需要改造成非重复 front-side-margin-larger variant；
不能把重复场景当两个独立证据。
```

### 8.6 Table III External comparison

放置位置：Experiment D。

数据来源：

```text
S01, S02, S03, S06, S07, S08 外部对比实验；
正文最终选 3 到 4 个场景。
```

目标：

```text
证明在代表性成功合流场景中，
OneStep 与 FIFO 物理合流入口/出口时间相近，
但 OneStep 主线扰动更小。
```

列设计：

| 列 | 含义 |
|---|---|
| Scenario | S01/S02/S03/... |
| Method | OneStep 或 FIFO |
| Entry time | MV 到达 \(x=555m\) 时间 |
| Exit time | MV 到达 \(x=600m\) 时间 |
| Affected mainline vehicles | 受影响主线车数 |
| Mainline acc. energy | 主线加速度能量 |
| Recovery time | 主线扰动恢复时间 |
| Min spacing | 最小车间距 |
| Min TTC | 最小 TTC |

如果表格过宽：

```text
Min spacing 和 Min TTC 合并为 Safety margin；
或者把 safety 指标只保留 S01 的正文解释。
```

S01 已有数据必须进入表：

```text
Entry time: OneStep 24.125s, FIFO 24.25s
Exit time: OneStep 26.375s, FIFO 26.5s
Affected vehicles: OneStep 1, FIFO 6
Mainline acceleration energy: OneStep 0.1706, FIFO 22.1881
Recovery time: OneStep 13.1s, FIFO 31.0s
Min spacing: OneStep 55.0m, FIFO 45.0m
Min TTC: OneStep 112.32s, FIFO 22.31s
```

### 8.7 Fig. 3 External disturbance bar chart

可选，不是必须。

若使用，推荐内容：

```text
x-axis: selected scenarios；
bar groups: OneStep vs FIFO；
metrics: affected vehicle count, mainline acceleration energy, recovery time。
```

由于三个指标量纲不同，建议做三联小图：

```text
(a) affected vehicle count
(b) mainline acceleration energy
(c) recovery time
```

图例：

```text
OneStep；
RiosTorres/FIFO。
```

如果加速度能量差异过大，可使用对数坐标，但 caption 必须说明。

若页面紧张，不画 Fig. 3，保留 Table III。

---

## 9. Experiment Evidence Contract

### 9.1 实验 B: Feasibility Validation

核心问题：

```text
本文方法能否从 candidate gap evaluation 闭环执行到 feasible trajectory？
```

场景：

```text
S01。
```

必须输出：

```text
candidate gap score table；
selected developed gap；
executed boundary adjustment；
\(t_m^*\)、\(p_m^*\)；
MV trajectory；
speed/acceleration constraint check。
```

支撑 claim：

```text
贡献 1；
贡献 3；
Claim C1: candidate gaps are developable resources；
Claim C2: selected developed gap can be converted into executable trajectory。
```

### 9.2 实验 C: Capacity and Adjustment Ablation

核心问题：

```text
adjacent-gap capacity 和 space-sensitive QP 是否符合预期机制行为？
```

场景：

```text
S02: no unnecessary cooperation；
S06: symmetric margins；
S07: front-side margin larger；
S08: rear-side margin larger。
```

主对比：

```text
Equal split vs Proposed。
```

可选对比：

```text
Unweighted QP。
```

必须输出：

```text
Delta；
front/rear neighboring margins；
equal split adjustment；
proposed adjustment；
minimum remaining neighboring-gap margin；
mechanism verified。
```

支撑 claim：

```text
贡献 2；
Claim C3: adjacent-gap margins bound cooperation capacity；
Claim C4: space-sensitive QP shifts adjustment toward larger-margin side。
```

写作限制：

```text
只说 compact ablation verifies intended behavior；
不说完整系统消融证明 proposed 在所有情况下最优。
```

### 9.3 实验 D: External Algorithm Comparison

核心问题：

```text
在代表性成功合流场景中，OneStep 是否能保持相近物理合流效率，同时减少主线扰动？
```

候选场景：

```text
S01, S02, S03, S06, S07, S08。
```

正文选择规则：

```text
S01 必须保留；
其余选 2 到 3 个；
OneStep 与 FIFO 都必须完成物理合流；
不能写重复场景；
不能按结果最漂亮单纯挑选；
选择标准是数据有效、机制覆盖、低扰动结论清楚。
```

主指标：

```text
physical merge entry time；
physical merge exit time；
affected mainline vehicle count；
mainline acceleration energy；
mainline speed recovery time。
```

安全解释指标：

```text
minimum spacing；
minimum TTC。
```

辅助解释指标：

```text
mainline average speed；
MV acceleration energy。
```

写作限制：

```text
可以说 representative successful merging scenarios；
不要说 all traffic scenarios；
不要说 globally optimal；
不要说 always outperforms FIFO。
```

---

## 10. Claim Register

| Claim ID | 中文 claim | 对应贡献 | 证据 | 允许写法 | 禁止写法 |
|---|---|---|---|---|---|
| C1 | 候选 gap 可以被视为可协作发展的交通资源，而不是只按当前长度静态选择。 | 贡献 1 | B 的 candidate score table | “candidate gaps are evaluated as developable resources” | “所有 gap 都会被实际发展后比较” |
| C2 | 边界车辆协作能力由相邻 gap 的剩余安全余量限制。 | 贡献 2 | C 的 S02/S06/S07/S08 | “cooperation capacity is bounded by neighboring-gap margins” | “主线车辆可任意调整” |
| C3 | 空间敏感 QP 会把调整负担转向余量更充足的一侧。 | 贡献 2 | C 的 proposed vs equal split | “ablation verifies intended allocation behavior” | “该 QP 在所有场景全局最优” |
| C4 | 扰动-效率联合评分可以选择 best developed gap。 | 贡献 3 | B 的 \(C_i^{coop}, C_i^{ego}, J_i\) 表 | “selects the best developed gap under the proposed score” | “真实交通全局最优 gap” |
| C5 | 选中 developed gap 可转化为合流时间、位置和可行轨迹。 | 贡献 3 | B 的 trajectory figure | “converted into executable terminal conditions and trajectory” | “解决所有闭环控制问题” |
| C6 | 在代表性成功合流场景中，OneStep 相比 FIFO 能以相近物理合流时间减少主线扰动。 | 贡献 3 的实验验证 | D 的 external comparison | “in representative successful scenarios” | “所有场景都优于 FIFO” |

---

## 11. Citation Sentence Map

现阶段不需要完整 citation support bank。现在只建立句子槽位，避免 Introduction 写散。正式文献查找和 BibTeX key 后续单独做。

### 11.1 当前只需要的 citation 槽位

| 句子功能 | 需要支撑的中文句意 | 文献类型 | 当前状态 |
|---|---|---|---|
| 背景 | 匝道合流是自动驾驶和智能交通中的重要交互规划问题。 | CAV merging / on-ramp merging survey 或代表论文 | 后续查找 |
| 现有方法 1 | 许多 CAV 合流方法通过优化顺序、轨迹、能耗或通行效率实现协调。 | cooperative merging optimization | 后续查找 |
| 现有方法 2 | 轨迹优化方法通常在目标 gap 或终端条件给定后生成可行轨迹。 | trajectory optimization / optimal control | 后续查找 |
| 问题缺口 | gap creation 可能使 facilitating vehicle 减速，并带来主线扰动或临时拥堵。 | gap creation / facilitating vehicle speed loss / temporary mainline congestion | 重点查找，含 Zhou 等 |
| baseline | FIFO/RiosTorres 类方法提供队列式合流协调基线。 | Rios-Torres / Malikopoulos 或相关 FIFO merging baseline | 后续确认 |
| 本文区别 | 本文显式评价发展每个候选 gap 所需的有限主线协作代价。 | 不需要外部引用，由本文方法支撑 | 自身贡献 |

### 11.2 引文工作放到什么时候

不在 v0.5 阶段做完整引文库。原因：

```text
当前优先级是锁定 Method 公式链和实验图表；
Introduction 还未进入正式写作；
refs.bib 还未建立真实文献集合；
引用应在 Introduction + Related Work 草稿前集中处理。
```

升级到 Control Sheet v1 前，需要完成：

```text
每个 citation 槽位至少 1 到 3 篇候选文献；
每篇文献真实存在并能进入 refs.bib；
每条引用只服务固定句子，不堆砌。
```

---

## 12. Drafting Order

初稿写作顺序：

```text
1. Problem Formulation
2. Method skeleton
3. Figure 1 方法图草稿需求
4. Experiment table templates
5. Introduction + compact Related Work
6. Experiments prose
7. Abstract
8. Conclusion
```

### 12.1 为什么先写 Problem Formulation + Method

原因：

```text
这篇论文的可控性来自符号和公式链；
Introduction 的漂亮叙述不能替代 Method 的清楚；
Method 定住后，Introduction 的 gap 和 contribution 会自然变窄；
实验图表也依赖 Method 中的符号定义。
```

### 12.2 Problem Formulation 初稿目标

产物：

```text
一节 0.5 页左右的英文初稿；
包括符号定义、局部决策问题、假设边界、抽象目标。
```

必须回答：

```text
candidate gap 是什么；
为什么当前长度不足的 gap 不应直接删除；
主线协作为什么是有约束、有代价的；
本文优化对象是什么；
哪些问题不在本文范围内。
```

### 12.3 Method skeleton 初稿目标

产物：

```text
一节 1.6 到 1.8 页的英文 skeleton；
公式位置基本确定；
图表引用位置预留；
每个小节有 1 到 2 段解释和核心公式。
```

必须完成：

```text
capacity 公式；
QP 和闭式投影解；
developed gap score；
terminal time/location；
selected-gap trajectory。
```

---

## 13. Writing Rationale Matrix Lite

这不是完整 PaperSpine matrix，而是初稿写作时的最小行级控制。

| Row ID | Manuscript Unit | Planned Function | Evidence / Anchor | Final Check |
|---|---|---|---|---|
| R0 | Whole paper framework | 把论文从“如何让 MV 并进去”收束到“以多小主线协作代价发展并选择安全 gap”。 | 四篇核心文档 + v3 实验链 | 摘要、Introduction、Method、Experiments 都围绕 mainline disturbance cost。 |
| R1 | Introduction opening | 建立主线协作不是免费资源的动机。 | gap creation / mainline disturbance 文献槽位 | 不把问题写成普通轨迹优化。 |
| R2 | Contribution bullets | 用三条贡献压缩问题视角、机制、选择执行链。 | 本 Control Sheet 第 4 节 | 不出现 4 到 6 条贡献。 |
| R3 | Problem Formulation | 定义 candidate gap、safety gap、adjacent gap、boundary adjustment。 | 公式整理文档 0 到 4 节 | 符号与 Method 完全一致。 |
| R4 | Capacity subsection | 说明协作能力来自相邻 gap 余量。 | \(\bar{\delta}_{f,i},\bar{\delta}_{r,i}\) | 图 1(a) 能直观看懂。 |
| R5 | QP subsection | 说明 gap shortage 如何低扰动分配。 | QP + projection solution | 不声称全局交通最优。 |
| R6 | Valuation subsection | 说明 developed candidate 如何评分。 | \(C_i^{coop},C_i^{ego},J_i\) | Table I 能对应每个变量。 |
| R7 | Trajectory subsection | 说明 selected gap 如何变成 \(t_m,p_m,x(t)\)。 | quintic trajectory formulas | 只对 \(i^*\) 执行的逻辑清楚。 |
| R8 | Experiments B | 展示完整链条可执行。 | S01 score table + trajectory | 不把 B 写成外部性能对比。 |
| R9 | Experiments C | 验证 capacity/QP 机制行为。 | S02/S06/S07/S08 | 不写成完整系统消融。 |
| R10 | Experiments D | 比较 OneStep 与 FIFO 的主线扰动。 | S01 报告 + 后续场景 | 所有结论限定在 representative successful scenarios。 |
| R11 | Conclusion | 收束贡献和未来工作。 | v3 边界 | 后续工作不写成当前论文失败。 |

---

## 14. Immediate Next Actions

### 14.1 我这边下一步

```text
写 Problem Formulation + Method skeleton。
```

写作前必须遵守：

```text
以本 Control Sheet 的符号为准；
不展开论文2内容；
不把可达性当贡献；
不把 C 节机制实验写成完整系统消融；
所有公式先按 Method Formula Depth 进入 skeleton。
```

### 14.2 你这边实验整理优先级

优先整理：

```text
1. S01 candidate gap score table；
2. S01 OneStep trajectory / time-space data；
3. C 节 S02/S06/S07/S08 机制表；
4. D 节 S01/S02/S03/S06/S07/S08 外部对比原始结果。
```

D 节不急着决定最终正文场景。先跑六个，再根据真实数据选 3 到 4 个。

### 14.3 Control Sheet v1 升级条件

当以下内容完成后，把本文档升级为 `v1`：

```text
Problem Formulation 初稿完成；
Method skeleton 完成；
Table I/II/III 的真实列名和数据格式确定；
Fig. 1 和 Fig. 2 的图形设计确定；
D 节最终正文场景确定；
citation sentence map 对应真实文献 key。
```

`v1` 之后就不再频繁重开故事，只做写作层面的收缩、润色和证据对齐。

