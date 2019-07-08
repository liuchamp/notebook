聚[合管](https://blog.csdn.net/congcong68/article/details/51619882)道

每个文档通一个或者多个阶段组成的管道，进行分组。

#### aggregate语法

db.collection.aggregate\(pipeline, options\)



 $group : 将集合中的文档分组，可用于统计结果，$group首先将数据根据key进行分组。



      $group语法： { $group: { \_id: &lt;expression&gt;, &lt;field1&gt;: { &lt;accumulator1&gt; : &lt;expression1&gt; }, ... } }

