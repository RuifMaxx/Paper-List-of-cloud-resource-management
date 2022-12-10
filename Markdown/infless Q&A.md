Q: 不同任务$i$，CPU资源配置$c_i$是否一致？

A: 不一定一致

Q: 同一个任务$c_i, g_i$是否不变？

A: 不一定不变，根据贪心算法挑选最优

Q: 对调度算法的解释

$$
\begin{gathered}
\text { minimize }: \sum_j^m\left(\beta C_j+G_j\right) y_j \\
t_{\text {wait }}^i+t_{\text {exec }}^i \leq t_{\text {slo }}^i, \quad \forall i \in[1, \ldots, n] \\
t_{\text {exec }}^i \leq t_{\text {wait }}^i, \quad \forall i \in[1, \ldots, n] \\
\sum_i^n c_i x_{i j} \leq C_j y_j, \quad \forall j \in[1, \ldots, m] \\
\sum_i^n g_i x_{i j} \leq G_j y_j, \quad \forall j \in[1, \ldots, m] \\
\alpha R_{\max }^k+(1-\alpha) R_{\min }^k \leq R_k \leq R_{\max }^k, \quad \forall k \in I \\
x_{i j} \in\{0,1\}, y_j \in\{0,1\} \\
b_i, c_i \in Z_{+}, g_i \in Z
\end{gathered}
$$

A: 贪心算法

* 评价指标综合考虑资源效率和资源碎片率

$$
e_{i j}=\frac{\text { RPS } / \text { resource }}{\text { fragmentation }}=\frac{r_{u p} /\left(\beta c_i+g_i\right)}{1-\left(\beta c_i+g_i\right) /\left(\beta C_j+G_j\right)} 
$$

* 系统的输入变量含有$R_k = R-R_{max}$和$B$, $B$ 是可能的$batchsize$
* 优化的决策变量包括副本数$n_k$, 批处理大小$b_i$, CPU资源$c_i$, GPU资源 $g_i$，$x_{ij}$（第$i$个任务放在第$j$个服务器上）。
* 算法的核心是遍历所有的资源配置组合$<c_i,g_i,b>$和放置机器策略$x_{ij}$，根据评价指标$e_{ij}$选择最好的配置方式，这里假设机器同构
* 算法思路：

	* 当$R_k > 0$，进行以下循环
	* 对某个确定的$b \in B$，遍历所有的$c_i, g_i$组合，找到其中所有能够满足RPS/时间约束的组合 $I_b$
	* 在找到的 $I_b$内，遍历寻找使$e_{ij}$最大的的放置策略$x_{ij}$和资源组合$c_i, g_i$
	* 尝试其他的$b$，找到最好的配置组合，循环结束



