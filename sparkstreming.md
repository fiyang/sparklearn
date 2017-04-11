###spark streaming

```java
StreamingContext jssc = new JavaStreamingContext(conf, Durations.seconds(1));
// 以端口7777作为输入来源创建DStream
JavaDStream<String> lines = jssc.socketTextStream("localhost", 7777);
// 从DStream中筛选出包含字符串"error"的行
JavaDStream<String> errorLines = lines.filter(new Function<String, Boolean>() {
public Boolean call(String line) {
return line.contains("error");
}});
// 打印出有"error"的行
errorLines.print();

```

可以设置时间的周期。也可以指定是窗口的大小

0.3　转化操作
DStream 的转化操作可以分为无状态（ stateless）和有状态（ stateful）两种。
• 在无状态转化操作中，每个批次的处理不依赖于之前批次的数据。 第 3 章和第 4 章中所
讲的常见的 RDD转化操作，例如 map()、filter()、reduceByKey()等，都是无状态转化操作。
• 相对地， 有状态转化操作需要使用之前批次的数据或者是中间结果来计算当前批次的数
据。有状态转化操作包括基于滑动窗口的转化操作和追踪状态变化的转化操作


可以有状态，也可以没有状态


函数名称 目　　的 Scala示例
用来操作DStream[T]
的用户自定义函数的
函数签名
map() 对 DStream 中的每个元素应用给
定函数，返回由各元素输出的元
素组成的 DStream。
ds.map(x => x + 1) f: (T) -> U
flatMap() 对 DStream 中的每个元素应用给
定函数，返回由各元素输出的迭
代器组成的 DStream。
ds.flatMap(x => x.split(" ")) f: T -> Iterable[U]
filter() 返回由给定 DStream 中通过筛选
的元素组成的 DStream。
ds.filter(x => x != 1) f: T -> Boolean
repartition() 改变 DStream 的分区数。 ds.repartition(10) N/A
reduceByKey() 将每个批次中键相同的记录归约。 ds.reduceByKey(
(x, y) => x + y)
f: T, T -> T
groupByKey() 将每个批次中的记录根据键分组。 ds.groupByKey() N/A
