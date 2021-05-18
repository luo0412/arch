# [2-4]从Hutool偷学bloomfilter(BitMap)

> @todo

- 一个很长的二进制向量和一系列随机映射函数
- 有一定的误识别率和删除困难
- @code 
  - https://github.com/looly/hutool/tree/v5-master/hutool-bloomFilter/src/main/java/cn/hutool/bloomfilter
  - https://github.com/service-java/hutool-v5.5.1/tree/v5-master/hutool-bloomFilter/src/main/java/cn/hutool/bloomfilter
- @doc https://www.hutool.cn/docs/#/bloomFilter/%E6%A6%82%E8%BF%B0

```java
// 初始化
BitMapBloomFilter filter = new BitMapBloomFilter(10);
filter.add("123");
filter.add("abc");
filter.add("ddd");

// 查找
filter.contains("abc")
```

# BitMap

- BitSet

# Filter

