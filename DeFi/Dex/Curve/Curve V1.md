# 简介
Curve V1 是一个稳定币及锚定币的交易和流动性提供平台。其独特优势是【低滑点】，尤其是大额兑换时。
# 主要功能
## Stable SWAP
基于 Curve 的流动性池子，Curve 提供稳定币的 SWAP 功能，可通过此功能以很低滑点进行交易。

<img src=./image/Curve/2023-04-10-14-57-36@2x.png width=500 />

## AMM
Curve V1 主要是解决稳定币和锚定币、映射币的流动性问题，因此其设计AMM机制有两个原则：
1. 当流动性充足时，滑点尽可能为低，甚至为0.
2. 为避免流动性枯竭，在代币比例不均衡时引入价格调节，使较少的代币不被全部提取。
当流动性充足时，Curve采用“线性”不变的兑换曲线来作为其AMM的机制。即：
$$\large \Sigma x_i = const $$
当代币数量不均衡时，采用具有价格调节机制的恒定乘积作为调节机制。
Curve要找到两种AMM类型之间的等式，结合两种AMM构建**“联合曲线”**，使AMM既能在流动性较好时保持价格稳定和低滑点，又能在流动性不足时可以自动调节价格。
### 推导过程
以两个币种的流动性池为例：
恒定和等式：
$$\large x+y = D $$
恒定乘积等式：
$$\large x*y = k $$
当x=y时，
$$\large x=y=D/2 $$
将之带入恒定乘积等式：
$$\large x*y=(\frac D 2)^2 $$
之后引入联合曲线调节因子α，并将两种AMM结合：
$$\large α（x+y）+xy=αD+(\frac D 2)^2 $$
调节因子α的作用是调整两种AMM曲线的使用比例，α越大，恒定和机制占比越多，因此需要使两种比数量接近时，让α更大，Curve称其为“杠杆”。
由于Curve中提供了多币种流动性池，但杠杆不能与币种数量相关，因此将α乘以D，得到：
$$\large αD(x+y)+xy=αD^2+(\frac D 2)^2 $$
当x=y时，α应为最大值，因此定义：
$$ \large α=\frac{Axy}{(D/2)^2} $$
A为常数，将上述阿尔法带入公式得：
$$ \large D\frac{Axy}{(D/2)^2}(x+y)+xy=\frac{Axy}{(D/2)^2}D^2+\frac{D^2}{2^2} $$
简化后为：
$$ \large 2^2A（x+y）+D=2^2AD+\frac{D^3}{2^2}xy $$
此公式为两个币种流动性池的公式，Curve支持多币种流动性池，套用上述计算过程，得出多币种流动性池的联合曲线公式：
$$\large An^n\Sigma x_i+D=AD_{n^n}+\frac{D^{n+1}}{n^n\prod x_i}  $$
公式中，A作为一个常数，需要在创建流动性池时设定，恒定不变。

### AMM特点
1. 手续费低。
2. 低滑点。
3. 根据不同流动性调节做市机制。

## 流动性池种类
Curve有三种类型的流动性池：
Plain pools：两个或多个稳定币交易流动性池，纯粹的用于交易的流动性池；
Lending pools：在这种交易池中，对原生的代币进行了一个再包装（wrapped tokens），像是一个代币的代币，例如用户质押的是DAI，但实际流动性池里是cDAI，这样做的目的是为了可以将质押到流动性池中的代币再借贷出去赚取利润，而用于交易的流动性池中实际保存的是wrapped tokens；
Metapools：稳定币与另外一个流动性池中的流动性代币（LP）组成的流动性池，这种流动性池的目的是为了给用户一个重叠质押的机会，例如用户质押DAI到一个Plain pool获得LP，然后又将LP质押到Metapool继续赚取收益，就像是你买了债券，然后又用债券换了一个理财，这样你既能获得债券的收益，又能获得理财的收益。

# Token经济

# 治理

# 项目意义

*参考资料：https://classic.curve.fi/whitepaper*
