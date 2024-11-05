
### 公式 (1)
$$
\begin{bmatrix}
\dot{x} \\
\dot{y} \\
\dot{v}_{la} \\
\dot{\varphi} \\
\ddot{\varphi}
\end{bmatrix}
=
\begin{bmatrix}
v_r \cos \varphi - v_{la} \sin \varphi \\
v_r \sin \varphi + v_{la} \cos \varphi \\
\dot{\varphi} \left( \frac{a k_f - b k_r}{M_v v_r} - v_r \right) + v_{la} \frac{k_f + k_r}{v_r M_v} \\
\dot{\varphi} \\
\dot{\varphi} \frac{a^2 k_f + b^2 k_r}{I_z v_r} + v_{la} \frac{a k_f - b k_r}{v_r I_z}
\end{bmatrix}
+
\begin{bmatrix}
0 \\
0 \\
\frac{k_f}{M_v} \\
0 \\
\frac{a k_f}{I_z}
\end{bmatrix} \delta_f
$$

### 公式 (2)
$$
\begin{cases}
v_{lg} = v_r \\
v_{la} = \frac{b}{R} v_r \\
\frac{v_r}{R} = \varphi = \frac{v_r \tan \delta_f}{l}
\end{cases}
$$

### 公式 (3)
$$
\begin{cases}
X(t+1) = F(X(t)) + G(X(t)) \delta_f(t) \\
Y(t) = X(t)
\end{cases}
$$

### 公式 (A1) - 离散化状态转移矩阵 \( F(X(t)) \)
$$
F(X(t)) =
\begin{bmatrix}
x(t) + v_r(t) \cos \varphi(t) \Delta t - v_{la}(t) \sin \varphi(t) \Delta t \\
y(t) + v_r(t) \sin \varphi(t) \Delta t + v_{la}(t) \cos \varphi(t) \Delta t \\
v_{la}(t) + \frac{k_f + k_r}{M_v v_r(t)} v_{la}(t) \Delta t + \left( \frac{a k_f - b k_r}{M_v v_r(t)} - v_r(t) \right) \dot{\varphi}(t) \Delta t \\
\varphi(t) + \dot{\varphi}(t) \Delta t \\
\dot{\varphi}(t) + \frac{a^2 k_f + b^2 k_r}{I_z v_r(t)} \dot{\varphi}(t) \Delta t + \frac{a k_f - b k_r}{v_r(t) I_z} v_{la}(t) \Delta t
\end{bmatrix}
$$

### 公式 (A2) - 控制输入矩阵 \( G(X(t)) \)
$$
G(X(t)) =
\begin{bmatrix}
0 \\
0 \\
\frac{k_f}{M_v} \Delta t \\
0 \\
\frac{a k_f}{I_z} \Delta t
\end{bmatrix}
$$
