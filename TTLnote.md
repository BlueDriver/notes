# Tiny模板语言 TTL(Tiny Template Language)

> 在写jetx文件时，需要遵循TTL语法规范，以下是部分摘要，详情参考[官方文档](http://www.tinygroup.org/docs/32d81fe7076947d585b5778a66a1a50d "跳转")

## 基本语法

### 注释

#### 单行注释
```html
## This is a single line comment.
## 这里是行注释内容
```
#### 多行注释
```html
#*
 Thus begins a multi-line comment. Online visitors won't
 see this text because the Tiny Templating Engine will
 ignore it.
*#
#--这里是块注释内容 --#
#* 这里是块注释内容 *#
```
> #* *#方式是为了与Velocity兼容，#-- --#是为了便于把Html中的注释改成Tiny模板注释。

---

### 表达式

#### 取值表达式

* `${expression}`：输出表达式的计算结果

* `$!{expression}`：输出表达式的计算结果，并转义其中的 HTML 标签

> 如果表达式执行结果为`null`或`void`，则**不会输出任何内容**

> 取值表达式作为指令的参数时，不可以采用`${expression}`形式，需要去掉`{}`，同时在字符串参数中，也不支持`	${expression}`

---

#### Map常量

> 在模板中为了数据存储、传递的方便，Tiny模板语言提供了Map常量的表达方式。

##### 语法

```html
{expression:expression,...}
```

##### 示例

```html
{}  ##表示空Map
{"aa":"aaValue","bb":"bbValue"}##纯字符串Map
{"aa":1,"bb":"bbValue"}		   ##数字及字符串混合Map
```

##### 调用示例
```html
#set(map={"aa":1,"key":"bbValue"})
${map.key}
${map["key"]}
${map.get("key")}
```

##### 注意事项

{aa:1}和{"aa":1}的含义是不同的，这一点必须要注意。

{aa:1}表示key值是aa变量的值，${"aa":1}表示key值是"aa"的字符串。

因此，如果写为{aa:1}的形式时，如果没有aa变量存在，则会报空指针错误。

> 小贴士

如果不能确认，前面的变量是否为空，可以加一个**安全调用**方式：
```html
${map?.key}
或
${map?.get("key")}
```
---

#### 数组常量

##### 语法

```
[expression,...]
```

在模板中定义数组常量也十分简单，看了下面的例子相信您一定可以掌握。

##### 示例

```
[]  ##表示空数组
[1..5]##等价于[1,2,3,4,5]
[5..1]##等价于[5,4,3,2,1]
[(1+4)..1]##等价于[5,4,3,2,1]
[1,2,3,4,5]##纯数字数组
[1,"aa",2,"cc",3]##数字及字符串混合数组
[1，aa,2,"cc",3]##数字，变量，字符串混合数组
```

而调用数组常量的方式有两种，一种是直接通过下标索引的方式，另一种是调用get(index)方式。

##### 调用示例

```
${list[1]}
${list.get(1)}
```

> 小贴士

如果不能确认，前面的变量是否为空，可以加一个安全调用方式：`${list?[index]}`或`${list?.get(1)}`

---

#### 其他表达式

| 类型         | 表达式                                                       | 说明                                                         |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 逻辑运算     | !<br>&&<br>\|\|                                              |                                                              |
| 自增/自减    | ++<br>--                                                     | 和Java语言自增自减规则一致。 自增（++）：将变量的值加1，分前缀式（如++i）和后缀式（如i++）。前缀式是先加1再使用；后缀式是先使用再加1。 自减（– –）：将变量的值减1，分前缀式（如--i）和后缀式（如i--）。前缀式是先减1再使用；后缀式是先使用再减1。 |
| 算术运算     | +<br> -<br> *<br> / <br>%                                    |                                                              |
| 空值常量     | null                                                         |                                                              |
| 移位运算     | >><br> >>><br> <<                                            |                                                              |
| 比较运算     | ==<br>!=  <br>><br>>= <br> <<br>  <=                         | ==的执行逻辑请查看13.3 关系和逻辑运算。                      |
| 方法调用     | functionName([...])                                          | 调用框架中的内嵌或扩展方法。                                 |
| 数组读取     | array[i]                                                     |                                                              |
| 数字常量     | 123<br>123L<br>99.99F<br>99.99d<br>99.99e9<br>9 -99.99E-10d0xFF000xFF00L0.0010.001D1.10D | 前缀0x不可以写成0X,后缀lfde可以用相应LFDE替换                |
| 成员方法调用 | object.methodName([...])                                     | 可以通过框架为某种类型增加新的方法或覆盖原有方法。           |
| 成员属性访问 | object.fieldName                                             |                                                              |
| 布尔值常量   | true<br>false                                                |                                                              |
| 字符串常量   | "abc--\"tiny\"--"'abc--"tiny"--'                             | 使用单引号和双引号标明的字符串常量是等效的。但使用单引号可以减少字符串中引号的转义。反之亦然，"abc--'tiny'--"和'abc--\'tiny\'--'是等效的。 |
| 位运算       | ~<br/>^<br/>&<br/>\|                                         |                                                              |
| 三元表达式   | exp?a:b<br/>exp?:b                                           | a?:b等价于a?a:b，当表达式比较复杂的时候，这种简写形式会比较漂亮，也更容易阅读 |

Tiny模板引擎对于布尔表达式进行多种强力支持，不仅仅只有布尔值才可以参与运算，它的运行规则如下：

- 如果是null，则返回false
- 如果是布尔值，则返回布尔值
- 如果是字符串且为空串“”，则返回false
- 如果是集合，且集合中没有元素，则返回false
- 如果是数组，且数组长度为0，则返回false
- 如果是Iterator类型，则如果没有后续元素，则返回false
- 如果是Enumerator类型，则如果没有后续元素，则返回false
- 如果是Map类型且其里面没有KV对，则返回false
- 否则返回true

在访问属性或成员变量的时候，普通的方式是用“.”运算符，也可以使用"?.”运算符，表示如果前置变量非空，都继续执行取属性值或调用成员函数，避免空指针异常的发生。

> 注意

`#if(0)zero#end`会显示zero，在Tiny模板语言中，***只要有值就会返回true***

---

### 索引表示法

用类似foo[0]的方式可以获取一个对象的指定索引的值。这种形式类似调用get(Object)方法，实际上是提供了一种简略，比如foo.get(0)。因此以下几种写法都是调用get方法：

```
foo[0]     ## foo takes in an Integer look up
foo[i]     ## Using another reference as the index  
foo["bar"]   ## Passing a string where foo may be a Map
```

Java数组适用相同的语法，因为Tiny模板引擎将数组包装成一个对象，它可以通过get(Integer)获得指定索引对象的元素。例如：

```
foo.bar[1].junk
foo.callMethod()[1]
foo["apple"][4]
```

---

### 指令集

#### #set

**基本语法**

```
#set(name1=expression,name2=expression,[...])用于向当前上下文赋值
#!set(name1=expression,name2=expression,[...])用于向当前模板的上下文赋值。
```

> 注意：Tiny模板语言的#set指令赋值的内容可以直接是变量名，但是不需采用${变量名}的形式赋值。如`#set( customer.Behavior = $primate )`就是一种错误的写法。

“左操作数被赋值“是引用操作的一个规则。=号右侧可能是以下类型之一：

- Variable reference变量引用 
- String literal字符串 
- Property reference 属性引用
- Method reference 命令引用
- Number literal 数字
- ArrayList 数组
- Map 映射

请看下面例子，可以帮助聪明的您理解上述类型设置的理解：

```
#set( monkey = bill ) ## variable reference
#set( blame = whitehouse.Leak ) ## property reference
#set( number = 123 ) ##number literal
#set( friend = "monica" ) ## string literal
#set( say = ["Not", friend, "fault"] ) ## ArrayList
#set( map = {"banana" : "good", "kg" : 1}) ## Map
```

注意：在ArrayList类型引用的例子中，其原素定义在数组 [..]中, 因此，您可以使 ${Say.get(0)}访问第一个元素。

类似的，引用Map 的例子中, 原素定义在 { } 中，其键和值间以“:”隔成一对，使用 ${map.get("bannana") }在上例中将返回 'good'， 如果写成${map.banana}也会有同样效果。

下面的例子是一般的计算表达式结果通过#set指令赋值：

```
#set( value = foo + 1 )
#set( value = bar - 1 )
#set( value = foo * bar )
#set( value = foo / bar )
```

如果您在模板中对一个变量进行多次赋值 ，可以对其值进行替换，比如下例中，最终name变量中赋的值为字符串“def”。

```
#set(name="abc",name="def")
```

在Tiny模板语言中已经内置上下文Context，如果调用宏，会产生一个上下文；如果进入循环语句或多层循环语句，采用同名变量值就会出现覆盖的现象。如果在宏里或循环里，想把值设到自己的生命周期结束之后还可以被继续使用，建议设置到模板的上下文中。

设置到当前上下文用#set，设置到模板的上下文上，则用#!set，如果当前位置就在模板中，使用#set和#!set没有任何区别。 

> 注意： `#set` 不需要使用`#end`来声明结尾。

---

#### 条件判断

例子：

```
#if( foo )
   Tiny!
#end
```

在 #if 和 #end 的内容是否会输出，由foo是否为true决定。这里，如果foo为true，输出将是：Tiny！如果foo 为null或false，将不会有任何输出。

\#elseif 或 #else 可以 #if 和组合使用。如果第一个表达式为true，将会不计算以后的流程，如下例假设foo 是15 而bar为6。

```
#if( foo < 10 )
    <strong>Go North</strong>
#elseif( foo == 10 )
    <strong>Go East</strong>
#elseif( bar == 6 )
    <strong>Go South</strong>
#else
    <strong>Go West</strong>
#end
```

输出将会是

```
 Go South.
```

其中#if指令及#end指令必须包含，#elseif及#else指令可以省略，#elseif可以多次出现，而#else最多只能出现一次。多个条件之间可以用&&、||等进行连接。**有时候#else或#end会和后面的字符内容连起来，从而导致模板引擎无法正确识别，这时就需要用#{else}或#{end}方式，避免干扰。**

---

#### 关系和逻辑运算

##### ==相等运算
Tiny模板语言使用==来做比较，如下例.：
```
#set (foo = "deoxyribonucleic acid")
#set (bar = "ribonucleic acid")
#if (foo == bar)
  In this case it's clear they aren't equivalent. So...
#else
  They are not equivalent and this will be the output.
#end
```

浮点数比较，不推荐采用==方式进行比较，因为这样会由于精度原因出现误差，而导致看似相同的结果在执行equals的时候返回false。

注意：== 计算与Java中的 == 计算有些不同，不能用来测试对象是否相等（指向同一块内存）。在相等运算时，Tiny模板引擎首先会判断操作对象是否为null，若两个操作数都为null，则判为相等，若两操作数中仅有一个为null则判为不相等；若两个操作数都是非null但类型相同，则调用其equals()方法。如果是不同的对象，会调用它们的toString()方法再调用两个String的equals()结果来比较。

##### AND运算
```
## logical AND
#if( foo && bar )
   <strong> This AND that</strong>
#end
```
仅当foo 和bar都为true时，#if()才会输出中间内容.

##### OR 运算
```
## logical OR
#if( foo || bar )
    <strong>This OR That</strong>
#end
```
foo或bar只要有一个为true就可以输出。
##### NOT运算
NOT运算则只有一个操作参数或表达式 :
```
##logical NOT
#if( !foo )
 <strong>NOT that</strong>
#end
```

---

#### 循环语句

##### for循环

基本语法

```
#for|foreach(var:expression)
...
#else
...
#end
```

>  #for表示对expression进行循环处理，当expression不可以循环时，执行#else指令部分的内容。虽然foreach也表示循环，但是为了通用建议使用#for

```
#for(number:[1,2,3,4,5])
    value:${number}
#end
#foreach(book:user.books)
    书名：${book.title}，标题：${book.author.name}
#else
  No books found.
#end
```

其中expression必须是一个合法的Tiny Template表达式，具体参考表达式小节。

\#end指令在使用的时候如果有歧义可以用#{end}代替。循环变量及循环状态变量只在循环体内可以使用，循环体外则不可用。

```
<ul>
#for ( product : allProducts )
    <li>${product.name}</li>
#end
</ul>
```

在上述例子中，allProducts或是一个`List、 Map或是 Array`类型的容器集合，#for每一次循环都会将容器集合中的一个对象赋给暂存变量product（称为循环变量），*allProducts*指定给变量 product 是一个引用到其中一个Java对象的引用。如果product确实是一个Java代码中的Product类，它可以通过product.name访问（或者Product.getName()）。

我们假设 allProducts 是一个HashMap，您会发现要取出其中的东西是多么的简单：

```
<ul>
#for ( key : allProducts.keySet())
    <li>Key: ${key} -> Value: ${allProducts.get(key)}</li>
#end
</ul>
```

逻辑表达式判定方法



Tiny模板引擎对于表达式进行了多种强力支持，不仅仅只有集合类型才可以参与运算，它的执行规则如下：

-  如果是null，则不执行循环体。
-  如果是Map，则循环变量存放其entry，可以用循环变量.key、循环变量.value的方式读取其中的值
-  如果是Collection或Array则循环变量放其中的元素
-  如果是Enumeration、Iterator，则循环变量存放其下一个变量。
-  如果是enum类，则循环变量存放其枚举值
-  否则，则把对象作为循环对象，但是只循环一次

######  循环状态变量

每个#for语句，会在循环体内产生两个变量，一个是`变量本身`，一个是`变量名+“For”`，比如：

```
#for(num:[1,2,3,4,5])
...
#end
```

例如上面的例子中，在循环体内可以有两个变量可以访问一个是“num”，一个是"numFor"。其中numFor是其状态变量，用于查看for循环中的一些内部状态，下面对numFor属性进行详细说明：

- numFor.index 可用于内部循环计数，从` 1 `开始计数。
- numFor.size 获取循环总数。如果对 Iterator 进行循环，或者对非 Collection 的 Iterable 进行循环，则返回 -1。
- numFor.first 是否为第一个元素。
-  numFor.last 是否为最后一个元素。
- numFor.odd 是否为第奇数个元素。
-  numFor.even 是否为第偶数个元素。

###### 循环中断：#break

循环中断语句只能用在循环体内，用于表示跳出当前循环体。

**基本语法**

```
#break(expression)
#break
```

其中expression必须是一个合法的TinyTemplate表达式，具体参考表达式小节。

```
#for(num:[1,2,3])
    #break(num==2)
#end
```

上面的例子表示当num的值为2的时候，跳出循环体。

它等价于：

```
#for(num:[1,2,3])
    #if(num==2)#break#end
#end
```

> #break只能跳出一层循环，不能跳出多层循环。

###### 循环继续：# continue

循环继续语句，只能用在循环体内，表示不再执行下面的内容，继续下一次循环。

**基本语法**

```
#continue(expression)
#continue
```

其中expression必须是一个合法的Tiny Template表达式，具体参考表达式小节。

```
#for(num:[1,2,3])
    #continue(num==2)
#end
```

表示当num的值为2的时候，执行下一次循环。

它等价于：

```
#for(num:[1,2,3])
    #if(num==2)#continue#end
#end
```

---

##### while循环
语法
```
#while(expression)
...
#end
```
>  此指令表示对判断expression进行循环处理，当expression运算结果为false时结束循环。其中expression必须是一个合法的TinyTemplate表达式，具体参考表达式小节。

> while循环指令是模板引擎2.0.10新增加的指令，更早的版本是不支持的。

**示例**

```
#set(i=0)
#while(i<10)
  #set(i=i+1)
  ${i}
#end
```

---

#### 其他特性

##### **数学计算**
Tiny模板语言自带一些数学函数，可以用#set指令在模板中使用。下面是四则运算的例子：

```
#set( foo = bar + 3 )
#set( foo = bar - 4 )
#set( foo = bar * 6 )
#set( foo = bar / 2 )
```

##### 范围操作符
范围操作符可以与#set和#foreach语句一起使用。用它来生成包含整数的数组非常方便：
```
[n..m]
```
其中n和m必须是整数。m可以大于n也可以小于n。如果m小于n，整数序列是递减的。

##### 连接字符串
很简单，看例子就是 ：
```
#set( size = "Big" )
#set( name = "Ben" )
The clock is ${size+name}.
```
上面模板的输出将是
```
  The clock is BigBen
```

最后一个例子如下：

```
#set( size = "Big" )
#set( name = "Ben" )
#set(clock = size+" Tall " + name )
The clock is ${clock}.
```

输出是：

```
 The clock is Big Tall Ben.
```