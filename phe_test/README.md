# PHE 性能测试

测试一下 10W个128维度的向量，未加密的浮点数，每个向量乘以一个1028位phe加密的浮点数后 得到 10w * 128个 加密的浮点数，然后求全部的和 需要多久。

另外再测试下这个计算使用spark进行分布式需要多久

`sum(X * [[c]])` 其中 X 是 `10W * 128`

另外时间只计算 『计算时间』 其他的spark启动啥的都去掉

## 单机测试结果

机器配置: 2.2GHz Intel Core i7 + 16GB 2400MHz DDR4

Python 单核

向量数 | 向量维度 | 加密位数 | 乘法耗时 | 总耗时
:--: | :--: | :--: | :--: | :--:
1000 | 128 | 1024 | 1min49s | 2min01s
2000 | 128 | 1024 | 3min44s | 4min10s
3000 | 128 | 1024 | 5min26s | 6min06s
4000 | 128 | 1024 | 7min13s | 8min09s
5000 | 128 | 1024 | 9min03s | 10min16s

## WTSS 测试结果

Executor 100 个，每个 1 核，只统计了纯计算的时间，不包括启动 spark 以及 numpy array 载入 RDD 的时间

向量数 | 向量维度 | 加密位数 | 总耗时
:--: | :--: | :--: | :--:
100000 | 128 | 1024 | 35s
200000 | 128 | 1024 | 1min11s
500000 | 128 | 1024 | 2min55s
1000000 | 128 | 1024 | 5min06s
1000000 | 128 | 1024 | 5min55s
1000000 | 128 | 1024 | 5min34s
5000000 | 128 | 1024 | 24min56s (Driver Memory 需要 32GB)