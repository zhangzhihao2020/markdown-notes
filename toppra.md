# toppra

### CanonicalLinearSecondOrderConstraint

表示典型线性广义二阶约束的类。

参数：

> inv_dyn: (array, array, array) -> array
>
> > 接收关节位置、速度和加速度作为输入并输出“关节扭矩”的“逆动力学”功能。 有关详细信息，请参阅注释。
>
> cnst_F: array -> array
>
> > 系数函数。 更多细节见注释。
>
> cnst_g: array -> array
>
> >系数函数。 更多细节见注释。
>
> dof: int, optional
>
> >关节位置向量的维数。 必需的。

注意：

二阶约束可由以下公式给出：

公式：

> A(q) \ddot q + \dot q^\top B(q) \dot q + C(q) = w

其中 w 是满足多面体约束的向量。

公式：

> F(q) w \leq g(q)

注意 `inv_dyn(q, qd, qdd) = w` 和 `cnsf_coeffs(q) = F(q), g(q)`。

要评估几何路径“p(s)”上的约束，需要多个 调用 `inv_dyn` 和 `const_coeff`。 具体一个可以推导出二阶方程如下。

公式：

> A(q) p'(s) \ddot s + [A(q) p''(s) + p'(s)^\top B(q) p'(s)] \dot s^2 + C(q) = w
>
> a(s) \ddot s + b(s) \dot s ^2 + c(s) = w

为了评估系数 a(s)、b(s)、c(s)，使用适当的参数重复调用 inv_dyn。



### compute_parameterization(self, sd_start, sd_end, return_data=False)

计算路径参数化。如果失败，无论是因为没有有效的参数化还是因为数值错误，返回的数组都应该包含 np.nan。

参数：

> sd_start: float
>
> > 起始路径速度。 必须是正的。
>
> sd_end: float
>
> >目标路径速度。 必须是正的。
>
> return_data: bool, optional
>
> > 如果为真，则还返回包含可控集的矩阵 K。

返回：

> sdd_vec: (N,) array
>
> > 路径加速度。 双数组。 如果失败，将包含 nan(s)。
>
> sd_vec: (N+1,) array
>
> > 路径速度。 双数组。 如果失败，将包含 nan(s)。
>
> v_vec: (N,) array or None
>
> > 辅助变量。
>
> K: (N+1, 2) array
>
> > 如果 `return_data` 为 True，则返回可控集