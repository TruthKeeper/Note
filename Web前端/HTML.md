# HTML

## 标签

### 超链接

a标签的target属性

- _blank：新窗口
-  _self：当前页面

​	target：_blank新窗口 _self当前页面

### 列表

 - 无序列表`<ul><li></li><li></li></ul>`

 - 有序列表`<ol><li></li><li></li></ol>`

 - 自定义列表：
```html
<dl>
	<dt>标题1</dt>
    <dd>内容1-1</dd>
    <dd>内容1-2</dd>
    <dt>标题2</dt>
    <dd>内容2-1</dd>
</dl>
```

### 表格

```html
<table width="200" border="1"  >
      <caption>表格的示例</caption>
      <thead>
          <tr>
              <td>姓名</td>
              <td>性别</td>
          </tr>
      </thead>
      <tbody>
          <tr>
              <td>张三</td>
              <td>男</td>
          </tr>
          <tr>
              <td>李四</td>
              <td>女</td>
          </tr>
      </tbody>
 </table>
```

### 表单

#### input

> 单标签

- **text**：单行文本输入框，通过**value**属性设置默认显示的文本，**maxlength**设置最长文本
- **password**：密码输入框
- **checkbox**：复选框
- **radio**：单选框，如果是一个分组，用相同的name来约束
- **button**：普通按钮，通过value属性设置显示的文本
- **submit**：提交按钮
- **reset**：重置按钮
- **image**：图像按钮，通过src指定图片链接
- **file**：文件按钮

#### label

> 当用户点即此标签后，会将焦点给指定的表单标签
>
> - 直接包裹input标签
> - 多个input标签时，通过**for**属性指定标签的**id**
>
> #### textarea
>
> > 文本域标签，用于多行文本 

#### select

```html
<select>
    <option>北京</option>
    <option>上海</option>
    <option>广州</option>
    <option selected>深圳</option>
</select>
```

#### datalist

> 候选框
>
> ```html
> <input type="text" value="输入姓名" list="namelist">
> <datalist id="namelist">
>     <option>张三</option>
>     <option>李四</option>
>     <option>王五</option>
>     <option>王富贵</option>
> </datalist>
> ```
>
> #### fieldset
>
> > 配合legend标签使用，对表单元素进行**分组**
>
> ```html
> <fieldset>
>    <legend>用户登录</legend>
>    <span>账号：</span>
>    <input type="text" name="">
>    <br />
>    <span>密码：</span>
>    <input type="password" name="">
> </fieldset>
> ```
>
> 