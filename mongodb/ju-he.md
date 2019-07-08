聚[合管](https://blog.csdn.net/congcong68/article/details/51619882)道

每个文档通一个或者多个阶段组成的管道，进行分组。

#### aggregate语法

```js
db.collection.aggregate(pipeline, options)
```

$group : 将集合中的文档分组，可用于统计结果，$group首先将数据根据key进行分组。

$group语法

```
 { $group: { _id: <expression>, <field1>: { <accumulator1> : <expression1> }, ... } }
```

```js
db.getCollection('cc_statistics').aggregate([
    { $match: { time: { $gte: 1562432400000, $lt: 1562605200000 } } },
    {
        $group: {
            _id: {
                $subtract: [
                    { $subtract: ["$time", 1476118800000] },
                    {
                        $mod: [
                            { $subtract: ["$time", 1476118800000] }, 1000 * 60 * 30
                        ]
                    }
                ]
            },
            value: { $sum: "$value" },
            timelist: { $push: "$time" }
        }
    },
    { $sot: { time: 1 } }
])
```



