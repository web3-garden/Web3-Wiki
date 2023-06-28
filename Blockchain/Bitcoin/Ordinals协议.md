# 简介
Ordinals协议是通过比特币挖矿sat的顺序作为序号，并将其铭刻内容从而构建NFT的一种方法。Ordinals协议让比特币网络得以记录NFT，创建了一条新的赛道。
# 具体原理
Ordinals的原理主要来基于比特币挖矿和交易机制，主要基础理论如下：
1. 比特币最小单位是sat（satoshi），1BTC=10^8 sat。
2. 比特币交易采用UTXO模型，详情见：Blockchain/Bitcoin/UTXO.md
3. NFT的每一枚token都是可区分的。

# 标记sat
想在BTC网络上铭刻(在EVM链上通常说成铸造)NFT，首先要解决的问题是如何在同质化crypto网络（BTC是同质化crypto，理论上每一枚BTC没有区别）中标记代币使之可区分。
Ordinals采用了非常巧妙的办法：
由于BTC用UTXO作为记账方法，因此网络上所有BTC都可溯源，而每一枚BTC的源头又都是来自于挖矿，那么理论上，无论经过多少次转账，都可以定位一枚BTC是什么时候挖出的、第几个挖出的。
Ordinals就是用这个方式，将每一枚Sat（BTC最小单位）标记序号，作为其非同质化的标识。
具体代码解析如下：

``` # 为给定的区块分配序号
def assign_ordinals(block):
    # 根据区块的高度计算第一个序号
    first = first_ordinal(block.height)
    # 根据区块的高度和补贴计算最后一个序号
    last = first + subsidy(block.height)
    
    # 创建一个从第一个序号到最后一个序号的coinbase序号列表
    coinbase_ordinals = list(range(first, last))
    
    # 遍历区块中的每个交易（跳过coinbase交易）
    for transaction in block.transactions[1:]:
        # 为每个交易重置序号列表
        ordinals = []
        
        # 遍历交易中的每个输入并将序号添加到序号列表中
        for input in transaction.inputs:
            ordinals.extend(input.ordinals)
        
        # 遍历交易中的每个输出
        for output in transaction.outputs:
            # 根据输出的值为其分配序号
            output.ordinals = ordinals[:output.value]
            
            # 从序号列表中删除已分配的序号
            del ordinals[:output.value]
    
    # 将剩余的序号扩展到coinbase序号列表中
    coinbase_ordinals.extend(ordinals)
    
    # 为coinbase交易的输出分配序号
    for output in block.transaction[0].outputs:
        # 根据输出的值为其分配序号
        output.ordinals = coinbase_ordinals[:output.value]
        
        # 从coinbase序号列表中删除已分配的序号
        del coinbase_ordinals[:output.value]
 ```

通过以上方法，让每一枚sat都有了自己独一无二的序号，并且在交易时，采用FIFO（first-in-first-out）的原则进行追踪。
>FIFO:https://coinmarketcap.com/alexandria/glossary/first-in-first-out

# 铭刻内容
仅仅标记token/crypto的区别只是有了NFT的理论基础，但NFT需要有内容来支持，如文字、图片、声音等，但比特币区块容量比较小且珍贵，不适合作为NFT内容的存储空间，因此Ordinals采用segwit来实现内容承载，segwit空间足够容纳小型图片、声音等内容。

# 衍生玩法
## 稀有sat
编号为特殊号码的sat可被认为是稀有Sat，较普通sat有额外价值。