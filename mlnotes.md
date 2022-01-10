## Boosting

### AdaBoost的训练误差推导

考虑有$N$个样本，$M$个弱分类器，AdaBoost算法的最终训练误差界为：

$$\frac{1}{N}\sum_{n=1}^{N}I(F(x^{(n)})\neq y^{(n)}) \leq \frac{1}{N}\sum_{n=1}^{N}\exp({-y^{(n)}f(x^{(n)})}) = \prod_m Z_m$$

其中

$$Z_m = \sum_{n=1}^{N}w_m^{(n)}\exp({-y^{(n)}\alpha_mf_m(x^{(n)})})$$

$$f(x) = \sum_{m=1}^{M}\alpha_mf_m(x)$$

不等式前半部分显然，后半部分推导过程如下：

$$\frac{1}{N}\sum_{n=1}^{N}\exp({-y^{(n)}f(x^{(n)})}) = \frac{1}{N}\sum_{n=1}^{N}\exp(-\sum_{m=1}^{M}\alpha_my^{(n)}f_m(x^{(n)}))$$

$$= Z_1\sum_{n=1}^{N}w_2^{(n)}\exp(-\sum_{m=2}^{M}\alpha_my^{(n)}f_m(x^{(n)}))$$

$$= \cdots = \prod_mZ_m$$

我们注意到$Z_m$是第$m$训练时对应的加权指数损失函数，因此，AdaBoost算法可以看作每一轮选择适当的弱分类器$f_m$使得$Z_m$最小。每一轮训练完成之后，为每一个样本重新分配权重。

$$w_{m+1}^{(n)} = \frac{1}{Z_m}w_{m}^{(n)}\exp({-y^{(n)}\alpha_mf_m(x^{(n)})})$$

其中$\alpha_m$是弱分类器$f_m$的权重，由计算其在训练集上的加权错误率$\epsilon_m$得到。

$$\alpha_m = \frac{1}{2}\log(\frac{1-\epsilon_m}{\epsilon_m})$$

更进一步：

$$Z_m = \sum_{n=1}^{N}w_m^{(n)}\exp({-y^{(n)}\alpha_mf_m(x^{(n)})}) $$
$$= \epsilon_m\exp(\alpha_m)+(1-\epsilon_m)\exp(-\alpha_m)$$
$$= 2\sqrt{\epsilon_m(1-\epsilon_m)} $$

$\frac{1}{2}-\epsilon_m = \gamma_m$，

$$Z_m = \sqrt{1-4\gamma_m^2}$$

$$\prod_m Z_m = \prod_m \sqrt{1-4\gamma_m^2} \leq \exp(-2\sum_{m=1}^{M}\gamma_m^2)$$

假设存在$\epsilon_m \geq \epsilon$对任意$m$成立，我们有$\gamma_m \leq \gamma$，

$$Z_m \leq \exp(-2M\gamma^2)$$

### Gradient Boosting

梯度提升拟合上一个模型的残差，在第$m$轮最小化损失函数

$$(\alpha_m, \phi_m) = \argmin \sum_{n=1}^{N}L(y^{(n)}, f_{m-1}(x^{(n)})+\alpha_m\phi_m(x^{(n)}))$$

$$f_m(x) = f_{m-1}(x)+\phi_m(x)$$

典型的有GBDT。然而并不是所有的损失函数都容易优化，所以可以考虑用$\phi_m(x)$拟合当前模型在训练集上的“残差”，

$$r_m^{(n)} = \frac{\partial L(y^{(n)},f_{m-1}(x^{(n)}))}{\partial f_{m-1}(x^{(n)})}$$

$$f_m(x) = f_{m-1}(x)+\gamma_m\phi_m(x)$$

一般称$\gamma_m$为“缩减率”。

