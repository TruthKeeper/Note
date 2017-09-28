## Java注释技巧

增加可读性

### HTML标签

- &lt;pre&gt;标签来时表达代码（不会格式化缩进）
- &lt;b&gt;标签来起到加粗作用
- &lt;i&gt;标签来起到倾斜作用（中文有兼容性）
- &lt;img src= /&gt;标签来插入网络图片

### @注释

- @param ?表示方法的参数，并且支持跳转定位
- @return ?表示方法的返回值
- @see ?#? 表示类中的方法或者域，并且支持跳转定位，例如`@see java.util.ArrayList#get(int)`
- @link 同上，需要用花括号修饰，可以在注释中任意出现，see只能独占一行
- @deprecated 等同@Deprecated注解的显示效果


