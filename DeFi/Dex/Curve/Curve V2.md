# 目标
Curve V1通过恒定乘积和恒定和的联合曲线，解决了stablecoin的swap问题。Curve V2想要满足除稳定币、锚定币之外的crypto的swap功能。
# 主要思想
Curve V2延续了V1的主要算法，


# 推导过程
在Curve V1中，两种稳定币价格越接近，AMM算法越偏向恒定和算法，当价格在平衡点时：
$$\large \Sigma x_i = D $$
其中D为一个常数，$x_i$ 是池中代币总数量。为方便计算，用$b$来表示$x_i$。而由于Curve V2中的代币并不是稳定币，相互之间有价格差异，因此采用 $b'$（代表价格）来替代数量$b$。
首先引入价格 $p$ 来代表（锚定）池中第一个资产的价格，白皮书中称之为'price_scale'。结合$b$和$b'$的表述可得：
$$\large b'=bp $$
在一个n个币组成的池子里，各资产价格为：
$$P=(p_0,p_1,...,p_n)$$
数量$b$和价值$b'$可表示为：
$$\large b=T(b'p)=(\frac{b_0'}{p_0},\frac{b_1'}{p_1},...,\frac{b_n'}{p_n}) $$
$$\large b'=T(b,p)=(b_0p_0,b_1p_1,...,b_np_n) $$
Curve V2中的D的定义与V1类似，在价值平衡点（即池中各资产价值相等）时：
$$\large D=Nx_eq $$
在Curve V2中，参考V1的方式定义平衡方程：
$$\large KD^{N−1}∑x^i+∏x^i=KD^N+(\frac{D}{N})^N
$$
但这里对K的定义有所不同，
$$\large K_0=\frac{∏x_iN^N}{D^N}$$
$$\large K=AK_0\frac{γ^2}{(γ+1-K_0)^2} $$




 

 
​
 $$
# 特点