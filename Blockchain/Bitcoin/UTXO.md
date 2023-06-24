# UTXO
UTXO（Unspent Transaction Output）是Bitcoin的独特的记账机制。不同于银行的“账户余额”模型，UTXO使用了一套更为简洁的记账机制：
对于每笔交易（tx），都至少有一个Input和一个Output，每个Input都来自于上一笔Output，而每个Output都会有下一笔Input消费，这些Output就是Unspent transaction output，即UTXO。
<div align="center">
<img src= ./image/UTXO.png width=800 />
</div>

# UTXO结构
下面是UTXO结构的详细说明：

* 交易哈希（Transaction Hash）：这是一个32字节的哈希值，表示之前交易的哈希。它唯一标识了产生该UTXO的交易。

* 输出索引（Output Index）：这是一个整数，表示在该交易中的输出索引位置。通过输出索引可以确定具体是第几个输出。

* 输出金额（Output Amount）：这是一个表示比特币数量的数，表示UTXO中的比特币金额。

* 锁定脚本（Locking Script）：这是一个脚本，用于定义接收方必须满足的条件才能使用该UTXO。通常使用脚本语言（如脚本语言的堆栈操作）来编写锁定脚本，以实现交易的安全和灵活性。

# UTXO优势
为什么比特币不采用传统的账户余额模型而要相对更难理解的UTXO呢？UTXO的优势主要有以下几点：
1. 
