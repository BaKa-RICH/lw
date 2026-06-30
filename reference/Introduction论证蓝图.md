# Introduction 论证蓝图

> 本文档服务于 ICUS 会议论文 Introduction 写作。它只支撑论文1的论证主线：主线协作有代价，候选 gap 需要在相邻安全余量约束下被协作发展，再用扰动-效率联合评分选择发展后的最优 gap。长匝道 action set、多 MV rolling、内侧换道辅助、SUMO 闭环和 CAV/HDV 混行不进入 ICUS 的中心论证。

---

## 1. 文档定位

Introduction 要完成三件事：

```text
1. 把匝道合流的本质矛盾从“MV 能不能并入”转为“主线协作为合流付出多少代价”；
2. 说明现有 CAV 合流、trajectory optimization、gap selection 和 cooperative gap creation 方法尚未充分定价 gap development 的主线扰动；
3. 引出本文方法：估计每个候选 gap 的最小必要协作代价和 MV 合流代价，再用扰动-效率联合评分选择发展后的最优 gap，并执行相应轨迹。
```

推荐问题链：

```text
主线协作不是免费的
    ->
gap 不应只是静态选择对象
    ->
一个当前不足的候选 gap 可以通过有限主线边界车辆协作发展成安全 gap
    ->
但 gap development 会消耗主线车辆速度、位置和相邻空间余量
    ->
因此应先估计每个候选 gap 的最小必要协作代价和 MV 合流代价
    ->
再用扰动-效率联合评分选择发展后的最优 gap，并最终执行该 gap 的协作调整和轨迹生成
```

---

## 2. Introduction 的中心论点

### 2.1 中文中心论点

本文主张，匝道合流不应仅被视为合流车轨迹规划或静态 gap 选择问题，而应被建模为主线扰动感知的协作 gap development 问题。一个当前空间不足的候选 gap 可以通过有限边界车辆协作发展为安全 gap；然而这种协作会消耗主线车辆的速度、位置和相邻空间余量，不能被视为免费资源。因此，候选 gap 应在估计其最小必要协作代价和 MV 合流代价后进行扰动-效率联合评分，并据此选择发展后的最优 gap。

### 2.2 英文中心论点草案

```text
This paper argues that on-ramp merging should be formulated as a mainline-disturbance-aware cooperative gap development problem, rather than merely a static gap selection or trajectory optimization problem. A spatially insufficient candidate gap can be developed into a safe merging gap through limited boundary-vehicle cooperation; however, such cooperation consumes mainline speed, position, and neighboring gap margins, and should therefore be explicitly constrained, priced, and minimized before the developed gap is selected through a disturbance-efficiency valuation.
```

### 2.3 需要守住的表达边界

ICUS 中不要把以下内容写成贡献或必要实验：

```text
long-ramp action generation
multi-MV rolling coordination
lane-change-assisted gap supply
random traffic statistics
SUMO closed-loop validation
CAV/HDV mixed traffic
```

可以作为 limitation 或 future work 简短出现，但 Introduction 主线不要依赖这些内容。

---

## 3. Introduction 问题链的五个部分

### Part 1: 背景与本质矛盾

开头要从匝道合流的交通冲突讲起：

```text
On-ramp merging is a typical conflict point where ramp vehicles need to enter the mainline stream while mainline vehicles attempt to preserve speed and spacing.
```

随后立刻提出本文视角：

```text
The essential question is not only whether the merging vehicle can enter the mainline, but how much cooperation the mainline traffic must pay to make such entry safe.
```

中文写法：

```text
匝道合流的核心矛盾不只是合流车能否找到并进入某个 gap，而是为了使该 gap 满足安全合流要求，主线交通需要付出多少协作代价。
```

这里要强调主线扰动：

```text
boundary vehicle deceleration or acceleration
speed loss of facilitating vehicle
following disturbance propagation
temporary mainline congestion
neighboring-gap compression
```

### Part 2: 现有方法图谱

Related Work 在 Introduction 中可以压缩成三类：

```text
1. Cooperative merging, sequencing, and scheduling
2. Trajectory optimization and flexible terminal planning
3. Cooperative gap creation/development and mainline disturbance
```

每类对应的评价口径：

```text
Cooperative merging / sequencing:
    优点：能优化合流顺序、总延误、能耗或安全性。
    不足：往往把主线车辆作为统一协调资源，主线协作代价没有被作为候选 gap 选择的核心对象。

Trajectory optimization:
    优点：能生成满足速度、加速度、安全距离约束的合流轨迹。
    不足：通常假设目标 gap 或终端条件已经给定，较少回答 gap 本身需要多少主线协作才能形成。

Gap creation / gap development:
    优点：已经关注 facilitating vehicle 的减速、gap development 对速度和拥堵的影响。
    不足：仍需要进一步把相邻 gap 余量、最小必要协作分配和发展后 gap valuation 连接成一个可计算链条。
```

### Part 3: 隐性缺口

隐性缺口不是“没有人做 gap selection”，而是：

> 候选 gap 的评价常未把协作发展所需的主线扰动代价作为核心评价对象。

可写成三个问题：

```text
1. 当前不足的候选 gap 是否能在不破坏相邻 gap 安全余量的前提下被发展？
2. 发展它需要前后边界车付出多少协作调整量？
3. 该协作代价与 MV 自车合流代价之间如何权衡？
```

英文缺口表达：

```text
Existing methods often optimize the merging trajectory or select a target gap after coordination assumptions are made, but they provide limited modeling of how much bounded mainline cooperation is required to develop each candidate gap and how this cost should be balanced against the MV's own merging efficiency.
```

### Part 4: 本文重定义

核心句：

```text
This work reframes on-ramp merging as the problem of developing and selecting a safe gap with the minimum necessary mainline cooperation cost.
```

中文逻辑：

```text
本文不从“如何最大化合流效率”出发，而从“如何以最少主线协作代价创造一个安全 gap”出发。
```

展开写法：

```text
给定多个候选 gap，本文不简单按当前长度、距离或自车代价选择，而是先估计每个 gap 在边界车辆有限协作下能否被发展为安全 gap，以及发展它所需的最小主线协作代价。随后，本文将该协作代价与 MV 合流代价共同纳入扰动-效率联合评分，选择发展后的最优 gap，并将其转化为合流时间、合流位置和可行轨迹。
```

### Part 5: 本文贡献

ICUS 建议三条贡献：

```text
1. A mainline-disturbance-aware cooperative gap development formulation is introduced for on-ramp merging, where boundary-vehicle cooperation is treated as bounded and costly rather than free.

2. An adjacent-gap-preserving cooperation capacity model and a space-sensitive weighted quadratic adjustment are developed to estimate the minimum necessary cooperation for each candidate gap under neighboring-gap safety margins.

3. A disturbance-efficiency valuation is proposed to select the best developed gap, followed by terminal time/location determination and closed-form feasible trajectory generation.
```

中文对应：

```text
1. 提出主线扰动感知的协作 gap development 问题表述，将边界车辆协作视为有限且有代价的资源。

2. 提出基于相邻 gap 余量的协作能力约束模型和空间敏感二次调整方法，在相邻安全余量约束下估计每个候选 gap 的最小必要协作。

3. 提出扰动-效率联合 developed-gap valuation 与选择机制，并进一步确定合流时空点与闭式可行轨迹。
```

---

## 4. Related Work 分类矩阵

| 类别 | 现有工作通常解决什么 | 仍可强调的缺口 | 本文切入点 |
|---|---|---|---|
| Cooperative merging and sequencing | 合流顺序、车辆协调、总效率或安全约束 | 主线协作常作为可调度资源出现，其局部代价未必被显式定价 | 把主线边界车辆协作建模为 bounded and costly local resource |
| Trajectory optimization | 给定目标 gap 或终端条件后生成可行轨迹 | 终端 gap 的形成过程和主线协作代价常被弱化 | 在轨迹优化前先评价候选 gap 的可发展性、协作能力和发展代价 |
| Cooperative gap creation/development | 通过 facilitating vehicle 调整创造或扩大 gap | 已注意速度损失和拥堵影响，但仍需要更明确的相邻空间约束与最小扰动分配 | 相邻 gap 余量 -> 协作能力上界 -> 空间敏感 QP |
| Integrated optimization | 联合考虑顺序、控制、能耗和安全性 | 全局问题可能复杂，且不一定突出局部 mainline disturbance pricing | 使用轻量、可解释的分解链：capacity, QP, valuation, trajectory |

Related Work 正文建议四个小段：

```text
1. Coordination and sequencing strategies for on-ramp merging
2. Trajectory optimization with flexible terminal conditions
3. Cooperative gap creation and mainline disturbance
4. Mainline-disturbance-aware gap development and valuation
```

不要专门设置 candidate generation 或 action set 小节，那会把故事拉回论文2。

---

## 5. 缺口表述候选

### 5.1 保守版

```text
Although cooperative merging and trajectory optimization have been extensively studied, less attention has been paid to evaluating a candidate gap according to the minimum bounded mainline cooperation required to develop it into a safe merging gap.
```

### 5.2 推荐版

```text
A key missing link is how to treat gap development itself as a constrained and costly local resource allocation problem: the boundary vehicles can only contribute within neighboring-gap safety margins, the required adjustment should be allocated with minimum disturbance, and the developed candidates should be compared through a joint valuation of mainline cooperation and MV merging efficiency.
```

### 5.3 强势版

```text
Without explicitly pricing bounded mainline cooperation, a merging planner may select a gap that is convenient for the MV but expensive for the mainline, thereby transferring the merging burden to facilitating vehicles and their followers.
```

### 5.4 中文推荐版

```text
现有方法并非没有研究合流协调或 gap creation，而是较少把候选 gap 的协作发展过程本身建模为有约束、有代价的局部主线资源分配问题：边界车辆能贡献多少受相邻 gap 安全余量限制，gap 缺口应以最小扰动方式分配给前后边界车，发展后的候选 gap 还需要在主线协作代价和 MV 合流代价之间进行联合评价。
```

---

## 6. Method 回扣表

| Introduction 问题 | Method 回答 | 对应方法节 |
|---|---|---|
| 主线协作为什么不是免费资源？ | gap development 会消耗边界车速度、位置和相邻空间余量 | Problem Formulation |
| 当前不足的 gap 是否必须舍弃？ | 不一定，可通过有限边界车辆协作发展为安全 gap | Adjacent-Gap-Preserving Cooperation Capacity |
| 主线边界车最多能帮多少？ | 由相邻 gap 余量决定 `delta_f_bar,i` 和 `delta_r_bar,i` | Adjacent-Gap-Preserving Cooperation Capacity |
| gap 缺口应由谁承担？ | 通过空间敏感 QP 最小化候选级协作扰动 | Space-Sensitive Minimum-Disturbance Adjustment |
| 多个发展候选如何比较？ | 先估计最小协作代价和 MV 合流代价，再联合评分选择发展后的最优 gap | Disturbance-Efficiency Valuation for Developed Gaps |
| 被选中的 gap 如何执行？ | gap 中心偏移决定 `d_i`，进一步确定 `t_m,i*`, `p_m,i`, `x_m(t)` | Terminal Time, Merging Location, and Feasible Trajectory |

---

## 7. Introduction 到实验的证据钩子

Introduction 不需要提前堆具体 scenario 编号，而应把实验组织成三层证据。这样写的好处是，Introduction 只承诺本文真实能支撑的结果，不把 D 节误写成所有场景都保持相近 physical merge timing。

| Evidence layer | 对应实验 | 支撑的 Introduction claim | 写作边界 |
|---|---|---|---|
| Closed-chain feasibility | B. Feasibility Validation | candidate gaps can be evaluated as developable resources；selected developed gap can be converted into terminal time, merging location, and feasible trajectory | 不写外部性能优越性 |
| Mechanism ablation | C. Capacity and Adjustment Ablation | adjacent-gap capacity and space-sensitive QP allocate cooperation according to neighboring-gap margins | 不写完整系统消融，也不写真实交通全局最优 |
| Representative external comparison | D. External Algorithm Comparison | representative successful-merging cases show reduced mainline disturbance relative to FIFO | 只有 primary representative case 可写 nearly identical physical merge timing；其他 cases 可写 disturbance-efficiency tradeoff |

推荐英文 evidence hook：

```text
The experiments first validate the feasibility of the developed-gap execution chain, then examine the intended behavior of the capacity and adjustment mechanisms, and finally compare the proposed method with a FIFO baseline in representative successful-merging cases to evaluate mainline disturbance reduction.
```

若需要提 timing，只能加限定句：

```text
In the primary representative case, the disturbance reduction is achieved with nearly identical physical merge timing.
```

Introduction 不写：

```text
all scenarios maintain comparable merge timing；
the proposed method is always faster than FIFO；
internal scenario IDs such as S01/S02 as rhetorical labels。
```

---

## 8. 当前结论

ICUS Introduction 要聚焦一句话：

> 本文不是提出一个全局合流调度系统，而是提出一种主线扰动感知的 cooperative gap development and valuation 方法，用最小必要主线协作代价发展候选 gap，并选择发展后的最优 gap 执行合流。

写作时最容易出错的是两件事：一是把故事重新带回“候选 gap 搜索”或“长时域 rolling 协调”；二是把外部对比写成所有场景都在 physical merge timing 上与 FIFO 相近。前者应留给论文2，后者超出了当前 D 节证据。ICUS Introduction 只需要承诺三层证据：feasibility、mechanism behavior 和 representative lower mainline disturbance。
