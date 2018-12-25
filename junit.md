# Junit

## 参数化测试

```java
import org.junit.Test;
import org.junit.runner.RunWith;
import org.junit.runners.Parameterized;
import java.util.Arrays;
import java.util.Collection;

@RunWith(Parameterized.class)//指定运行容器
public class Test3 {
    private String name;
    private static int count;

    //带参构造方法
    public Test3(String name){
        this.name = name;
        count++;
        System.out.println("count: " + count);
    }

    /*
提供数据的方法上加上一个@Parameters注解，这个方法必须是静态static的，并且返回一个集合Collection
    */
    @Parameterized.Parameters
    public static Collection<Object> data(){
        Object[] nameArray = {"Alice", "Bob", "Cathe", "Disk", "Even"};
        System.out.println("collection: -------------");
        return Arrays.asList(nameArray);
    }

    //测试逻辑代码
    @Test
    public void test(){
        System.out.println("name: " + name);
    }
}
```

```java
import org.junit.runner.RunWith;
import org.junit.runners.Suite;

@RunWith(Suite.class)
@Suite.SuiteClasses({
        Test3.class
})
public class RunTest {
    //运行
}
```

输出结果：
```html
collection: -------------count: 1
name: Alice
count: 2
name: Bob
count: 3
name: Cathe
count: 4
name: Disk
count: 5
name: Even

Process finished with exit code 0
```

执行过程：

1. 初始化Collection数据
2. 利用Collection中的数据执行测试类的构造方法（遍历执行）
3. 执行test方法体
4. 回到第2步，直至遍历完集合数据

