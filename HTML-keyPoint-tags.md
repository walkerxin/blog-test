<h1 style="text-align:center">浅谈 HTML 标签</h1>

> HTML 是一种定义网页内容结构的标记语言，全称 Hyper Text Markup Language ;<br>
> 对于 HTML 标签，如果懂得标签所代表的英文单词，基本上就知道这个标签的作用。

<!--记录下 HTML 常见标签的使用-->

## 常见标签
### iframe
含义：内联框架，在页面中嵌套另一个页面<br>
Tip：上述用法已经基本不用，但在遗留项目中可能遇到；除此之外，iframe有其它的[高级用法]()。

- iframe 是一种可替换元素（replaced element）
> <b>可替换元素</b>，常自带默认的宽、高，是一类 外观渲染独立于CSS的 外部对象。<br>

典型的可替换元素有 &lt;img&gt;、 &lt;object&gt;、 &lt;video&gt; 和 表单元素，如 &lt;textarea&gt;、 &lt;input&gt; 。 某些元素只在一些特殊情况下表现为可替换元素，如 &lt;audio&gt; 和 &lt;canvas&gt; 。 通过 CSS content 属性来插入的对象 被称作 匿名可替换元素（anonymous replaced elements）。


属性 | 作用 | 属性值
---|---|---
name | 常与 a 标签配合使用，展示目标网页 | 与 a 标签的 target 属性值一致
src | iframe显示的网页地址 | 相对路径、绝对路径、网址   
frameborder | iframe默认自带边框 | 通常设为 0 去掉边框

#### 示例

```
<iframe name="xx" src="" frameborder="0"></iframe>
<iframe src="./css.img" frameborder="0"></iframe>
<br>
<a href="//qq.com" target="xx">腾讯新闻</a>
<a href="//baidu.com" target="xx">百度一下</a>
```

### a 标签 - 链接
含义：a 是 "anchor"（锚）的缩写，实现页面间跳转（HTTP GET请求）

常见属性：
- href
- target
- download
- rel=noopener （主要用来避免一个bug）

#### href —— 指定一个超链接
1. 完整路径（如网址 http://www.baidu.com）
2. 无协议，跳转时使用当前页面使用的协议（因此最好不要使用file协议访问页面）
3. 相对路径
    - ./xxx.html
    - #nd1 （不会发送请求）
    - ?name=lucy
4. 伪协议
    1. 形式：<a href="javascript:alert(1);"></a>
    2. 用户点击a 标签后执行 javascript 代码
    3. 用法：可能有些奇葩的需求，需要「点击之后页面没有任何动作的 a 标签」（试过 href="" 会刷新当前页面， href="#" 当不在顶部时页面会滚动到顶部，锚点为#）。
5. 锚点

#### target —— 指定打开这个超链接的窗口
target 属性值   | 含义
---             | ---
_blank          | 在 新 / 空白页面
_self           | 在 当前页面
_parent         | 在 上一层父页面
_top            | 在 顶层页面
与iframe的name属性值一致    | 在 此iframe中

##### download —— 强制下载(而不是展示链接内容) 
- 决定是否下载的两种方式
    1. HTTP 响应决定。当响应头中的Content-Type: application/octet-stream 时，浏览器就会以下载的形式接收这个响应内容
    2. a 标签加上 download属性，没什么软用，浏览器可能不支持

#### 示例
```
<a target="_blank">
<a target="_self">

<!-- 两个需要结合iframe，才能看出效果 -->
<a target="_parent">
<a target="_top">
```

### form 标签
含义：form 像 div 或 p 元素，是一个容器元素，嵌套着一个或多个表单控件构成一个表单。<form></form>标识一个表单的开始和结束；表单常用来提交数据，本质上也是跳转页面（常发起POST请求）。<br>

属性    | 作用  | 属性值
---     | ---   | ---
action &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   | 指定表单提交到的请求路径  | 处理该表单的 URL
method  | 指定请求的动词                | "get" 或 "post"（不区分大小写）
enctype | method="post" 时, 提交表单内容的MIME类型  | ==application/x-www-form-urlencoded== (默认值) 或 ==multipart/form-data== 或 ==text/plain== (HTML5)
target  | 指定提交表单之后，在哪里显示收到的回复    | _blank, _self, _parent, _top 或 与iframe的name属性值一致

- method="get"，提交时会把表单数据以查询参数 ?name=value&name2=value2 的形式拼接到 URL 中进行请求（这样数据就暴露在URL里，因此不常用）
- method="post"，则会把表单元素的 name 和 value 放入HTTP请求的第 4 部分进行提交
- 提交的数据中出现一些奇怪的 % 组合，是因为除了键盘上之外的字符（如中文），会使用其UTF-8编码，每两位用 % 分隔的形式进行编码转换
- 一个表单需要：
    - 表单元素需设置name属性，提交时才会加入到请求数据中
    - 提交按钮
        - 一般形式 <input type="submit" value="提交"> 或 <button type="submit">提交<button>
        - 如果没有上述提交按钮，表单中的<button>Save</button>元素会被升级为提交按钮（<input type="button"> 不行）。
- 常见的表单元素有 input (text、password、checkbox、radio、number、button)、label、 select、textarea、button。

```
<form id="form" action="/user" method="post" target="xx">
    <label>
        Name:
        <input type="text" id="userName" name="userName">
    </label>
    <br>

    Hobby:
    <label for="badmin1">羽毛球</label>
    <input type="checkbox" id="badmin1" name="ball" value="badmin">
    <label for="read1">阅读</label>
    <input type="checkbox" id="read1" name="read" value="read">
    <br>
    
    <label for="user_intro">Introduction:</label>
    <textarea name="introduction" id="user_intro" cols="30" rows="10">Default value</textarea>
    
    <button type="reset">Reset</button>
    <button>Save</button>
</form>
```

#### <label> 元素
1. 为表单元素定义标签，有助于无障碍阅读
2. 通过其 for 属性与表单元素的 id 属性进行关联（点击label元素，激活对应表单控件聚焦），①和②两种关联方式

```
<!-- ① for与id 关联 -->
<label for="name2">LastName:</label>
<input type="text" id="name2" name="lastName">

<!-- ② 表单控件嵌套在<label>中关联 -->
<label>
    FirstName:
    <input type="text" name="firstName">
</label>
```

#### <input> 元素
含义：最常用的表单控件，其最重要的属性：type 属性，可通过变化type属性值变形为不同的表单控件

type            | 对应的控件    | 注意
---             | ---       | ---
text            | 单行文本框| 默认值，设定值不支持时为备用值
password        | 密码      | 以点显示输入内容，但并不保证安全
email           | 邮件      | 会检查是否为有效的email格式（带@）
radio           | 单选      | 使用单选框的要把name设置为相同
checkbox        | 多选框    | checked 设置选中属性
file            | 文件选择  | accept指定文件类型，multiple可选择多文件
url             | URL       | 会检查是否为有效的 URL 
color           | 颜色控件
date            | 日期控件
reset           | 重置按钮
submit          | 提交按钮

参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input)

#### <textarea> 元素
1. 设置<input>的默认值，必须使用 value 属性
2. 而设置<textarea>的默认值，只需在<textarea>元素的开始和结束标记之间放置默认值
3. 自带rows、cols属性设置宽高，但由于单位不准确，故常使用样式设定宽高px，固定大小样式 resize: none;

```
<label for="user_intro">Introduction:</label>
<textarea name="introduction" id="user_intro" cols="30" rows="10">Default value</textarea>
```

#### <select> 元素

```
<label for="nation1">
<select id="nation1" name="nation">
    <option>==请选择==</option>
    <option value="china" selected>China</option>
    <option value="usa" selected>USA</option>
    <option value="england" selected>England</option>
</select>
```

### table 表格元素
- 之前听说不要用table了，改用div，但是发现实际使用中table用得还挺多
- 根据HTML规范文档，table 标签的子元素有 thead、tbody、tfoot、colgroup
- tr （table row）
- th（table header 表头）
- td （table data 数据）

```
<table>
    <!-- 设置列的 -->
    <colgroup>
        <col></col>
    </colgroup>
</table>
```