OVal

> OVal is a pragmatic and extensible general purpose validation framework for any kind of Java objects (not only JavaBeans)
>
> [OVal doc](http://oval.sourceforge.net/userguide.html)

```java

public class User{
    @NotNull
 	@NotEmpty
  	@Length(max=32)
    private String name;
    
    private int age;
    
    
}
```

