# [2-4]从Nutz偷学MapList(中间体)

> @todo Mapl = MapList

- 为JSON服务的一个`中间结构`
- 满足一小撮人的一小撮要求 @minor
- @code 
  - https://github.com/nutzam/nutz/tree/master/src/org/nutz/mapl
  - https://github.com/service-java/nutz-v1.r.68/blob/master/src/org/nutz/mapl

- @doc https://nutzam.com/core/maplist/overview.html

# mapl使用 @demo

- JSON字符串 --> Mapl --> Mapl.maplistToObj()
- 跨层直接读取

```java  
String name = (String) Mapl.cell(obj, "as[1].name");
```

- mapl合并+过滤+包含

```
list不做递归合并, 只做简单的合并, 清除重复
```

- `mapl结构转换` 

```java
// 数据
String json = "[{'name':'jk', 'age':12},{'name':'nutz', 'age':5}]";
// 模型
String model = "[{'name':['user[].姓名', 'people[].name'], 'age':['user[].年龄', 'people[].age']}]";

String dest = "{\"people\":[{\"age\":12,\"name\":\"jk\"}, "
              + "{\"age\":5,\"name\":\"nutz\"}],"
              + "\"user\":[{\"姓名\":\"jk\",\"年龄\":12}, "
              + "{\"姓名\":\"nutz\",\"年龄\":5}]}";
Object obj = Mapl.convert(Json.fromJson(new StringReader(json)), new StringReader(model));
assertEquals(dest, Json.toJson(obj, new JsonFormat()));
```

- mapl的增删改 --> MaplRebuild