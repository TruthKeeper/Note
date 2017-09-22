## APT

全称 Annotation Processing Tool，即注解处理器，在编译时通过注解生成代码，应用于ButterKnife，EventBus等流行框架

### ButterKnife原理

通过APT获取到修饰@BindView等的注解，获取到注解修饰信息后，对当前绑定对象，在当前包目录下后生成后缀**_ViewBinding**的Java文件，里面则是对**findViewById**，点击监听的代码的封装，而对注解修饰变量的赋值时机，是在**ButterKnife#bind**方法中，方法内部调用**_ViewBinding**类的构造方法，进行了属性注入

### EventBus原理

传统的方式是在**register**方法中扫描被**@Subscribe**修饰的方法，在合适的时机进行调用，用APT生成扫描后的文件索引，优化了耗时的反射扫描操作，[详细源码分析](http://www.jianshu.com/p/08fa335dddf9)

### 操作

1. 创建一个注解，[有关注解](https://github.com/TruthKeeper/Note/blob/master/Android/Annotation.md)
2. 创建一个Java Library（依赖大量javax包下的类）
3. .gradle如下
```
apply plugin: 'java'

dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    //生成META-INF的信息
    compile 'com.google.auto.service:auto-service:1.0-rc3'
    //优雅的生成Java代码（可选）
    compile 'com.squareup:javapoet:1.9.0'
    compile project(':annotation')
}
//  解决build警告：编码GBK的不可映射字符
tasks.withType(JavaCompile) {
    options.encoding = "UTF-8"
}
sourceCompatibility = "1.7"
targetCompatibility = "1.7"
```
4. 继承**AbstractProcessor**，建议通过方法来实现支持的源码版本和处理的注解集合（兼容性有待测试）
```
//生成 META-INF 信息
@AutoService(Processor.class)
//支持的源码版本
@SupportedSourceVersion(SourceVersion.RELEASE_7)
//处理的注解
@SupportedAnnotationTypes({"注解全名"})
public class AnnotationProcessor extends AbstractProcessor {
    /**
     * 基于元素进行操作的工具方法
     */
    private Elements elementUtils;
    /**
     * 代码创建器
     */
    private Filer fileCreator;
    /**
     * 日志消息输出器
     */
    private Messager messager;

    @Override
    public synchronized void init(ProcessingEnvironment processingEnvironment) {
        super.init(processingEnvironment);
        elementUtils = processingEnv.getElementUtils();
        fileCreator = processingEnv.getFiler();
        messager = processingEnv.getMessager();
    }
...
```
5. 实现**process**抽象方法（return true则代表处理了注解），方法中的参数**RoundEnvironment**可以通过**getElementsAnnotatedWith**方法获取到被注解修饰的元素

### [详细处理](http://www.jianshu.com/p/50d95fbf635c)