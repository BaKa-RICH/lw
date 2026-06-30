> 最优合流时间t最终是从[无约束最优合流时间，加速度约束，速度约束]的最大值里面选对吗？那其实这个问题是不是也可以用一个优化问题来表达呢？就像之前算最优协调量一样，文档里的7. 每个 gap 的合流时间计算更像是算最优合流时间的一种方法，但对于论文，应该更聚焦于怎么把这个问题描述清楚，求解的方法也应该有多种吧？
> 再帮我梳理一下，4、5章给6、7章提供了什么？我说说我的理解，6、7章就是在算最优合流时间，这个最优合流时间也直接关系到最优合流位置，这两个应该是一体的？6、7章应该运用到了4、5章里算出来的最优协调量，但是是怎么利用的呢？在哪里利用、哪个公式？7、8章想要做的事情是不是还有很多方法可以求解，这篇文档里的方法有被文献使用过吗？还是原创，帮我调研文献，至少相关10篇。大家对于这个具体的问题的解法都有什么，简要介绍，不要相似问题，就这个具体的不确定合流点不确定合流时间求最优合流时间的问题。
> 你要理解我的问题后再调研，调研清楚后，再思考，思考清楚后再给我汇报。

我会分两层回答：先把数学推导补全到“每一步怎么来”，再把 7、8 章抽象成优化问题，并结合文献判断它是常见套路、变体，还是更像本文档的工程闭式求解。

我查到的文献里，和你这个问题最接近的通常叫“on-ramp merging 的 merging time / merging sequence / trajectory planning 联合优化”，有些固定合流点，有些会在候选点或候选 gap 中搜索。下一步我会筛掉只讲普通换道或无关路径规划的文献。

你这次的问题可以拆成2个核心点：

3. 第 7 章的 $t_m^*$ 能不能写成一个优化问题；
4. 第 4、5 章到底怎么给第 6、7、8 章提供输入，以及这种方法在文献里是不是常见。

下面按这个顺序讲。

# 3. 最优合流时间是不是从三个值里取最大？

对，文档中的最终合流时间就是：

$$

t_{m,i}^*
=
\max
\left(
t_{m,i}^{0},
t_{a,i},
t_{v,i}
\right)

$$

也就是从：

$$

\text{无约束最优时间}

$$

$$

\text{加速度约束时间下界}

$$

$$

\text{速度约束时间下界}

$$

里面取最大值。fileciteturn0file0

---

## 3.1 这其实可以写成一个优化问题

完全可以。

对某一个 gap $i$，前面已经算出了 $d_i$。那么第 7 章可以写成下面这个优化问题：

$$

\min_{t_m>0}
\quad
\frac{K d_i^2}{t_m^3}
+
w_t t_m

$$

约束为：

$$

\frac{\frac{10}{\sqrt{3}}|d_i|}{t_m^2}
\leq
a_{\lim}

$$

以及速度约束：

$$

\frac{1.875|d_i|}{t_m}
\leq
v_{\lim,i}

$$

其中：

$$

v_{\lim,i}
=
\begin{cases}
v_{\max}-v_{ref}, & d_i\geq 0\\[4pt]
v_{ref}-v_{\min}, & d_i<0
\end{cases}

$$

这个问题的意思是：

> 在满足最大速度、最大加速度约束的情况下，选择一个合流时间 $t_m$，让“加速度能量 + 时间损失”最小。

---

## 3.2 这是一个什么优化问题？

它是一个**一维凸优化问题**。

目标函数是：

$$

F(t_m)=\frac{K d_i^2}{t_m^3}+w_t t_m

$$

令：

$$

A=Kd_i^2

$$

则：

$$

F(t_m)=A t_m^{-3}+w_t t_m

$$

一阶导数：

$$

F'(t_m)=-3A t_m^{-4}+w_t

$$

二阶导数：

$$

F''(t_m)=12A t_m^{-5}

$$

只要：

$$

d_i\neq 0

$$

就有：

$$

A>0

$$

因此：

$$

F''(t_m)>0

$$

所以目标函数是严格凸的。

约束也可以改写成：

$$

t_m\geq t_{a,i}

$$

$$

t_m\geq t_{v,i}

$$

所以可行域是：

$$

t_m\in
[
\max(t_{a,i},t_{v,i}),\infty
)

$$

这是一个凸区间。

因此第 7 章不是随便取最大，而是在求解一个特殊的一维凸优化问题。

---

## 3.3 为什么解就是取最大值？

无约束最优时间是：

$$

t_{m,i}^{0}
=
\left(
\frac{3Kd_i^2}{w_t}
\right)^{1/4}

$$

但这个时间可能太短，导致速度或加速度超限。

加速度约束要求：

$$

t_m\geq t_{a,i}

$$

速度约束要求：

$$

t_m\geq t_{v,i}

$$

所以最终可行的时间必须满足：

$$

t_m\geq
\max(t_{a,i},t_{v,i})

$$

对于一个严格凸函数，若无约束最优点已经在可行区间内，就用它；如果无约束最优点在可行区间左边，就取可行区间的左端点。

因此：

$$

t_{m,i}^*
=
\max
\left(
t_{m,i}^{0},
t_{a,i},
t_{v,i}
\right)

$$

这和 4.9 的“投影解”很像。

4.9 是把无约束协作调整量投影到可行区间：

$$

\delta_f^*=\Pi_{[L,U]}(\delta_f^{raw})

$$

第 7 章也可以理解为把无约束最优时间投影到时间下界上：

$$

t_m^*=\max(t_m^0,t_{\min})

$$

其中：

$$

t_{\min}=\max(t_a,t_v)

$$

只是因为这里可行区间是：

$$

[t_{\min},\infty)

$$

所以投影形式就变成了简单的：

$$

\max(\cdot)

$$

---

# 4. 对论文来说，第 7 章应该怎么描述？

你的理解是对的。

文档里的第 7 章更像是：

> 在已经选定五次平滑轨迹族的前提下，对每个候选 gap 求解最优合流时间的一个闭式方法。

论文里更应该先把问题描述清楚，而不是一上来就写：

$$

t_m^*=\max(...)

$$

更好的逻辑是：

先定义轨迹族：

$$

x_m(t)=x_m(0)+v_{ref}t+d_iS(t/t_m)

$$

然后定义目标：

$$

\min_{t_m}
\frac{K d_i^2}{t_m^3}
+
w_t t_m

$$

再定义约束：

$$

|a_m(t)|\leq a_{\lim}

$$

$$

v_{\min}\leq v_m(t)\leq v_{\max}

$$

然后说明由于：

$$

\max |S'|=1.875

$$

$$

\max |S''|=\frac{10}{\sqrt{3}}

$$

这些连续时间约束可转化为时间下界：

$$

t_m\geq t_v

$$

$$

t_m\geq t_a

$$

最后得到闭式解：

$$

t_m^*
=
\max
(t_m^0,t_a,t_v)

$$

这样写，论文味道会更强。

也就是说：

> 第 7 章不是一个任意经验公式，而是一个一维约束凸优化问题的解析解。

---

# 5. 第 4、5 章给第 6、7 章提供了什么？

你的理解已经很接近了，但还可以再精确一点。

第 4 章算的是：

$$

\delta_{f,i}^*

$$

和：

$$

\delta_{r,i}^*

$$

也就是前车和后车分别调整多少。

第 5 章用这两个调整量算出了：

$$

d_i
=
c_i+\frac{\delta_{f,i}^*-\delta_{r,i}^*}{2}-x_m(0)

$$

这个 $d_i$ 是第 6、7、8 章的核心输入。

---

## 5.1 第 4 章如何影响第 5 章？

第 4 章输出：

$$

\delta_{f,i}^*,\delta_{r,i}^*

$$

第 5 章计算 gap 中心偏移：

$$

\Delta c_i=
\frac{\delta_{f,i}^*-\delta_{r,i}^*}{2}

$$

为什么是这个？

因为前车向前移动：

$$

+\delta_{f,i}^*

$$

后车向后移动：

$$

-\delta_{r,i}^*

$$

gap 中心是前后车位置平均值，所以中心变化是：

$$

\frac{(+\delta_{f,i}^*)+(-\delta_{r,i}^*)}{2}
=
\frac{\delta_{f,i}^*-\delta_{r,i}^*}{2}

$$

所以协作后目标中心相对合流车初始位置的偏移就是：

$$

d_i
=
c_i+\frac{\delta_{f,i}^*-\delta_{r,i}^*}{2}-x_m(0)

$$

这一步就是 4 章和 6、7、8 章之间的桥。fileciteturn0file0

---

## 5.2 第 6 章怎么用 $d_i$？

第 6 章轨迹写成：

$$

x_m(t)=x_m(0)+v_{ref}t+d_iS\left(\frac{t}{t_m}\right)

$$

这里直接用到了：

$$

d_i

$$

它决定合流车相对参考匀速轨迹要偏移多少。

如果：

$$

d_i>0

$$

说明合流车要追到参考轨迹前方。

如果：

$$

d_i<0

$$

说明合流车要落到参考轨迹后方。

如果：

$$

|d_i|

$$

很大，则合流车需要更大的速度变化和加速度变化。

---

## 5.3 第 7 章怎么用 $d_i$？

第 7 章每个时间公式都用到了 $d_i$。

无约束最优时间：

$$

t_{m,i}^{0}
=
\left(
\frac{3Kd_i^2}{w_t}
\right)^{1/4}

$$

加速度约束时间：

$$

t_{a,i}
=
\sqrt{
\frac{\frac{10}{\sqrt{3}}|d_i|}{a_{\lim}}
}

$$

速度约束时间：

$$

t_{v,i}
=
\begin{cases}
\dfrac{1.875|d_i|}{v_{\max}-v_{ref}}, & d_i\geq 0\\[8pt]
\dfrac{1.875|d_i|}{v_{ref}-v_{\min}}, & d_i<0
\end{cases}

$$

最终：

$$

t_{m,i}^*
=
\max(t_{m,i}^{0},t_{a,i},t_{v,i})

$$

所以第 4、5 章真正传给第 7 章的不是 $\delta_f,\delta_r$ 本身，而是它们共同决定出来的：

$$

d_i

$$

---

## 5.4 第 8 章怎么用 $d_i$ 和 $t_m^*$？

第 8 章合流点：

$$

p_{m,i}
=
x_m(0)+v_{ref}t_{m,i}^*+d_i

$$

所以你说“最优合流时间直接关系到最优合流位置，这两个是一体的”，这个理解是对的。

但要更准确地说：

> 对某个固定 gap 和固定协作调整量来说，$d_i$ 已经确定；一旦 $t_{m,i}^*$ 确定，合流位置 $p_{m,i}$ 就被唯一确定。

也就是说，在本文档中，第 8 章不是独立优化合流点，而是：

$$

t_m^*
\Rightarrow
p_m

$$

因为 gap 中心随着 $v_{ref}$ 匀速往前走，合流时间越晚，合流点自然越靠前。

---

# 6. 7、8 章可以有很多求解方法吗？

可以。

本文档的方法是：

1. 先固定轨迹形状为五次平滑函数；
2. 再把连续轨迹优化降维成只优化一个变量 $t_m$；
3. 最后得到闭式解。

这是一种很适合工程实现的方法，因为计算量小、可解释、无需调用复杂优化器。

但如果论文问题描述更一般，可以写成：

$$

\min_{i,t_i,p_i}
\quad
w_cC_i^{coop}
+
w_e
\left[
\frac{Kd_i^2}{t_i^3}+w_tt_i
\right]

$$

约束：

$$

p_i=x_m(0)+v_{ref}t_i+d_i

$$

$$

|a_m(t)|\leq a_{\lim}

$$

$$

v_{\min}\leq v_m(t)\leq v_{\max}

$$

$$

i\in\mathcal{G}_{coop}

$$

这里 $i$ 是离散的 gap 选择变量，$t_i$ 是连续合流时间变量，$p_i$ 是由时间决定的合流点。

如果把 gap 选择也放进优化里，这就是一个**离散-连续混合优化问题**。如果对每个 gap 单独求 $t_i^*$，再比较评分 $J_i$，就是本文档采用的分解式求解。

---

# 7. 这个方法是文献已有，还是原创？

我的判断是：

> 五次多项式轨迹、自由终端时间、速度/加速度约束、合流时间优化、候选 gap 评分，这些部件在文献中都有先例；但本文档这种“先协作扩 gap，再用协作后中心偏移 $d_i$ 推导一维闭式合流时间，再生成单帧最优轨迹”的完整组合，更像是一个工程化整理/特定算法设计，不宜直接说完全来自某一篇文献。

换句话说：

- **不是凭空原创所有数学部件**；
- **但具体组合方式、评分结构、协作调整闭式解，可能是本文档算法自己的设计**。

---

# 8. 相关文献：针对“合流时间/合流点/轨迹联合或半联合优化”的研究

下面这些是我筛出来比较贴近“on-ramp merging 中合流时间、合流点或候选 gap、轨迹优化”的文献。不是普通换道，也不是完全无关路径规划。

1. *Zhou, Y., Cholette, M. E., Bhaskar, A., & Chung, E. (2018). Optimal vehicle trajectory planning with control constraints and recursive implementation for automated on-ramp merging. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/8510865/)*
   这篇非常相关，核心是自动匝道合流中的轨迹优化，并包含控制约束和递归实现。它和你文档第 7 章的关系在于：都把合流轨迹看成带约束的优化问题，而不是简单经验轨迹。区别是，文档采用五次函数后把时间优化化成闭式公式。

2. *Zhou, Y., Cholette, M. E., Bhaskar, A., & Chung, E. (2018). Automated on-ramp merging and gap development with speed constraints: A state-constrained optimal control approach. American Control Conference. [PDF](https://eprints.qut.edu.au/120912/1/120912.pdf)*
   这篇直接涉及 on-ramp merging、gap development、speed constraints，并且明确关注 flexible terminal time / merge location 一类问题。它和你的问题非常接近：合流点和合流时间不是死固定，而是受状态约束、速度约束和 gap 发展影响。

3. *Gao, Z., Wu, Z., Hao, W., Long, K., & Byon, Y. J. (2021). Optimal trajectory planning of connected and automated vehicles at on-ramp merging area. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/9565856/)*
   这篇研究 CAV 在匝道合流区的最优轨迹规划，检索摘要中明确提到 optimize both merge time and vehicle trajectory。它比本文档更偏一般最优控制/轨迹优化，而本文档是固定五次轨迹族后求解析时间。

4. *Rios-Torres, J., & Malikopoulos, A. A. (2016). Automated and cooperative vehicle merging at highway on-ramps. IEEE Transactions on Intelligent Transportation Systems. [PDF](https://www.osti.gov/pages/servlets/purl/1311220)*
   这是 CAV 协同匝道合流的经典文献之一。它通常将合流控制写成最优控制问题，关注车辆在控制区内如何以能耗/时间等目标完成合流。它与本文档的共同点是“合流行为 = 约束优化问题”，不同点是本文档进一步把单 gap 的时间求解简化成一维闭式形式。

5. *Chen, J., Zhou, Y., & Chung, E. (2023). An integrated approach to optimal merging sequence generation and trajectory planning of connected automated vehicles for freeway on-ramp merging sections. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10268666/)*
   这篇把合流序列生成和轨迹规划结合起来。它更接近“先选谁进哪个 gap，再规划轨迹”的整体问题。你的文档则是“先筛 gap、算协作调整、算每个 gap 的时间和评分、选最优 gap”。

6. *Chen, N., van Arem, B., Alkim, T., & Wang, M. (2020). A hierarchical model-based optimization control approach for cooperative merging by connected automated vehicles. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/9145866/)*
   这篇采用分层优化思想处理协同合流。它和你的文档结构类似：上层做决策/协调，下层做轨迹或控制。但具体时间公式未必是你文档这种 $t_m^*=\max(t_0,t_a,t_v)$。

7. *Liu, H., Zhuang, W., Yin, G., & Li, Z. (2023). Safety-critical and flexible cooperative on-ramp merging control of connected and automated vehicles in mixed traffic. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10053376/)*
   这篇强调 safety-critical 和 flexible cooperative merging，在混合交通下考虑约束和可行性。它与本文档的联系是：合流时间和轨迹不是孤立算出来的，而要受安全约束、车辆能力约束和周围车辆行为影响。

8. *Liang, J., Guan, T., Liu, D., Liu, X., & Luan, Z. (2023). An optimal trajectory planning for automated on-ramp merging. IET Intelligent Transport Systems. [Link](https://ietresearch.onlinelibrary.wiley.com/doi/abs/10.1049/itr2.12308)*
   这篇明确使用 quintic polynomial 描述合流轨迹，并通过不同终端时间组合得到候选轨迹。它和文档第 6、7 章很接近：都是利用多项式轨迹，把终端时间作为关键决策变量。

9. *Le, V. A., Chalaki, B., Tzortzoglou, F. N., et al. (2024). Stochastic time-optimal trajectory planning for connected and automated vehicles in mixed-traffic merging scenarios. IEEE Transactions. [arXiv PDF](https://arxiv.org/pdf/2311.00126)*
   这篇关注 mixed-traffic merging 中的 stochastic time-optimal trajectory planning。它和你问的“最优合流时间”直接相关，不过它处理的是随机/混合交通不确定性，而文档是确定性单帧闭式计算。

10. *Zhao, Z., Wu, G., Wang, Z., & Barth, M. J. (2020). Optimal control-based eco-ramp merging system for connected and automated vehicles. IEEE Intelligent Vehicles Symposium. [arXiv PDF](https://arxiv.org/pdf/1910.7620)*
   这篇从 eco-driving 和 optimal control 角度处理匝道合流。它更强调能耗/效率目标，与文档中的

   $$

   \frac{Kd_i^2}{t_m^3}+w_tt_m

$$

   这种“控制能量 + 时间”的代价结构在思想上相近。

11. *Hu, X., & Sun, J. (2019). Trajectory optimization of connected and autonomous vehicles at a multilane freeway merging area. Transportation Research Part C: Emerging Technologies. [Link](https://www.sciencedirect.com/science/article/pii/S0968090X18304844)*
   这篇研究多车道 freeway merging area 中的 CAV 轨迹优化。它更复杂，通常会把车道选择、轨迹、安全约束一起考虑；你的文档则把问题拆成 gap 筛选、协作调整、时间计算、评分选择。

12. *Xie, Y., Lu, G., Zheng, F., & Cao, P. (2024). A hierarchical approach for integrating merging sequencing and trajectory optimization for connected and automated vehicles. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10414392/)*
   这篇是“merging sequencing + trajectory optimization”的层级优化框架，和你的算法整体流程很像：先解决合流对象/顺序/候选，再做轨迹优化。区别在于文档的第 7 章给了一个特别简洁的一维解析时间公式。

---

# 9. 文献中大家常用的几类解法

围绕“合流时间不确定、合流点不确定、轨迹要满足约束”这个具体问题，常见做法大概有四类。

第一类是**固定或候选合流点，然后优化合流时间和控制轨迹**。经典最优控制文献常这么做。合流点可能是合流区末端、冲突点、merge point，时间可以是自由终端时间或由调度层给出。

第二类是**候选 gap / 候选合流点枚举 + 每个候选点做轨迹优化**。你的文档就是这一类：先筛 gap，再对每个 gap 算 $d_i$、$t_i^*$、$p_i$、评分 $J_i$，最后选最优。

第三类是**合流序列、合流 gap、轨迹一起优化**。这类问题更完整，但通常变成混合整数优化、非线性规划或分层优化，求解复杂度更高。

第四类是**采用多项式、Bezier、B-spline 等轨迹参数化，然后优化终端时间或控制点**。这类方法工程上很常见，因为轨迹天然平滑，速度和加速度可以由导数直接得到。

你的文档属于第二类和第四类的结合：

$$

\text{候选 gap 枚举}
+
\text{协作调整}
+
\text{五次平滑轨迹}
+
\text{一维凸优化闭式时间解}

$$

---

# 10. 最后帮你重新梳理一遍主线

第 4 章算：

$$

\delta_{f,i}^*,\delta_{r,i}^*

$$

意思是前车、后车各动多少。

第 5 章把这两个量转成协作后 gap 中心偏移：

$$

d_i
=
c_i+\frac{\delta_{f,i}^*-\delta_{r,i}^*}{2}-x_m(0)

$$

第 6 章用 $d_i$ 构造平滑轨迹：

$$

x_m(t)=x_m(0)+v_{ref}t+d_iS(t/t_m)

$$

第 7 章用 $d_i$ 决定最优合流时间：

$$

t_{m,i}^*
=
\max(t_{m,i}^{0},t_{a,i},t_{v,i})

$$

第 8 章用 $d_i$ 和 $t_{m,i}^*$ 得到合流点：

$$

p_{m,i}
=
x_m(0)+v_{ref}t_{m,i}^*+d_i

$$

所以你的理解可以修正成这句话：

> 4、5 章先把“协作后 gap 中心在哪里”变成一个相对位移 $d_i$；6 章规定合流车如何平滑地走到这个相对位移；7 章在速度、加速度约束下选择最优合流时间；8 章再由这个时间反推出最终合流位置。
> 因此，合流时间和合流位置在本文档中是一体的；但真正连接前后章节的关键变量是 $d_i$。

-----

> 如果把 gap 选择也放进优化里，这就是一个离散-连续混合优化问题-给了我很多启发。
> 其实这篇文档就是在解决一个合流问题。我在这篇文档里面用的方法是,或者说想解决的问题，呈现的效果是,我可以在一个车流中找到一个gap,这个gap是整个车流中对于合流车协作调整起来最优的,同时又有可达性的筛选,速度安全的保障,加速度的保障等等。还有一个的话呢,也可以说是,最优合流时间,其实这个是和最优合流gap是一起的,就是整个文档或者是这篇理论报告就是在描述并解决这一个问题。
> 这篇文档的比较大的篇幅是在怎么求解这个问题,怎么去把它算出来,是一个很好的理论求解的文档。但是它不是一篇合格的期刊或会议论文,因为它对于问题的描述不够清晰,然后比较大的篇幅是在怎么求解上。但是对于期刊和会议论文更大的重心可能是放在了问题的建模和描述定义理论上,这个需要你去调研相关方向的期刊或会议至少十篇以上,把它们的表达方法,写作方法给理解起来。 我来总结一下的话,我在这篇理论文档里面看到的比较大的分步骤,不能说步骤,比较大的几个创新或者是优化问题,就是这样建模。第一个是可达性的分析,第二个是求解最优协同,协作调整量。 第三个是求解最优河流时间,然后你也提出如果把gap选择也放进优化里,就是一个离散连续混合优化问题。最后一个是轨迹的求解,得到必视解。 所以说这篇理论文其实是可以写成一篇比较好的期刊和会议,但是写作的风格以及就是描述问题,需要再调整一下,以及它的创新点的表述也是需要调整。 我需要你帮我先把这篇论文会议的逻辑和优化问题组织一下,汇报给我。先不要求你写一篇详细的论文,你先去理解我说的话,按照我理解的意思再去调研相关的期刊会议论文。 调研的时候,你要知道你想要找的是什么,你想学习的是什么,然后再把这篇文档里的内容给组织一下,形成一篇期刊会议论文的一个也不是大纲,也不是初稿,你就是把它的逻辑组织一下,就是怎么写能把它写好看。 写得有故事性,有逻辑性,有创新性。一定要逐个思考之后,才做下一步,不可以不思考,不调研就做下一步。

我先把你的意思重构成一个“论文问题”：不是单纯推公式，而是提出一个可达性约束下的协作 gap 选择、调整、合流时间与轨迹联合决策框架。接下来我会重点查三类文献：问题建模怎么讲、优化问题怎么拆、创新点怎么包装。

初步看，顶级写法通常不是“公式流水账”，而是先把问题定义成 merge-in gap selection、cooperative gap development、terminal time/location planning 的联合问题，再说明直接联合求解很难，于是提出可解释的分解式闭式求解。

我理解你的核心意思是：

> 这份文档现在像一份“公式推导与算法实现说明”，但如果要写成期刊/会议论文，不能把主线写成“我怎么算每一步”，而应该写成“我提出并解决了一个什么合流决策与轨迹规划问题”。

更适合的论文主线应该是：

$$

\boxed{
\text{在车流中选择最优可协作 gap，并同时确定协作调整量、合流时间、合流位置和可行轨迹}
}

$$

也就是说，论文的故事不是“第 2 章算可达，第 4 章算调整量，第 7 章算时间”，而是：

> 合流车面对多个候选 gap 时，需要判断哪些 gap 可达、哪些 gap 可通过协作扩张、每个 gap 需要多大协作代价、合流车应在何时何地进入该 gap，并保证速度、加速度、安全距离都满足约束。这个问题本质上是一个带离散 gap 选择和连续轨迹决策的混合优化问题；本文提出一种可解释、可闭式求解、适合实时实现的分解式方法。fileciteturn0file0

---

# 1. 从论文角度重新定义你的问题

现在不要先说“可达筛选怎么推公式”，而要先定义问题。

可以把问题命名为：

$$

\textbf{Reachability-aware Cooperative Gap Selection and Merging Trajectory Planning}

$$

中文可以叫：

> 面向协作合流的可达性约束 gap 选择与轨迹规划问题。

这个问题的输入是：

- 合流车初始状态；
- 主路目标车道上的车辆位置；
- 车辆速度、加速度约束；
- 安全 gap 要求；
- 协作车辆可调整能力；
- 合流时间窗或规划时间窗。

输出是：

$$

i^*,\quad
\delta_{f,i^*}^*,\quad
\delta_{r,i^*}^*,\quad
t_{m,i^*}^*,\quad
p_{m,i^*}^*,\quad
x_m(t)

$$

也就是：

- 最优 gap；
- 前车协作调整量；
- 后车协作调整量；
- 最优合流时间；
- 最优合流位置；
- 合流车完整轨迹。

这样一写，论文的对象就非常清楚了：**不是单纯轨迹规划，也不是单纯 gap 选择，而是 gap 选择、协作扩张、合流时空点、轨迹生成的一体化问题。**

---

# 2. 这个问题本质上可以写成一个混合离散-连续优化问题

你前面说“如果把 gap 选择也放进优化里”，这个想法非常关键。

因为 gap 索引：

$$

i\in\{1,2,\dots,N-1\}

$$

是离散变量。

而协作调整量：

$$

\delta_{f,i},\delta_{r,i}

$$

合流时间：

$$

t_{m,i}

$$

合流点：

$$

p_{m,i}

$$

轨迹：

$$

x_m(t)

$$

都是连续变量。

所以总问题可以抽象成：

$$

\min_{i,\delta_{f,i},\delta_{r,i},t_{m,i},p_{m,i},x_m(t)}
J_i

$$

其中：

$$

J_i
=
w_c C_i^{coop}
+
w_e C_i^{ego}

$$

约束包括：

$$

i\in\mathcal{G}

$$

$$

\text{gap } i \text{ 可达}

$$

$$

G_i+\delta_{f,i}+\delta_{r,i}\geq G^{req}

$$

$$

0\leq\delta_{f,i}\leq\bar{\delta}_{f,i}

$$

$$

0\leq\delta_{r,i}\leq\bar{\delta}_{r,i}

$$

$$

v_{\min}\leq v_m(t)\leq v_{\max}

$$

$$

a_{\min}\leq a_m(t)\leq a_{\max}

$$

$$

p_{m,i}=x_m(0)+v_{ref}t_{m,i}+d_i

$$

$$

d_i=c_i+\frac{\delta_{f,i}-\delta_{r,i}}{2}-x_m(0)

$$

这就是一个**离散-连续混合优化问题**。如果再把完整轨迹动力学作为连续时间约束写进去，它还带有最优控制问题的味道。

所以期刊/会议论文里可以先说：

> The original problem is a mixed discrete-continuous optimization problem, involving merge-in gap selection, cooperative gap development, terminal time/location determination, and dynamically feasible trajectory generation.

然后再说：

> Directly solving this problem as a monolithic MINLP/OCP is computationally expensive for real-time merging. Therefore, we decompose it into reachability screening, cooperative gap development, terminal-time optimization, and closed-form trajectory generation.

这就有论文味道了。

---

# 3. 你的文档真正的“创新模块”可以重组为四个

你总结得很准。现在可以把文档中的公式重新组织成四个贡献点。

---

## 创新点 1：可达性约束下的候选 gap 筛选

这部分不是简单判断：

$$

G_i\geq G^{req}

$$

而是判断：

> 合流车能否在时间窗内到达 gap 中心，并且到达时速度回到 $v_{ref}$。

这个点很重要。

因为很多简单方法会先看 gap 当前长度够不够，但你的文档强调：

$$

G_i<G^{req}

$$

并不意味着 gap 不可用，因为它可以通过协作扩张。

所以你可以把第 2 章包装成：

> A reachability-based candidate gap screening mechanism is proposed to avoid prematurely discarding gaps that are spatially insufficient but cooperatively expandable.

也就是：

> 提出可达性驱动的候选 gap 筛选方法，避免把当前长度不足但可协作扩张的 gap 提前误删。

这比“我算了 $t_{reach}$”更像论文贡献。

---

## 创新点 2：空间敏感的协作 gap 扩张优化

第 4 章不是简单分配：

$$

\delta_f+\delta_r=\Delta

$$

而是提出一个空间敏感权重：

$$

\gamma_{f,i}
=
\left(
\frac{\delta_{ref}}{\bar{\delta}_{f,i}}
\right)^q

$$

$$

\gamma_{r,i}
=
\left(
\frac{\delta_{ref}}{\bar{\delta}_{r,i}}
\right)^q

$$

然后解：

$$

\min_{\delta_{f,i},\delta_{r,i}}
\gamma_{f,i}\delta_{f,i}^2
+
\gamma_{r,i}\delta_{r,i}^2

$$

约束：

$$

\delta_{f,i}+\delta_{r,i}\geq\Delta_i

$$

$$

0\leq\delta_{f,i}\leq\bar{\delta}_{f,i}

$$

$$

0\leq\delta_{r,i}\leq\bar{\delta}_{r,i}

$$

这个可以包装成：

> A space-sensitive cooperative gap development model is formulated as a convex quadratic program, where the adjustment burden is automatically allocated according to the available upstream and downstream adjustment margins.

这句话比“4.8 是个二次凸优化问题”更有论文感。

它的亮点是：

- 不是平均分配；
- 不是人为指定谁动；
- 哪一侧外部空间更充足，就让哪一侧承担更多调整；
- 问题是凸的；
- 有闭式解析解；
- 适合实时实现。

---

## 创新点 3：协作后目标中心驱动的最优合流时间计算

第 7 章不能只说“从三个时间里取最大”。

应该先说：

> Once the cooperative gap development is determined, the adjusted gap center induces a relative terminal displacement $d_i$. The merging time is then optimized by balancing ego-vehicle control effort and merging efficiency under speed and acceleration constraints.

对应你的公式：

$$

d_i=
c_i+
\frac{\delta_{f,i}^*-\delta_{r,i}^*}{2}
-
x_m(0)

$$

这个 $d_i$ 是第 4、5 章和第 6、7、8 章之间的桥。

然后第 7 章写成一个一维优化问题：

$$

\min_{t_m>0}
\frac{K d_i^2}{t_m^3}
+
w_t t_m

$$

约束为：

$$

a_m^{peak}(t_m)\leq a_{\lim}

$$

$$

v_{\min}\leq v_m(t)\leq v_{\max}

$$

利用五次轨迹的性质：

$$

S'_{\max}=1.875

$$

$$

\max |S''|=\frac{10}{\sqrt{3}}

$$

可以把连续时间速度、加速度约束转化成时间下界：

$$

t_m\geq t_{v,i}

$$

$$

t_m\geq t_{a,i}

$$

所以闭式解为：

$$

t_{m,i}^*
=
\max(t_{m,i}^0,t_{a,i},t_{v,i})

$$

这样表达时，第 7 章就不是“经验取最大”，而是：

> 一个约束一维凸优化问题的解析解。

---

## 创新点 4：只对最优 gap 生成闭式可行轨迹

文档不是对所有 gap 都生成完整轨迹，而是：

$$

i^*=\arg\min_i J_i

$$

然后只对：

$$

i^*

$$

生成完整轨迹。

轨迹为：

$$

x_m(t)
=
x_m(0)+v_{ref}t+d^*S\left(\frac{t}{t_m^*}\right)

$$

前车：

$$

x_f(t)
=
x_f^*
+
v_{ref}t
+
\delta_f^*
S\left(\frac{t}{t_m^*}\right)

$$

后车：

$$

x_r(t)
=
x_r^*
+
v_{ref}t
-
\delta_r^*
S\left(\frac{t}{t_m^*}\right)

$$

这个可以包装成：

> A closed-form quintic trajectory generator is used only for the selected optimal gap, reducing unnecessary trajectory computation for rejected candidates while ensuring terminal position, speed, and acceleration continuity.

这也是工程价值：实时性和可复现性。

---

# 4. 论文的“故事线”应该怎么写

我建议你把整篇论文的逻辑写成下面这种故事。

---

## 第一层：现实问题

匝道合流中，合流车不是只需要“找最近的 gap”。

它要同时回答四个问题：

1. 哪个 gap 我能到达？
2. 哪个 gap 可以通过主路车辆协作扩张到安全长度？
3. 如果选择这个 gap，我应该什么时候合流、在哪里合流？
4. 能不能生成满足速度、加速度约束的平滑轨迹？

传统方法往往只处理其中一部分，例如：

- 固定合流点；
- 固定合流时间；
- 固定目标 gap；
- 只做轨迹优化；
- 只做合流序列优化；
- 不考虑 gap 可协作扩张；
- 不区分“当前 gap 不够”和“协作后 gap 可用”。

你的文章要强调：**你处理的是从候选 gap 到最终轨迹的一体化决策问题。**

---

## 第二层：理论困难

完整问题如果一次性写出来，会包含：

- 离散 gap 选择；
- 连续协作调整量；
- 连续合流时间；
- 连续合流点；
- 连续轨迹；
- 速度/加速度约束；
- gap 安全约束；
- 邻接车辆协作容量约束。

所以它天然是：

$$

\text{mixed discrete-continuous constrained optimization}

$$

甚至可以理解成：

$$

\text{MINLP + trajectory optimal control}

$$

如果直接求解，会比较重，不利于在线实时合流。

---

## 第三层：你的方法

你提出一个分解式框架：

$$

\boxed{
\text{Reachability Screening}
\rightarrow
\text{Cooperative Gap Development}
\rightarrow
\text{Terminal Time Optimization}
\rightarrow
\text{Gap Scoring and Selection}
\rightarrow
\text{Closed-form Trajectory Generation}
}

$$

这个顺序和文档原流程一致，但论文里表达的重点不一样。

文档现在是“怎么一步步算出来”。

论文应该写成：

> 为了实时求解原始混合优化问题，本文将其分解为若干具有明确物理意义且可解析求解的子问题。

这个“分解”就是文章的主线。

---

# 5. 可以把总优化问题先写出来，再说如何分解

这是让论文更高级的关键。

可以先定义候选 gap 集合：

$$

\mathcal{G}=\{1,2,\dots,N-1\}

$$

对每个 gap $i$，定义：

$$

z_i\in\{0,1\}

$$

其中：

$$

z_i=1

$$

表示选择 gap $i$。

要求只选择一个 gap：

$$

\sum_{i\in\mathcal{G}} z_i=1

$$

总目标可以写成：

$$

\min_{\{z_i,\delta_{f,i},\delta_{r,i},t_{m,i}\}}
\sum_{i\in\mathcal{G}}
z_i
\left[
w_c C_i^{coop}
+
w_e C_i^{ego}
\right]

$$

其中：

$$

C_i^{coop}
=
\gamma_{f,i}\delta_{f,i}^2
+
\gamma_{r,i}\delta_{r,i}^2

$$

$$

C_i^{ego}
=
\frac{K d_i^2}{t_{m,i}^3}
+
w_t t_{m,i}

$$

约束包括：

$$

z_i\in\{0,1\}

$$

$$

\sum_i z_i=1

$$

$$

z_i=1\Rightarrow t_{reach,i}\leq T

$$

$$

z_i=1\Rightarrow G_i+\delta_{f,i}+\delta_{r,i}\geq G^{req}

$$

$$

0\leq\delta_{f,i}\leq\bar{\delta}_{f,i}

$$

$$

0\leq\delta_{r,i}\leq\bar{\delta}_{r,i}

$$

$$

d_i=c_i+\frac{\delta_{f,i}-\delta_{r,i}}{2}-x_m(0)

$$

$$

t_{m,i}\geq t_{a,i}

$$

$$

t_{m,i}\geq t_{v,i}

$$

这样一写，文章问题就清楚了。

然后再说：

由于直接求解上述混合优化问题不利于实时应用，本文采用分解式解析求解：

1. 先用可达性筛掉不可能 gap；
2. 对每个可达 gap 解一个二变量凸 QP，得到协作调整量；
3. 由协作后中心偏移 $d_i$ 解一个一维凸优化，得到合流时间；
4. 用总成本选最优 gap；
5. 对最优 gap 生成闭式轨迹。

这样论文就从“公式堆叠”变成“一个复杂优化问题的结构化求解”。

---

# 6. 相关文献中常见的写作方式

我调研后看到，相关论文常见的写法大概有几类。

---

## 类型 A：把问题写成合流序列 + 轨迹规划联合优化

这类论文通常先强调：单独优化轨迹不够，因为合流顺序或合流 gap 决定了终端条件。

代表文献：

*Chen, J., Zhou, Y., & Chung, E. (2023). An integrated approach to optimal merging sequence generation and trajectory planning of connected automated vehicles for freeway on-ramp merging sections. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10268666/)*

这篇的表达方式值得学：它不是从某个轨迹公式开始，而是先提出“merging sequence generation and trajectory planning”是耦合问题，并将 merge-in gap selection 放进整体框架。检索结果显示它把问题建成 MINLP，并关注 on-ramp vehicles 的 optimal merge-in gaps。你的论文可以借鉴这种写法，但你的方法更偏单车单帧下的可解释闭式求解。

---

## 类型 B：把合流看成 gap development + trajectory planning

这类论文很接近你的思想，因为它们不是只让合流车自己动，而是考虑主路车辆协作创造 gap。

代表文献：

*Zhou, Y., Cholette, M. E., Bhaskar, A., & Chung, E. (2018). Optimal vehicle trajectory planning with control constraints and recursive implementation for automated on-ramp merging. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/8510865/)*

*Zhou, Y., Cholette, M. E., Bhaskar, A., & Chung, E. (2018). Automated on-ramp merging and gap development with speed constraints: A state-constrained optimal control approach. American Control Conference. [PDF](https://eprints.qut.edu.au/120912/1/120912.pdf)*

这两篇非常值得学。它们的关键词是：

$$

\text{on-ramp merging}
+
\text{gap development}
+
\text{control constraints}
+
\text{free terminal time}

$$

你的文章也应该强调 gap development，而不是只说 gap selection。你的第 4 章其实就是一个“cooperative gap development”模型。

---

## 类型 C：把合流时间作为自由终端时间优化

这类论文不是固定合流时间，而是让合流时间由优化决定。

代表文献：

*Gao, Z., Wu, Z., Hao, W., Long, K., & Byon, Y. J. (2021). Optimal trajectory planning of connected and automated vehicles at on-ramp merging area. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/9565856/)*

这类论文通常会说：合流时间和轨迹耦合，合流时间影响能耗、安全和效率。你的第 7 章正好可以放在这个语境下：不是“取三个时间最大值”，而是“free terminal time optimization under speed and acceleration constraints”。

---

## 类型 D：分层优化框架

这类论文通常把复杂问题分成上层决策和下层控制。

代表文献：

*Chen, N., van Arem, B., Alkim, T., & Wang, M. (2020). A hierarchical model-based optimization control approach for cooperative merging by connected automated vehicles. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/9145866/)*

*Xie, Y., Lu, G., Zheng, F., & Cao, P. (2024). A hierarchical approach for integrating merging sequencing and trajectory optimization for connected and automated vehicles. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10414392/)*

你的文章也可以采用“hierarchical”或“decomposed”这个写法，但我建议不要强行叫 hierarchical control，除非你后面真的有控制器。可以叫：

$$

\text{decomposition-based analytical planning framework}

$$

也就是“分解式解析规划框架”。

---

## 类型 E：多项式轨迹 / 五次轨迹参数化

这类论文会用 quintic polynomial、Bezier、B-spline 等轨迹形式来降低轨迹规划复杂度。

代表文献：

*Liang, J., Guan, T., Liu, D., Liu, X., & Luan, Z. (2023). An optimal trajectory planning for automated on-ramp merging. IET Intelligent Transport Systems. [Link](https://ietresearch.onlinelibrary.wiley.com/doi/abs/10.1049/itr2.12308)*

*Hu, Y., Yu, C., Su, Z., Ma, W., & Chen, Z. (2024). Critical trajectory point planning for connected and autonomous vehicles on freeway on-ramps under mixed traffic environment. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10614844/)*

你的第 6 章属于这一类。但你的亮点不是“我用了五次多项式”，因为五次多项式本身很常见；你的亮点是：

> 五次轨迹与协作后 gap 中心偏移 $d_i$ 绑定，并进一步导出速度、加速度约束下的闭式合流时间。

这才是更强的表述。

---

## 类型 F：安全关键 / 混合交通 / 鲁棒性

这类论文更强调安全约束、混合交通不确定性、通信延迟等。

代表文献：

*Liu, H., Zhuang, W., Yin, G., & Li, Z. (2023). Safety-critical and flexible cooperative on-ramp merging control of connected and automated vehicles in mixed traffic. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10053376/)*

*Fang, Y., Min, H., Wu, X., & Wang, W. (2022). On-ramp merging strategies of connected and automated vehicles considering communication delay. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/9678122/)*

你的文档目前主要是确定性单帧规划，不要过度宣称能解决混合交通不确定性或通信延迟。但可以在 introduction 里说：相比更复杂的鲁棒/随机方法，本文关注一个可解释、可实时实现的基础规划模块。

---

## 类型 G：综述类，用来找研究空白

代表文献：

*Zhu, J., Easa, S., & Gao, K. (2022). Merging control strategies of connected and autonomous vehicles at freeway on-ramps: A comprehensive review. Journal of Intelligent and Connected Vehicles. [PDF](https://www.emerald.com/jicv/article-pdf/5/2/99/1459412/jicv-02-2022-0005.pdf)*

*Viscio, S., Giuffrida, N., Marinelli, M., & Binetti, M. (2026). Optimization of Highway On-Ramp Merging Maneuvers in Pure and Mixed CAV Environments: An Overview. [Link](https://www.intechopen.com/online-first/1238955)*

综述类文献可以帮助你写 introduction：已有方法包括 rule-based、optimal control、MPC、game-theoretic、mixed-integer optimization、trajectory parameterization 等，但在“实时 gap 选择 + 协作扩张 + 合流时间闭式求解 + 轨迹可行性”之间仍有空间。

---

# 7. 至少 10 篇相关文献可以怎么服务你的文章

下面不是单纯列文献，而是告诉你每类文献该怎么用。

1. *Rios-Torres, J., & Malikopoulos, A. A. (2016). Automated and cooperative vehicle merging at highway on-ramps. IEEE Transactions on Intelligent Transportation Systems. [PDF](https://www.osti.gov/pages/servlets/purl/1311220)*
   用来说明 CAV 匝道合流可以建模为最优控制问题，这是领域基础。

2. *Zhou, Y., Cholette, M. E., Bhaskar, A., & Chung, E. (2018). Optimal vehicle trajectory planning with control constraints and recursive implementation for automated on-ramp merging. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/8510865/)*
   用来说明“控制约束 + 合流轨迹 + 递归实现”是主流写法。

3. *Zhou, Y., Cholette, M. E., Bhaskar, A., & Chung, E. (2018). Automated on-ramp merging and gap development with speed constraints: A state-constrained optimal control approach. American Control Conference. [PDF](https://eprints.qut.edu.au/120912/1/120912.pdf)*
   用来支撑你的“gap development”概念，尤其是主路车辆协作形成 gap。

4. *Gao, Z., Wu, Z., Hao, W., Long, K., & Byon, Y. J. (2021). Optimal trajectory planning of connected and automated vehicles at on-ramp merging area. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/9565856/)*
   用来说明 merge time 和 trajectory 可以联合优化。

5. *Chen, N., van Arem, B., Alkim, T., & Wang, M. (2020). A hierarchical model-based optimization control approach for cooperative merging by connected automated vehicles. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/9145866/)*
   用来支撑“复杂合流问题可采用分层/分解式建模”。

6. *Chen, J., Zhou, Y., & Chung, E. (2023). An integrated approach to optimal merging sequence generation and trajectory planning of connected automated vehicles for freeway on-ramp merging sections. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10268666/)*
   用来支撑“merge-in gap selection 可以作为优化变量”，并说明直接联合建模常形成 MINLP。

7. *Liang, J., Guan, T., Liu, D., Liu, X., & Luan, Z. (2023). An optimal trajectory planning for automated on-ramp merging. IET Intelligent Transport Systems. [Link](https://ietresearch.onlinelibrary.wiley.com/doi/abs/10.1049/itr2.12308)*
   用来支撑五次多项式轨迹规划在 automated on-ramp merging 中是常见技术路线。

8. *Liu, H., Zhuang, W., Yin, G., & Li, Z. (2023). Safety-critical and flexible cooperative on-ramp merging control of connected and automated vehicles in mixed traffic. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10053376/)*
   用来说明 safety-critical constraints 和 flexible merging 是当前热点。

9. *Zhao, Z., Wu, G., Wang, Z., & Barth, M. J. (2020). Optimal control-based eco-ramp merging system for connected and automated vehicles. IEEE Intelligent Vehicles Symposium. [arXiv PDF](https://arxiv.org/pdf/1910.7620)*
   用来支撑“能耗/控制 effort + 时间效率”的目标函数设计。

10. *Xie, Y., Lu, G., Zheng, F., & Cao, P. (2024). A hierarchical approach for integrating merging sequencing and trajectory optimization for connected and automated vehicles. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10414392/)*
   用来支撑“序列/选择 + 轨迹优化”的集成框架。

11. *Zhu, J., Easa, S., & Gao, K. (2022). Merging control strategies of connected and autonomous vehicles at freeway on-ramps: A comprehensive review. Journal of Intelligent and Connected Vehicles. [PDF](https://www.emerald.com/jicv/article-pdf/5/2/99/1459412/jicv-02-2022-0005.pdf)*
   用来写 literature review 和 research gap。

12. *Shi, J., Luo, Y., Li, P., Wang, J., & Li, K. (2024). Collaborative multi-lane on-ramp merging strategy for connected and automated vehicles using dynamic conflict graph. IEEE Transactions on Intelligent and Connected Vehicles. [Link](https://ieeexplore.ieee.org/abstract/document/10499221/)*
   用来说明更复杂场景会发展成图模型、MINLP 或多车协作，但你的方法更强调单帧、单合流车、实时闭式规划。

---

# 8. 你的文章应该避免怎么写

不要写成：

> 第一步，我们计算 $D_i$。第二步，我们计算 $v_p$。第三步，我们计算 $s_{acc}$。第四步……

这样像技术文档，不像论文。

也不要把贡献写成：

> 本文提出了一堆公式。

而应该写成：

> 本文提出一种面向协作合流的可达性约束 gap 选择与解析轨迹规划框架。该框架将原始混合离散-连续优化问题分解为可达性筛选、空间敏感协作 gap 扩张、一维终端时间优化和闭式轨迹生成四个可解释子问题。

这个表述更适合期刊/会议。

---

# 9. 文章可以这样组织逻辑

你现在的理论文档可以重组为下面这种论文逻辑。

---

## 1 Introduction

先讲背景：

匝道合流是高速公路交通效率和安全的关键瓶颈。CAV 可以通过 V2X 与主路车辆协作，但合流车面临多个候选 gap。不同 gap 的可达性、协作成本、合流时间和轨迹可行性不同。

然后指出不足：

现有研究常常固定合流点或合流时间，或只优化轨迹，或只做合流序列，或没有显式区分“当前 gap 不足”和“可协作扩张 gap”。

然后提出本文目标：

$$

\text{select the best reachable and cooperatively expandable gap}

$$

同时确定：

$$

\delta_f,\delta_r,t_m,p_m,x_m(t)

$$

---

## 2 Problem Statement

这一节是你现在文档最缺的。

应该先定义场景：

- 单个合流车；
- 目标车道有 $N$ 辆车；
- 相邻车辆形成候选 gap；
- 目标车道车辆初始匀速；
- 合流车需要进入某一个 gap；
- 允许 gap 前后边界车辆协作调整。

然后定义决策变量：

$$

i,\delta_f,\delta_r,t_m,p_m,x_m(t)

$$

再定义约束：

- 可达性；
- gap 安全；
- 相邻 gap 安全；
- 速度约束；
- 加速度约束；
- 轨迹边界条件。

最后写出原始混合优化问题。

这一节的目标是让读者明白：

> 你不是在随便算公式，而是在求解一个结构清晰的合流优化问题。

---

## 3 Decomposition-Based Analytical Solution

这一节才开始讲你的方法。

可以分成四个子节：

### 3.1 Reachability-aware candidate gap screening

讲第 2 章。

重点不是公式细节，而是说明：

$$

t_{reach,i}\leq T

$$

用于判断 gap 是否物理可达。

并强调：

$$

G_i\geq G^{req}

$$

不作为前置筛选条件，因为 gap 可以协作扩张。

---

### 3.2 Space-sensitive cooperative gap development

讲第 4、5 章。

先定义缺口：

$$

\Delta_i=\max(0,G^{req}-G_i)

$$

再定义容量：

$$

\bar{\delta}_{f,i},\bar{\delta}_{r,i}

$$

再定义空间敏感权重：

$$

\gamma_{f,i},\gamma_{r,i}

$$

再建立 QP：

$$

\min
\gamma_f\delta_f^2+\gamma_r\delta_r^2

$$

最后给闭式解。

这一节要强调：

> 这是一个严格凸 QP，具有唯一全局最优解，并且可以解析求解。

---

### 3.3 Terminal displacement and merging time optimization

讲第 5、6、7、8 章的核心联系。

先说：

协作调整改变 gap 中心：

$$

d_i=c_i+\frac{\delta_f^*-\delta_r^*}{2}-x_m(0)

$$

然后合流轨迹采用：

$$

x_m(t)=x_m(0)+v_{ref}t+d_iS(t/t_m)

$$

再写时间优化：

$$

\min_{t_m>0}
\frac{Kd_i^2}{t_m^3}+w_tt_m

$$

约束：

$$

v_{\min}\leq v_m(t)\leq v_{\max}

$$

$$

a_{\min}\leq a_m(t)\leq a_{\max}

$$

然后得到：

$$

t_m^*=\max(t_m^0,t_a,t_v)

$$

再得到：

$$

p_m=x_m(0)+v_{ref}t_m^*+d_i

$$

这一节要强调：

> 合流时间和合流位置不是分别独立求的，而是由同一个终端位移 $d_i$ 和目标 gap 运动共同决定。

---

### 3.4 Gap scoring and final trajectory generation

讲评分和最优 gap：

$$

J_i=w_cC_i^{coop}+w_eC_i^{ego}

$$

$$

i^*=\arg\min_i J_i

$$

然后只对 $i^*$ 生成完整轨迹。

---

# 10. 这篇文章的“创新点”可以这样写

我建议你不要写太多“首次提出”。除非后面严格对比证明。可以稳妥地写成：

**Contribution 1.**
提出一种可达性约束的候选 gap 筛选方法，将合流车的速度和加速度能力转化为同速到达时间判据，避免提前排除可协作扩张但当前长度不足的 gap。

**Contribution 2.**
提出一种空间敏感的协作 gap 扩张模型，将前后边界车辆的调整量分配建模为带线性约束的凸二次规划，并给出闭式解析解。

**Contribution 3.**
提出一种由协作后 gap 中心偏移驱动的最优合流时间计算方法，将五次平滑轨迹下的速度、加速度约束转化为显式时间下界，得到一维约束凸优化问题的闭式解。

**Contribution 4.**
构建一个统一评分函数，在协作代价和自车合流代价之间进行权衡，实现最优 gap、合流时间、合流位置和轨迹的一体化选择。

这样写比“本文提出可达性分析、协作调整量、合流时间、轨迹求解”更像论文贡献。

---

# 11. 最后给你一个核心论文叙事版本

可以把整篇文章的逻辑压缩成这样：

> 本文研究单个合流车在目标车道车流中选择最优协作 gap 的问题。与固定合流 gap 或固定合流点的传统方法不同，本文同时考虑候选 gap 的可达性、主路车辆协作扩张能力、合流车速度/加速度约束以及合流效率。原始问题可表述为一个包含离散 gap 选择和连续协作调整、合流时间、合流位置、轨迹变量的混合优化问题。为满足实时规划需求，本文提出一种分解式解析求解框架：首先基于同速到达时间筛选可达 gap；随后针对每个可达 gap，构建空间敏感的协作 gap 扩张凸二次规划，并给出解析解；接着基于协作后 gap 中心偏移，建立一维约束合流时间优化问题，将速度和加速度约束转化为显式时间下界；最后通过统一代价函数选择最优 gap，并生成满足终端位置、速度、加速度连续性的五次平滑轨迹。

这个版本已经接近期刊/会议摘要或 introduction 末尾的写法。

---

# 12. 我的判断

你这份文档是有论文潜力的，但当前形态确实更像：

$$

\text{algorithm derivation notes}

$$

而不是：

$$

\text{research paper}

$$

它需要做的不是推翻重写，而是**换一个叙事层级**：

- 从“公式步骤”上升到“优化问题”；
- 从“怎么求解”上升到“为什么这样建模”；
- 从“每章算什么”上升到“每个子问题在原始混合优化问题中承担什么角色”；
- 从“轨迹公式”上升到“终端时间/位置/轨迹可行性的闭式解析规划”。

真正的主线可以定为：

$$

\boxed{
\text{A reachability-aware, cooperation-sensitive, analytically solvable framework for optimal merge-in gap selection and trajectory generation}
}

$$

中文就是：

$$

\boxed{
\text{一种可达性约束、协作敏感、可解析求解的最优合流 gap 选择与轨迹生成框架}
}

$$

这个定位比“合流轨迹规划算法”更准确，也更有创新表达空间。

-----

> 我需要你帮我做像是审稿人一样来帮我审稿这篇论文,但是你先不用审具体内容,你先帮我判断它解决的问题,提出的优化问题,给出的解决方法,然后想要达成的效果,这个故事在现有的文献中它是不是可以发表,是不是有创新性的?你已经调研过很多期刊和会议的论文,但我不知道你记忆中有没有把它们保存下来去帮我做一次这样的调研。如果没有的话,我希望你至少调研20到30篇的论文或期刊会议,并把它们如果有记忆中的话,我希望也是补充到20到30篇以上的个水平,来判断我这个故事,这个论文,这个理论的创新性能不能支撑发表一篇期刊或会议论文。你需要作为审稿人来思考这件事情,你要严谨,要严苛,你不能顺从我,也不能刻意反驳我,总之就是你要公平公正,并且数据要真实可靠。如果可以的话,你也可以把整个故事再给我梳理一下,就不用再给我分点什么的,你就给我一个核心的论文叙事版本,把这个故事描述得更好的,可以更详细一点,就主要是讲故事。如果不可以的话,你觉得哪里再调整一下就可以实现发一篇期刊或者会议论文。我的要求不是发顶刊,也不是发顶会。一定要严谨调研深度思考，不可以偷懒

我会按审稿人的口径来判断：先把你的问题抽象成论文问题，再用近期同方向论文对照“新意在哪里、风险在哪里、怎样改写才更像可发表论文”。

我会先基于你上传的理论文档和我已有的领域知识做审稿式判断；由于当前无法进行实时联网检索，我不会伪装成刚刚完成了新的在线调研。

我会用审稿人的标准来做判断：不先假设它能发，而是对照同类合流、gap selection、cooperative merging、terminal-time planning 文献，看它的“问题—方法—贡献”是否站得住。

我会按“审稿意见”的方式来做：先核对你文档里的真实问题与方法，再对照合流优化文献判断它的新意、可发表性和风险点。

我会先把上传文档的“问题—模型—求解链条”核对一遍，再用同方向文献做审稿式对照；我会明确区分“已有常见做法”和“可能成立的新意”。

我已经核对到文档本身的定位：它明确是 One-Step 单帧算法核心，完成可达 gap 筛选、协作调整、合流时间与评分计算，并只对最优 gap 生成完整轨迹。接下来我把它放到“论文贡献是否足够”的框架里评审。

目前对照结果已经显示：候选 merge-in gap、合流时间、合流序列、轨迹优化这些主题在文献里都很成熟；真正需要审查的是你这篇是否把它们组合成了一个有辨识度的问题，而不是把常见模块简单拼接。

我会再补一组更偏“轨迹参数化/五次多项式/评分选择”的论文，避免只拿最优控制和合流序列论文来判断。这样审稿判断会更公平。

先给出审稿式结论：**这个故事有发表潜力，但当前文档形态本身还不能直接支撑一篇合格期刊/会议论文。** 它现在更像“单帧合流算法的公式推导说明”，而论文需要把它上升为一个清楚的优化问题：**在多个候选 gap 中，联合决定最优合流 gap、协作调整量、合流时间、合流位置和可行轨迹**。如果按这个角度重写，并补充严谨实验，它可以支撑普通会议或应用型期刊；若想投较强 T-ITS/TR-C/IEEE T-IV 一类，需要更强的理论证明、对比实验和场景泛化。

我会很严苛地说：**“最优 gap 选择 + 合流时间 + 轨迹规划”这个大问题本身不是全新问题。** 文献里已经有很多工作做了 merge-in gap selection、merging sequence、free terminal time、trajectory optimization、cooperative gap development。例如 Chen et al. 已经把 optimal merge-in gaps 和 trajectory planning 集成在 CAV 匝道合流框架里；Zhou et al. 已经研究 free terminal time / flexible merge location 的 gap development 与轨迹优化；Gao et al. 已经明确优化 merge time 和 vehicle trajectory。你的创新不能写成“首次解决最优合流 gap 和时间问题”，这个会被审稿人直接打回来。

但你的文档仍然有一个可以成立的论文故事：**它不是追求一个大而重的 MINLP/OCP 求解器，而是提出一个可达性约束、协作敏感、可解析求解的 one-step gap selection and trajectory planning framework。** 这个定位比“我提出了最优合流算法”更安全，也更有辨识度。

---

你这篇论文真正解决的问题，可以这样定义：

给定一个局部单帧合流场景，主路目标车道上有一串车辆，相邻车辆形成多个候选 gap。合流车不应该只选择当前最大的 gap，也不应该只选择最近的 gap，因为有些 gap 当前不够大但可以通过前后车协作扩张；有些 gap 空间够大但合流车在时间窗内不可达；有些 gap 虽然可达，但需要合流车剧烈加减速，或者需要主路车辆做过大的协作调整。因此，问题的核心不是单纯轨迹规划，而是：

$$

\boxed{
\text{在多个候选 gap 中选择一个综合最优的可达、可协作、安全、可执行合流 gap}
}

$$

这个问题的决策变量包括：

$$

i,\quad \delta_{f,i},\quad \delta_{r,i},\quad t_{m,i},\quad p_{m,i},\quad x_m(t)

$$

其中 $i$ 是离散 gap 选择变量，$\delta_{f,i},\delta_{r,i}$ 是连续协作调整量，$t_{m,i}$ 是连续合流时间，$p_{m,i}$ 是合流位置，$x_m(t)$ 是轨迹。因此，从优化理论角度，它天然可以被表述为一个**离散-连续混合优化问题**：

$$

\min_{i,\delta_f,\delta_r,t_m,x_m(\cdot)}
J_i

$$

其中一个合理目标是：

$$

J_i
=
w_c
\left(
\gamma_{f,i}\delta_{f,i}^2+
\gamma_{r,i}\delta_{r,i}^2
\right)
+
w_e
\left(
\frac{K d_i^2}{t_{m,i}^3}
+
w_t t_{m,i}
\right)

$$

约束包括：

$$

i\in\mathcal{G}

$$

$$

t_{reach,i}\leq T

$$

$$

G_i+\delta_{f,i}+\delta_{r,i}\geq G^{req}

$$

$$

0\leq \delta_{f,i}\leq \bar{\delta}_{f,i}

$$

$$

0\leq \delta_{r,i}\leq \bar{\delta}_{r,i}

$$

$$

d_i=c_i+\frac{\delta_{f,i}-\delta_{r,i}}{2}-x_m(0)

$$

$$

v_{\min}\leq v_m(t)\leq v_{\max}

$$

$$

a_{\min}\leq a_m(t)\leq a_{\max}

$$

这个总问题如果直接求解，会变成混合整数优化加轨迹优化，实时性和可解释性都不好。你的文档的价值就在这里：**它不是直接求一个庞大的黑箱优化，而是把原问题分解成几个可解析的小问题。**

---

作为审稿人，我会把你的方法理解成下面这个链条。

第一步，不用当前 gap 长度做硬筛选，而是先做**同速可达性筛选**。这点有一定新意，因为你明确避免了 $G_i\geq G^{req}$ 的前置硬筛选。很多工程方法会先把当前 gap 不足的候选删掉，但你的逻辑是：当前 gap 不足不等于不可用，因为后续可以通过主路车辆协作扩张。这个点如果在论文里讲清楚，是一个比较好的问题切入点。

第二步，对每个可达 gap，求协作扩张量。你不是平均分配前车和后车调整量，而是用外侧可调整空间定义权重：

$$

\gamma_{f,i}=
\left(
\frac{\delta_{ref}}{\bar{\delta}_{f,i}}
\right)^q,\qquad
\gamma_{r,i}=
\left(
\frac{\delta_{ref}}{\bar{\delta}_{r,i}}
\right)^q

$$

然后解一个凸二次规划：

$$

\min_{\delta_{f,i},\delta_{r,i}}
\gamma_{f,i}\delta_{f,i}^2+
\gamma_{r,i}\delta_{r,i}^2

$$

这部分的可发表价值不在于“用了 QP”，因为 QP 很常见；价值在于你把“哪一侧更适合承担协作调整”编码成空间敏感权重，并且给出了闭式解析解。这个比较适合包装为：

$$

\textbf{space-sensitive cooperative gap development}

$$

第三步，协作调整量改变 gap 中心，于是得到：

$$

d_i=
c_i+\frac{\delta_{f,i}^*-\delta_{r,i}^*}{2}-x_m(0)

$$

这是你整篇文章里非常关键的桥梁。第 4 章不是孤立算协作量；它真正给第 6、7、8 章提供的是 $d_i$。这个 $d_i$ 决定合流车相对于参考匀速轨迹要提前多少或落后多少。

第四步，基于五次平滑轨迹：

$$

x_m(t)=x_m(0)+v_{ref}t+d_iS(t/t_m)

$$

把合流时间写成一维约束优化问题：

$$

\min_{t_m>0}
\frac{K d_i^2}{t_m^3}+w_t t_m

$$

并在速度、加速度约束下得到：

$$

t_{m,i}^*=\max(t_{m,i}^{0},t_{a,i},t_{v,i})

$$

这一点也不是凭空全新，因为 free terminal time / flexible merge location 在文献里已有很多；但你的闭式时间解配合前面的 $d_i$ 和 gap 评分，形成了一个轻量化算法。这可以作为工程实时性贡献，而不是理论原创性贡献。

第五步，对每个候选 gap 计算综合评分：

$$

J_i=w_cC_i^{coop}+w_eC_i^{ego}

$$

然后：

$$

i^*=\arg\min_i J_i

$$

最后只对最优 gap 生成完整轨迹。这个做法的优点是计算量低、结构清楚、易复现；缺点是它是分解式近似，不能宣称求解了原始混合优化问题的全局最优解。

---

和已有文献相比，审稿人会这样看。

已有文献已经覆盖了很多相近主题。Rios-Torres and Malikopoulos 把自动协同合流建成优化框架，并给出解析/最优控制思路；Zhou et al. 研究了带控制约束的自动匝道合流轨迹规划；Zhou et al. 的 ACC 工作已经明确讨论 automated on-ramp merging and gap development with speed constraints，并且有 flexible terminal time / merge location；Gao et al. 直接优化 merge time 和 vehicle trajectory；Chen et al. 把 merging sequence generation、optimal merge-in gap 和 trajectory planning 集成起来；Xie et al. 也做了 merging sequencing and trajectory optimization 的层级框架。这些文献会让你的“大问题”看起来不新。

但是，你的可竞争点是：这些方法往往求解更重的 OCP、MINLP、iterative LP、hierarchical optimization 或 receding horizon control。你的方法更像一个**单帧、可解释、闭式求解、候选 gap 枚举式的实时规划器**。如果你把创新点压在“轻量、解析、可达性前筛、空间敏感协作分配、合流时间闭式下界”上，而不是压在“首次联合优化 gap/time/trajectory”上，论文故事会站得住。

---

我认为当前故事的创新性强度是：**中等偏弱到中等，取决于实验和写法。**

如果只提交现在这份理论文档，审稿结果大概率是拒稿。原因不是公式错，而是论文问题没有充分上升，文献差异不清楚，实验验证缺失，很多参数看起来像工程调参。

如果你把它改成一篇完整论文，贡献写成：

> A reachability-aware and cooperation-sensitive analytical framework for optimal merge-in gap selection and trajectory generation

并且实验充分，那么它有机会支撑普通会议或应用型期刊。

如果想冲更好的期刊，需要至少补三件事：

第一，证明你的分解式方法相对于原始混合优化问题的关系。你不能说它是全局最优，只能说它是“within-candidate analytical optimum”或“decomposition-based optimum under the proposed scoring structure”。最好证明每个子问题的最优性：协作调整量是凸 QP 的全局最优；合流时间是一维凸优化的全局最优；gap 选择是在候选集合上枚举评分的全局最小。

第二，补强安全性。现在文档有终端 gap、安全距离、速度、加速度约束，但审稿人会问：合流过程中是否全时域安全？前车/后车协作轨迹是否会在中途压缩相邻 gap 到不安全？$G^{adj}$ 是否只保证终端，还是保证整个 $t\in[0,t_m]$？如果你用同一个 $S(\tau)$ 单调变化，可以尝试证明动态 gap 长度的最小值出现在终端或起点，从而给出全时域安全保证。

第三，实验必须有对比。至少需要和以下 baseline 比较：最近 gap、最大 gap、当前满足 $G^{req}$ 的 gap、平均分配协作调整量、固定合流时间、通用 QP/NLP 求解器、无可达性筛选版本、无空间敏感权重版本。指标包括成功率、协作调整总量、最大加速度、最大速度偏差、合流时间、计算时间、舒适性、对主路扰动。

---

我会把你的核心论文叙事写成这样：

匝道合流中的核心挑战并不是简单地生成一条平滑轨迹，而是在动态车流中判断合流车应该进入哪一个 gap。一个看似足够大的 gap 可能在车辆能力约束下不可达；一个当前长度不足的 gap 可能通过前后车辆的轻微协作扩张成为更优选择；一个距离合流车较近的 gap 可能需要过大的速度变化；一个协作代价低的 gap 也可能导致过长的合流时间。因此，合流决策应同时考虑候选 gap 的可达性、协作扩张成本、合流车动力学可行性以及最终轨迹的平滑性。

为此，本文将单车匝道合流问题表述为一个包含离散 gap 选择和连续协作调整、合流时间、合流位置、轨迹变量的混合优化问题。直接求解该问题会导致较高的计算复杂度，不利于实时应用。本文提出一种分解式解析规划框架：首先基于同速到达时间对候选 gap 进行可达性筛选，而不是用当前 gap 长度进行硬筛选，从而保留那些当前不足但可协作扩张的潜在 gap；随后，对每个可达 gap，基于相邻空间余量构造空间敏感权重，将前后边界车辆的协作调整量分配建模为一个带线性约束的凸二次规划，并给出闭式投影解；接着，根据协作调整后的 gap 中心偏移，构造五次平滑合流轨迹，并将速度、加速度约束转化为显式的合流时间下界，从而得到一维约束凸优化问题的闭式合流时间；最后，综合协作代价和自车合流代价对所有候选 gap 评分，选择最优 gap，并只对该 gap 生成完整轨迹。

这个叙事的核心不是“我推导了很多公式”，而是：

$$

\boxed{
\text{把一个复杂的混合合流优化问题，分解成若干有物理意义、可解析求解、适合实时实现的子问题}
}

$$

这个故事是可以写成论文的。

---

下面是我用于对照判断的相关文献。它们说明该领域已有工作已经覆盖了 gap selection、sequence planning、trajectory optimization、free terminal time、cooperative gap development、polynomial trajectory 等主题，因此你的论文必须强调“解析分解式实时框架”的差异，而不能泛泛宣称首次联合优化。

1. *Rios-Torres, J., & Malikopoulos, A. A. (2016). Automated and cooperative vehicle merging at highway on-ramps. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/7534837/)*

2. *Zhou, Y., Cholette, M. E., Bhaskar, A., & Chung, E. (2018). Optimal vehicle trajectory planning with control constraints and recursive implementation for automated on-ramp merging. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/8510865/)*

3. *Zhou, Y., Cholette, M. E., Bhaskar, A., & Chung, E. (2018). Automated on-ramp merging and gap development with speed constraints: A state-constrained optimal control approach. American Control Conference. [Link](https://ieeexplore.ieee.org/abstract/document/8430796/)*

4. *Jing, S., Hui, F., Zhao, X., Rios-Torres, J., & Khattak, A. J. (2019). Cooperative game approach to optimal merging sequence and on-ramp merging control of connected and automated vehicles. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/8765399/)*

5. *Gao, Z., Wu, Z., Hao, W., Long, K., & Byon, Y. J. (2021). Optimal trajectory planning of connected and automated vehicles at on-ramp merging area. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/9565856/)*

6. *Chen, N., van Arem, B., Alkim, T., & Wang, M. (2020). A hierarchical model-based optimization control approach for cooperative merging by connected automated vehicles. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/9145866/)*

7. *Fang, Y., Min, H., Wu, X., & Wang, W. (2022). On-ramp merging strategies of connected and automated vehicles considering communication delay. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/9678122/)*

8. *Zhu, J., Easa, S., & Gao, K. (2022). Merging control strategies of connected and autonomous vehicles at freeway on-ramps: A comprehensive review. Journal of Intelligent and Connected Vehicles. [Link](https://www.emerald.com/insight/content/doi/10.1108/jicv-02-2022-0005/full/html)*

9. *Liang, J., Guan, T., Liu, D., Liu, X., & Luan, Z. (2023). An optimal trajectory planning for automated on-ramp merging. IET Intelligent Transport Systems. [Link](https://ietresearch.onlinelibrary.wiley.com/doi/abs/10.1049/itr2.12308)*

10. *Chen, J., Zhou, Y., & Chung, E. (2023). An integrated approach to optimal merging sequence generation and trajectory planning of connected automated vehicles for freeway on-ramp merging sections. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10268666/)*

11. *Liu, H., Zhuang, W., Yin, G., & Li, Z. (2023). Safety-critical and flexible cooperative on-ramp merging control of connected and automated vehicles in mixed traffic. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10053376/)*

12. *Yang, L., Zhan, J., Shang, W. L., Fang, S., & others. (2023). Multi-lane coordinated control strategy of connected and automated vehicles for on-ramp merging area based on cooperative game. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10144477/)*

13. *Xie, Y., Lu, G., Zheng, F., & Cao, P. (2024). A hierarchical approach for integrating merging sequencing and trajectory optimization for connected and automated vehicles. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10414392/)*

14. *Le, V. A., Chalaki, B., Tzortzoglou, F. N., & others. (2024). Stochastic time-optimal trajectory planning for connected and automated vehicles in mixed-traffic merging scenarios. IEEE Transactions. [Link](https://ieeexplore.ieee.org/abstract/document/10621701/)*

15. *Hu, Y., Yu, C., Su, Z., Ma, W., & Chen, Z. (2024). Critical trajectory point planning for connected and autonomous vehicles on freeway on-ramps under mixed traffic environment. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10614844/)*

16. *Xia, S., Min, H., Wu, X., Fang, Y., & Wang, W. (2024). On-ramp merging strategy integrating dynamic lane-changing decision and quasi-uniform B-spline trajectory optimization. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/10640249/)*

17. *Gu, M., Su, Y., Wang, C., & Guo, Y. (2024). Trajectory planning for automated merging vehicles on freeway acceleration lane. IEEE Transactions on Vehicular Technology. [Link](https://ieeexplore.ieee.org/abstract/document/10604430/)*

18. *Zhang, C., Qi, J., He, Y., & others. (2024). Collision-free merging control via trajectory optimization for connected and autonomous vehicles. Transportation Research Record. [Link](https://journals.sagepub.com/doi/abs/10.1177/03611981231224739)*

19. *Chen, N., van Arem, B., & Wang, M. (2022). Hierarchical optimal maneuver planning and trajectory control at on-ramps with multiple mainstream lanes. IEEE Transactions on Intelligent Transportation Systems. [Link](https://ieeexplore.ieee.org/abstract/document/9764649/)*

20. *Zhao, Z., Wu, G., Wang, Z., & Barth, M. J. (2020). Optimal control-based eco-ramp merging system for connected and automated vehicles. IEEE Intelligent Vehicles Symposium. [Link](https://ieeexplore.ieee.org/abstract/document/9304709/)*

21. *Huang, T., & Sun, Z. (2019). Cooperative ramp merging for mixed traffic with connected automated vehicles and human-operated vehicles. IFAC-PapersOnLine. [Link](https://www.sciencedirect.com/science/article/pii/S2405896319322852)*

22. *Wu, J., Wang, Y., Zhang, Z., Wen, Y., Zhong, L., & Zheng, P. (2022). A cooperative merging control method for freeway ramps in connected and autonomous driving. Sustainability. [Link](https://www.mdpi.com/2071-1050/14/18/11120)*

23. *Li, W., Ding, H., Xu, N., Song, Z., & Zhang, J. (2024). A time and energy efficient merging control for platoon formation of connected and automated electric vehicles at on-ramps. Nonlinear Dynamics. [Link](https://search.proquest.com/openview/d735a42b150cfe53b8716451fc476236/1)*

24. *Li, S., Zhou, Y., Ye, X., Jiang, J., & others. (2023). Sequencing-enabled hierarchical cooperative on-ramp merging control for connected and automated vehicles. IEEE ITSC. [Link](https://ieeexplore.ieee.org/abstract/document/10421889/)*

25. *Gao, Z., Yao, L., Li, J., Yin, Y., & Xiang, J. (2026). Trajectory planning and tracking with multiobjective optimization for connected and automated vehicles at expressway on-ramps. Journal of Advanced Transportation. [Link](https://onlinelibrary.wiley.com/doi/abs/10.1155/atr/9412778)*

26. *Lei, C., Tan, X., Zhang, J., Xiao, N., Deng, K., & Li, J. (2026). Optimized control method for cooperative on-ramp merging of connected vehicles based on discretized spatial feature modeling. Transportation Research Part C. [Link](https://www.sciencedirect.com/science/article/pii/S0968090X25004632)*

最终建议：**可以写，但不要把创新点写大。** 最稳妥的定位是“实时解析型单帧协作 gap 选择与轨迹生成框架”，不是“全局最优协同合流理论”。如果论文能证明子问题最优性、说明分解误差、补足动态安全约束，并做充分 ablation 和 baseline 对比，我认为它有希望支撑一篇会议论文或应用型期刊论文；当前版本如果不重构叙事和实验，大概率会被认为是工程公式整理而不是研究论文。