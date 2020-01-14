## 注解

- **编译前**提示信息：注解可以被编译器用来发现错误，忽视警告

- **编译时**生成代码（APT）：编译时根据注解生成代码，例如ButterKnife，EventBus

- **运行时**处理：在运行时通过注解反射获取到方法信息，然后执行操作，例如方法耗时计算

### 系统内置的注解

- @Override：表示重写
- @Deprecated：表示已过时
- @SuppressWarnnings：表示抑制警告

### 元注解 

- @Target：表示注解的修饰范围（类、构造器、方法、域等等）
- @Retention：表示注解的代码生存时期（编译时丢弃，类中保留，运行时保留）
- @Documented：表示可被提取成文档
- @Inherited：表示注解可以被子类继承（只作用在类上，对方法、属性域不起作用）

### In Android

- @NonNull、@Nullable 表示指定的空类型或非空类型
- @StringRes、@IntegerRes等 表示指定的资源变量类型
- @WorkerThread、@UIThread等 表示指定的线程类型
- @CallSuper、@CheckResult等 表示调用父类的方法和返回校验

### 自定义注解

> 当自定义注解只有一个属性时，且属性名为value时，赋值时value可省略

```java
public @interface Table {
    public String value();
}

@Table("student")
public class Student {
	
}
```

