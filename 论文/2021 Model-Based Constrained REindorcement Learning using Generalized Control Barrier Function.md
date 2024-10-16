GPI 与 IC 应用于 model-based / model-free RL 问题  
使用 Actor-Critic 架构来近似策略函数和价值函数

- 策略网络（Actor 网络） $\pi(x; \theta)$
- 价值网络（Critic 网络） $V(x; w)$

更新策略：
$$ x \overset{\theta}{\rightarrow} \pi(x; \theta) \rightarrow U (\text{策略 / 控制}) $$

更新价值：
$$ x \overset{w} \rightarrow V(x; w) \rightarrow V (\text{价值}) $$

1. **策略评估**（采用 TD 算法，更新价值网络参数，逼近真实的价值函数）  
   更新 Critic：
   $$
   \min_w L(w) = \mathbb{E}_{x_k \sim \Omega} \left\{ \frac{1}{2} \left( G - V(x_k; w) \right)^2 \right\}
   $$
   其中：
   $$
   G = \sum_{i=k}^{k+N} \gamma^{i-k} \left( L(x_i, \pi(x_i| \theta)) \right) + \gamma^N V(x_{k+N+1}; w)
   $$

   损失函数的梯度为：
   $$
   \frac{dL}{dw} = \mathbb{E}_{x_k \sim \Omega} \left\{ \left( G - V(x; w) \right) \frac{d V(x; w)}{d w} \right\}
   $$

2. **策略改进**（最小化目标函数）  
   更新 Actor：
   $$
   \min_{\theta} J(\theta) = \mathbb{E}_{x_k \sim \Omega} \left\{ G \right\}
   $$

#### Constrained GPI

$$
\theta_{k+1} = \arg\min_{\theta} J(\theta)
$$

**s.t.**
$$
x_{k+i+1} = f(x_{k+i}, \pi(x_{k+i}, \theta)), \quad i \in [0, N]
$$
$$
J_{C_z}(x_{k+i+1}) \leq b_{\tau}, \quad \tau \in [1, M]
$$
$$
\bar{D}_p(\theta; \theta_k) \leq \delta
$$

在 $\theta_k$ 处 Taylor 展开，构造线性目标函数和约束：

$$
\theta_{k+1} \approx \arg\min_{\theta} J(\theta_k) + \left( \frac{d J}{d \theta} \right)^{\top}_{\theta = \theta_k} (\theta - \theta_k)
$$

**s.t.**
$$
x_{k+i+1} = f(x_{k+i}, \pi(x_{k+i}, \theta)), \quad i \in [0, N]
$$
$$
J_{C_{\tau}}(x_{k+i+1}) |_{\theta = \theta_k} - b_{\tau} + \left( \frac{d J_{C_{\tau}}}{d \theta} \right)_{\theta = \theta_k} (\theta - \theta_k) \leq 0, \quad \tau \in [1, M]
$$
$$
\frac{1}{2} (\theta - \theta_k)^{\top} H(\theta - \theta_k) \leq \delta
$$

其中：
$$
g = g' / || g' ||_2
$$
$$
C_z = C_z' / || C_z' ||_2
$$

目标函数：
$$
\min_{\Delta \theta} \quad g^T \Delta \theta
$$

**s.t.**
$$
z_{\tau i} + C_{\tau} \Delta \theta \leq 0, \quad i \in [0, N], \quad \tau \in [1, M]
$$
$$
\frac{1}{2} \Delta \theta^T H \Delta \theta \leq \delta, \quad H \geq 0
$$

使用拉格朗日对偶方法求解：

$$
L(\Delta \theta, \eta, v) = g^T \Delta \theta + \lambda \left(\frac{1}{2} \Delta \theta^T H \Delta \theta - \delta\right) + \sum_{\tau =1}^{M} \sum_{i=0}^{N} v_{\tau i} C_{\tau}
$$

对 $\Delta \theta$ 求导，并令其为零：

$$
\nabla_{\Delta \theta} L(\Delta \theta, \eta, v_{zi}) = g + \lambda H \Delta \theta + \sum_{\tau=1}^{M} \sum_{i=0}^{N} v_{\tau i} C_\tau = 0
$$

得到：
$$
\Delta \theta = -\frac{1}{\lambda} H^{-1} \left(g + \sum_{\tau = 1}^{M} \sum_{i=0}^{N} v_{\tau i} C_\tau \right)
$$

求 $\lambda^*, v_{\tau i}^*$ 满足：
$$
(\eta^*, v_{\tau i}^*) = \arg\min_{\eta > 0, v_{\tau i} \geq 0} \left[ -L(\lambda, v_{\tau i}) \right]
$$

最终解为：
$$
\theta_{k+1} = \theta_k - H^{-1} \frac{\left(g + \sum_{\tau=1}^{M} \sum_{i=0}^{N} v_{\tau i}^* C_\tau\right)}{\lambda^*}
$$
在求解对偶问题之前，判断可行域是否为空：

令
$$
A = \left\{ \Delta \theta : z_{\tau i} + C_\tau \Delta \theta \leq 0 \right\}
$$
$$
B = \left\{ \Delta \theta : \frac{1}{2} \Delta \theta^T H \Delta \theta \leq \delta \right\}
$$

如果 $A \cap B = \varnothing$，则问题无解。

构造优化问题：
$$
\min_{\Delta \theta} \quad \frac{1}{2} \Delta \theta^T H \Delta \theta
$$

**s.t.**
$$
z_{\tau i} + C_\tau \Delta \theta \leq 0
$$

令 $\delta_{\text{min}} = \frac{1}{2} \Delta \theta_{\text{min}}^T H \Delta \theta_{\text{min}}$。

在问题设定中 TP 约束边界为 $\delta_a$：
$$
\delta_{\text{min}} \leq \delta_a, \quad \text{若 } \exists \, \Delta \theta_{\text{min}} \text{ 满足 } \frac{1}{2} \Delta \theta_{\text{min}}^T H \Delta \theta_{\text{min}} = \delta_{\text{min}} \leq \delta
$$

若 $A \cap B \neq \varnothing$，则存在可行解：

1. 若 $\delta_{\text{min}} \leq \delta_a$，令 $\delta = \delta_a$
2. 若 $\delta_{\text{min}} > \delta_a$，则 $A \cap B = \varnothing$
