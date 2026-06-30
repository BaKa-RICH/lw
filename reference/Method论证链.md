# Method 论证链

> 本文档服务于 ICUS 论文 Method 写作。Method 的任务是把候选 gap 的主线协作能力、协作调整代价、MV 合流代价和可行轨迹生成连接成一个分解式解析规划链条。本文档不再设置 candidate generation 或 action set 构造章节。

---

## 1. Method 写作总原则

Method 要回答的核心问题是：

```text
候选 gap 的主线协作能力如何确定？
主线边界车最多能帮多少？
gap 缺口应该由谁承担？
多个发展后的 gap 如何评分和选择？
选中的 gap 如何转化为合流时间、位置和轨迹？
```

总原则：

```text
candidate gap 的主线协作能力、协作调整代价、MV 合流代价和可行轨迹生成连接成一个分解式解析规划链条。
```

不要写成：

```text
先搜索所有可能 gap，再做长时域候选集构造，再做 rolling 协调。
```

推荐写成：

```text
给定当前合流决策中的 candidate gaps，本文关注每个 gap 如何被协作发展、评分、选择和执行。
```

这里的“给定”只是一句自然场景设定，不要过度强调“给定若干局部候选 gap 的情况下”，否则论文会显得像人为简化问题。写作上可以轻轻带过：

```text
At a merging decision instant, the planner evaluates candidate gaps in the mainline stream.
```

---

## 2. Method 总体叙事

### 2.1 不推荐的写法

```text
1. calculate candidate gap generation
2. calculate long-horizon action set
3. calculate cooperation
4. calculate trajectory
```

这种写法会把 ICUS 拉成系统型论文，且容易暴露后续论文2才要解决的问题。

### 2.2 推荐的写法

```text
Problem Formulation:
    这个问题为什么不是普通静态 gap selection 或 trajectory planning？

Adjacent-Gap-Preserving Cooperation Capacity:
    主线协作为什么是有限资源？

Space-Sensitive Cooperative Adjustment Optimization:
    如何估计每个候选 gap 的最小必要主线协作代价？

Disturbance-Efficiency Valuation for Selecting the Best Developed Gap:
    如何用扰动-效率联合评分选择发展后的最优 gap？

Flexible Terminal Time/Location and Trajectory Generation:
    被选中的 gap 如何被实际执行？
```

核心动作顺序必须写清楚：

```text
candidate gaps
-> candidate-level cooperation capacity
-> candidate-level minimum necessary adjustment estimate
-> candidate-level disturbance-efficiency score
-> selected developed gap
-> actual boundary adjustment and MV trajectory execution
```

注意区分：

```text
用于评分的候选级协作调整估计
!=
所有 gap 都被实际协作调整
```

---

## 3. Problem Formulation

### 3.1 这一节回答什么问题？

这一节要说明：本文研究的不是一个纯静态 gap selection，也不是一个只在目标 gap 给定后生成 MV 轨迹的问题。本文的问题是：

> 在候选 gap 中选择一个可通过有限主线边界车辆协作发展为安全 gap 的目标，并联合确定协作调整、合流时间、合流位置和可行轨迹，使主线协作代价与 MV 合流代价的综合成本最小。

### 3.2 应定义的对象

```text
G                  candidate gap set
i                  candidate gap index
G_i                current length of candidate gap i
G_req              required safe merging gap length
Delta_i            required shortage, max(0, G_req - G_i)
c_i                current center of candidate gap i
delta_f,i          front boundary vehicle adjustment estimate
delta_r,i          rear boundary vehicle adjustment estimate
delta_f_bar,i      maximum safe front-boundary contribution
delta_r_bar,i      maximum safe rear-boundary contribution
C_coop,i           mainline cooperation cost
C_ego,i            MV ego merging cost
J_i                integrated valuation score
t_m,i              merging time
p_m,i              merging location
x_m(t)             MV trajectory
```

### 3.3 抽象优化问题

可以用以下形式作为 Method 的总入口：

```text
min over i, delta_f,i, delta_r,i, t_m,i, p_m,i, x_m(t):
    J_i = w_c C_coop,i + w_e C_ego,i

s.t.
    i in G
    G_i + delta_f,i + delta_r,i >= G_req
    0 <= delta_f,i <= delta_f_bar,i
    0 <= delta_r,i <= delta_r_bar,i
    v_min <= v_m(t) <= v_max
    a_min <= a_m(t) <= a_max
    d_i = c_i + (delta_f,i - delta_r,i)/2 - x_m(0)
    p_m,i = x_m(0) + v_ref t_m,i + d_i
```

这里 `G` 表示当前合流决策中被评价的 candidate gaps。ICUS 论文不研究长时域候选生成或 action set 构造，而聚焦每个 candidate gap 如何被协作发展、评分、选择和执行。

### 3.4 原始优化问题的论文作用

这个原始问题的作用不是让读者以为本文要做全局混合整数规划，而是为后续分解链条提供统一目标：

```text
mainline cooperation cost
ego merging cost
safe developed gap
feasible trajectory
```

随后说明该问题被分解为四个可解释子问题：

```text
1. cooperation capacity
2. minimum-disturbance adjustment
3. developed gap valuation
4. terminal condition and trajectory generation
```

### 3.5 转入下一节的桥

```text
The first step is to determine how much each boundary vehicle can safely contribute. This capacity is not a fixed parameter; it is constrained by the residual safety margin of the neighboring gaps.
```

---

## 4. Adjacent-Gap-Preserving Cooperation Capacity

### 4.1 这一节回答什么问题？

这一节回答：

```text
Given a candidate gap, how much can its boundary vehicles safely contribute without compressing neighboring gaps below the safety margin?
```

也就是：主线协作能力从哪里来？

### 4.2 论文论证重点

给定一个 candidate gap，它的前后边界车辆可协作能力不是固定常数，也不是随意设定的最大调整量，而是由相邻 gap 的剩余安全余量决定。

直观链条：

```text
front boundary vehicle moves forward
-> its front-side neighboring gap is compressed
-> maximum forward contribution is bounded by that neighboring gap margin

rear boundary vehicle moves backward
-> its rear-side neighboring gap is compressed
-> maximum backward contribution is bounded by that neighboring gap margin
```

因此：

```text
0 <= delta_f,i <= delta_f_bar,i
0 <= delta_r,i <= delta_r_bar,i
```

### 4.3 推荐表达

```text
Given a candidate gap, the first question is not whether the mainline vehicles are willing to help, but how much its boundary vehicles can safely contribute without compressing neighboring gaps below the safety margin.
```

### 4.4 公式应该服务什么

公式要服务于两个结论：

```text
1. mainline cooperation is bounded;
2. the bound is state-dependent and comes from neighboring gap margins.
```

可以抽象写成：

```text
delta_f_bar,i = max(0, G_front_adj,i - G_adj_req)
delta_r_bar,i = max(0, G_rear_adj,i - G_adj_req)
```

具体符号可根据论文正文中的车辆编号再调整。

### 4.5 需要明确的输出

本节输出：

```text
delta_f_bar,i
delta_r_bar,i
```

它们进入下一节，用于判断 candidate gap 的协作发展能力和约束 QP 的可行域。

### 4.6 如果没有这一节会怎样？

没有这一节，论文会退化为普通 gap enlargement：

```text
只要目标 gap 不够，就让边界车移动。
```

但这会忽略主线协作对相邻车辆的二次影响，也无法证明本文是 mainline-disturbance-aware。

### 4.7 对应实验

对应实验：

```text
Adjacent-Gap Capacity Test
```

核心验证：

```text
固定或无边界的协作能力会错误估计 gap development 可行性，或压缩相邻 gap；
基于相邻 gap 余量的能力模型可以保持相邻安全余量。
```

### 4.8 转入下一节的桥

```text
After the cooperation capacity of each boundary vehicle is obtained, the remaining question is how to allocate the required gap shortage between the two boundary vehicles with minimum disturbance.
```

---

## 5. Space-Sensitive Minimum-Disturbance Adjustment

### 5.1 这一节回答什么问题？

这一节回答：

```text
For each candidate gap, how should the required shortage be allocated to its boundary vehicles with minimum mainline disturbance?
```

### 5.2 论文论证重点

对 candidate gap `i`，当前长度不足量为：

```text
Delta_i = max(0, G_req - G_i)
```

若：

```text
Delta_i > delta_f_bar,i + delta_r_bar,i
```

则该 candidate gap 在当前相邻安全余量约束下无法被纵向协作发展为安全 gap。若可发展，则求解：

```text
min C_coop,i = gamma_f,i delta_f,i^2 + gamma_r,i delta_r,i^2
s.t.
    delta_f,i + delta_r,i >= Delta_i
    0 <= delta_f,i <= delta_f_bar,i
    0 <= delta_r,i <= delta_r_bar,i
```

权重建议写成：

```text
gamma_f,i = (delta_ref / delta_f_bar,i)^q
gamma_r,i = (delta_ref / delta_r_bar,i)^q
```

含义：

```text
可用余量越小 -> 该侧调整越昂贵
可用余量越大 -> 该侧可以承担更多 gap shortage
```

### 5.3 输出与动作顺序

本节输出：

```text
delta_f,i*
delta_r,i*
C_coop,i
```

必须在正文中明确：

```text
These values are first computed as candidate-level optimal adjustment estimates for valuation. The physical adjustment is executed only for the selected gap after valuation.
```

中文解释：

> `delta_f,i*` 和 `delta_r,i*` 首先只是候选级最小必要协作调整估计，用于计算评分；实际物理调整只对最终被选中的 gap 执行。

### 5.4 关于解析解的写法

如果论文篇幅允许，可以说明该问题是二维凸二次规划，具有可解释解结构：

```text
unconstrained weighted split
-> projection onto box constraints
-> saturation of tighter side if necessary
```

但 ICUS 篇幅有限，不一定需要完整展开所有 case。建议把核心放在可解释性：

```text
The solution naturally shifts adjustment away from the side with smaller residual neighboring-gap margin.
```

### 5.5 如果没有这一节会怎样？

没有这一节，本文只知道 gap 是否可以发展，却不知道发展它的最小扰动代价是多少。那后续 valuation 就只能比较静态 gap 或 ego cost，无法支撑“主线协作不是免费”的中心论点。

### 5.6 对应实验

对应实验：

```text
Space-Sensitive Adjustment Ablation
```

对比方法：

```text
equal split
front-only
rear-only
capacity-proportional
unweighted QP
proposed space-sensitive QP
```

验证指标：

```text
C_coop
maximum single-vehicle adjustment
tight-side margin consumption
remaining neighboring-gap safety margin
```

### 5.7 转入下一节的桥

```text
Once each candidate gap has a minimum cooperation estimate, the planner can compare the developed candidates by jointly considering mainline cooperation cost and MV ego merging cost.
```

---

## 6. Disturbance-Efficiency Valuation for Selecting the Best Developed Gap

### 6.1 这一节回答什么问题？

这一节回答：

```text
After each candidate gap obtains its minimum necessary cooperation estimate and ego merging cost, which developed candidate should be selected?
```

### 6.2 论文论证重点

最优 gap 不一定是：

```text
nearest gap
largest gap
minimum ego cost gap
minimum cooperation cost gap
existing-safe-gap only
```

本文的选择对象是“经过候选级协作发展估计后的 developed candidate”，评分为：

```text
J_i = w_c C_coop,i + w_e C_ego,i
i* = argmin_i J_i
```

其中：

```text
C_coop,i = gamma_f,i (delta_f,i*)^2 + gamma_r,i (delta_r,i*)^2
C_ego,i  = K d_i^2 / (t_m,i*)^3 + w_t t_m,i*
```

### 6.3 评分动作在前，选择在后，执行最后

正文中要明确动作顺序：

```text
1. estimate candidate-level cooperation and ego cost for each gap
2. score each developed candidate
3. select the best developed gap
4. execute only the selected gap
```

不要写成：

```text
发展后的 gap 用扰动-效率联合评分选择
```

推荐写成：

```text
用扰动-效率联合评分选择发展后的最优 gap。
```

英文推荐：

```text
The planner selects the best developed gap through a disturbance-efficiency valuation.
```

### 6.4 需要明确的输出

本节输出：

```text
J_i for each candidate
i* selected developed gap
```

### 6.5 对应实验

对应实验：

```text
Gap Valuation and Best Developed Gap Selection
```

核心验证：

```text
本文方法选择的 gap 不一定最近、最大或 ego 最省力，但在 C_coop 和 C_ego 之间取得更合理折中。
```

### 6.6 转入下一节的桥

```text
After the best developed gap is selected, its cooperation-induced center shift determines the MV's terminal displacement, from which the merging time, merging location, and feasible trajectory are generated.
```

---

## 7. Terminal Time, Merging Location, and Feasible Trajectory

### 7.1 这一节回答什么问题？

这一节回答：

```text
How is the selected developed gap converted into an executable MV trajectory?
```

### 7.2 关键连接变量：`d_i`

只对被选中的 gap 执行。协作调整导致 gap 中心移动：

```text
c_i' = c_i + (delta_f,i* - delta_r,i*) / 2
```

MV 需要匹配的终端位移为：

```text
d_i = c_i' - x_m(0)
```

这条连接很关键，因为它把主线协作结果传递到 MV 轨迹规划中。

### 7.3 合流时间优化

可以用简洁表达：

```text
t_m,i* = argmin_t K d_i^2 / t^3 + w_t t
```

解释：

```text
too short t -> high control effort
too long t  -> low efficiency
```

### 7.4 合流位置

```text
p_m,i = x_m(0) + v_ref t_m,i* + d_i
```

该位置不是固定合流点，而是由 selected developed gap 的中心偏移和合流时间共同决定。

### 7.5 轨迹生成

采用闭式可行轨迹生成，例如多项式轨迹：

```text
x_m(t), v_m(t), a_m(t)
```

并检查：

```text
v_min <= v_m(t) <= v_max
a_min <= a_m(t) <= a_max
terminal position
terminal speed
```

### 7.6 对应实验

对应：

```text
Feasibility Validation
trajectory plot
speed profile
acceleration profile
```

---

## 8. Method 各节之间的关键连接

Method 中建议显式写出以下连接，帮助读者理解为什么每节必要：

```text
连接 1：G_i -> Delta_i
    当前 gap 长度决定安全缺口。

连接 2：相邻 gap 余量 -> delta_f_bar,i, delta_r_bar,i
    相邻安全余量决定边界车辆最大协作能力。

连接 3：Delta_i + capacity -> candidate-level delta_f,i*, delta_r,i*, C_coop,i
    gap 缺口和协作能力共同决定最小必要主线协作估计。

连接 4：delta_f,i*, delta_r,i* -> d_i -> C_ego,i
    协作调整改变 gap 中心，从而改变 MV 终端位移和自车代价。

连接 5：C_coop,i + C_ego,i -> J_i -> selected developed gap
    扰动-效率联合评分选择发展后的最优 gap。

连接 6：selected gap -> actual adjustment -> t_m,i* -> p_m,i -> x_m(t)
    只对选中的 gap 执行协作调整和轨迹生成。
```

---

## 9. 子问题最优性与声明边界

### 9.1 可以声明的内容

```text
在给定 candidate gap 集合和评分函数下，i* = argmin J_i 是评分意义下的最优发展后 gap。

在每个 candidate gap 的协作能力约束下，空间敏感 QP 给出该 gap 的最小二次协作代价调整估计。

被选中的 developed gap 可以进一步转化为满足速度、加速度和终端条件的可行 MV 轨迹。
```

### 9.2 不应声明的内容

```text
本文不声明解决长匝道 action generation 问题。
本文不声明解决多 MV rolling resource allocation 问题。
本文不声明解决内侧换道辅助 gap supply 问题。
本文不声明 SUMO 闭环或 CAV/HDV 混行下的交通级最优性。
```

### 9.3 推荐措辞

```text
Under the considered candidate gaps and valuation function, the proposed decomposition selects the developed gap with the minimum disturbance-efficiency score.
```

---

## 10. Method 与实验的对应关系

| Method 节 | 要证明什么 | 对应实验 |
|---|---|---|
| Problem Formulation | 完整链条能从 candidate gap 发展、评分选择到轨迹执行闭环运行 | Feasibility Validation |
| Adjacent-Gap-Preserving Cooperation Capacity | 固定或无边界协作会错误估计协作能力或压缩相邻 gap | Adjacent-Gap Capacity Test |
| Space-Sensitive Minimum-Disturbance Adjustment | 相比 equal split、front-only、rear-only、capacity-proportional、unweighted QP，本文更少消耗紧张侧空间并降低最大单车扰动 | Space-Sensitive Adjustment Ablation |
| Disturbance-Efficiency Valuation | 最优发展后 gap 不一定最近、最大或 ego 最省力 | Gap Valuation and Selection Test |
| Terminal Time/Location and Trajectory | 选中 gap 可转化为满足约束的合流时空点和轨迹 | Feasibility Validation, trajectory/speed/acceleration plots |

---

## 11. Method 写作反面清单

不要写：

```text
本文先构造长时域 action set。
本文重点解决多 MV rolling resource conflict。
所有 gap 先被实际调整，然后再评分。
gap 发展后的选择发生在实际协作执行之后。
主线车辆协作默认服从且无代价。
```

要写：

```text
候选级估计用于评分；
实际协作只对选中 gap 执行；
主线协作能力由相邻 gap 安全余量约束；
协作分配按空间敏感代价最小化；
最终用扰动-效率联合评分选择发展后的最优 gap。
```

---

## 12. 推荐 Method 章节标题草案

IEEE 会议论文可采用紧凑结构：

```text
II. Problem Formulation

III. Mainline-Disturbance-Aware Cooperative Gap Development
    A. Adjacent-Gap-Preserving Cooperation Capacity
    B. Space-Sensitive Minimum-Disturbance Adjustment
    C. Disturbance-Efficiency Valuation for Developed Gaps
    D. Terminal Time, Merging Location, and Feasible Trajectory
```

如果篇幅紧，也可以将 II 和 III.A 合并部分叙述，但不建议删除 valuation 和 trajectory execution 的连接，否则故事会断在“估计协作代价”而不是“选中并执行 gap”。

---

## 13. 当前结论

ICUS Method 的主线必须清晰：

```text
candidate gap
-> adjacent-gap-preserving capacity
-> space-sensitive minimum cooperation estimate
-> disturbance-efficiency valuation
-> selected developed gap
-> terminal condition and feasible trajectory
```

这条链条足以支撑 ICUS 初稿，不需要把论文2的 rolling、换道、SUMO 和混行交通内容提前塞进 Method。

