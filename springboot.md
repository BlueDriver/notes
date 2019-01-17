# SpringBoot

## 项目从jar变成war

> IDEA使用Sping Initializer新建SpringBoot项目，选择打包为jar后，如果后期想改成war部署在Tomcat中，该怎么做呢？

假设我们的启动类为`Applicaion.java` 

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Applicaion {
    public static void main(String[] args) {
        SpringApplication.run(Applicaion.class, args);
    }
}
```

### Step 1

修改Application类，使其继承自`SpringBootServletInitializer`， 并重写`configure`方法

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Applicaion extends SpringBootServletInitializer{
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(CourseWarApplication.class);
    }
    public static void main(String[] args) {
        SpringApplication.run(Applicaion.class, args);
    }
}

```

### Step 2

修改`pom.xml`文件

```xml
<packaging>jar</packaging>
```

为

```xml
<packaging>war</packaging>
```

并加入如下的依赖

```xml
<!-- 这个是比生成jar文件要多加的包 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <scope>provided</scope>
</dependency>
```

> 实际是改变springBoot内置Tomcat依赖的作用域，使其被部署时不会出现冲突

## jar变成war后要注意！

很有可能出现数据库连接不上的坑！

```xml
<property name="jdbcUrl">jdbc:mysql://localhost:3306/db_name</property>
```

上面这个看起来没问题，但实际操作时，应注意`localhost`可能给你制造麻烦，某些服务器本地地址不一定是`127.0.0.1`，而是其他诸如`192.169.1.100`,  **建议不用`localhost`，而是用实际本地地址**

