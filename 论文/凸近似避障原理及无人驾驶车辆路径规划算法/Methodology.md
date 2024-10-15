## 基于凸近似的车辆避障路径规划方法

### 1. 避障基本原理
通过将障碍物表示为一组凸多边形的组合，定义出车辆的可行域，使其避免与障碍物发生碰撞。

### 2. 可行域计算公式
车辆的可行域定义为：
$$
P = \{ p \in \mathbb{R}^2 : A_c p \leq b_c + g - A_c d_m, \quad m = 1, \dots, r \}
$$
其中：
- $A_c$ 为边界方向向量矩阵：
  $$
  A_c = \begin{bmatrix} (q_1 - p_0)^T \\ \vdots \\ (q_M - p_0)^T \end{bmatrix}
  $$
- $b_c$ 为边界偏移向量：
  $$
  b_c = \begin{bmatrix} (q_1 - p_0)^T q_1 \\ \vdots \\ (q_M - p_0)^T q_M \end{bmatrix}
  $$
- $g$ 为最小距离向量，计算如下：
  $$
  g^j = \min_{w \in w} A_c^j w
  $$

### 3. 可行域条件简化
若障碍物为凸多边形且可以表示为顶点集的凸组合，则可以简化为：
$$
g^j = \min_{h=1, \dots, s} A_c^j w_{jh}
$$
并可以通过缩放系数 $$ \mu_j \geq 0 $$ 调整边界大小：
$$
w_j = \text{conv}\{\mu_j w_{j1}, \dots, \mu_j w_{js_j}\}
$$

该方法通过定义车辆在每个方向上的最小距离约束，确保路径规划过程中车辆能够避开障碍物。

---
示例：基于自车与障碍车的可行域计算 [[Example for Methodology]]