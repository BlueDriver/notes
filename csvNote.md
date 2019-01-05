# csv测试数据相关
> 编写测试数据时，对CSV文件的使用需要遵循一定规则

## Example

ManReq.java

```java
public class ManReq(){
    private String name;
    private int age;
    private Date birth;
    //getter and setter
}
```

ManReq.csv表格视图

Man|desc|name|age|birth
:--:|:--:|:--:|:--:|:--:
1 | 正例 | Alice | 20 | 19980408
2 |正例 | Bob  | 22 | 19960608

ManReq.csv源文件

```html
ManReq,desc,name,age,birth
1,正例,Alice,20,19980408
2,正例,Bob,22,19960408
```

---
>###  注意事项

1. 行下标从**0**开始，第**0**行为表头

2. 表头从第三列开始，其名称必须与对应的Req的field一一对应
3. 在形如`reqList = CSVSupport.getListFromCSV(ManReq.class,
   ​    ​    "path/ManReq.csv@1-2")`

 的使用中，`@1-2`表示数据从第一行取到第二行，若无，则将第0行（表头）忽略，从第1行开始读取到最后一行

 ```html
其他用法：
ManReq.csv			读取第1至最后一行

ManReq.csv@1		只读取第1行

ManReq.csv@2-4		读取2至4行

ManReq.csv@2:5		读取第2和第5行

ManReq.csv@2:4-6	读取第2行和4至6行

ManReq.csv@2-5:8-10	读取第2至5和第8至10行
 ```
## 扩展
> 了解如何读取指定行之后，我们就可以往数据中加入更多辅助数据了，比如注释说明

ManReq.csv表格视图，附带注释说明

| Man  | desc | name  | age  |  birth   |
| :--: | :--: | :---: | :--: | :------: |
|  字段   | 说明 | 姓名 |  年龄  | 生日 |
|  Man   | 类型 | String |  int  | String |
|  1   | 正例 | Alice |  20  | 19980408 |
|  2   | 正例 |  Bob  |  22  | 19960608 |

如上，加了两行辅助行来说明字段意思和数据类型，这时读取数据就要跳过他们，从第**3**行开始

## 重要提示

> 自动生成CSV表头的IDEA插件在这：[ingTools](https://github.com/BlueDriver/ingTools)