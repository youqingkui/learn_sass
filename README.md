# learn_sass

# Sass 基础

[toc]


### 监视文件编译成CSS
使用命令可以监视指定文件夹，然后将sass文件编译为css文件放入另外一个指定文件夹

    sass --watch sass:css # 监视sass文件夹，将文件编译到css文件夹
> 同时还可以编译sass输出css的格式
>  `nested` 嵌套， 默认的css输出样式，感觉输出的css比较适合sass的嵌套规则
>  `compact` 紧凑, 所有的css样式都单独一行
>  `expanded`  扩展， 符合常规的手写格式
>  `compressed` 压缩， 压缩格式，适合部署
>  使用方法： `sass --watch sass:css --style compact`


### sass 与 scss
以`.sass` 为扩展，是使用缩减的语法来写的sass，使用`.scss` 则是符合常规的css书写语法。一般喜欢使用python之类的，会喜欢上`.sass`的书写规则


### 使用变量
以一个`$` 定义变量，可以在变量里面使用变量

```sass
$primary-color: #1269b5
$primary-border:1px solid $primary-color
div.box
  background: $primary-color

h1.page-header
  border-right: $primary-border
```
```css
div.box {
  background: #1269b5; }

h1.page-header {
  border-right: 1px solid #1269b5; }

```


### 使用嵌套
对应一些重复可以使用嵌套来解决

```
.nav
  height: 100px
  ul
    margin: 0px
    li
      float: left
      list-style: none
      padding: 5px
```
```
.nav {
  height: 100px;
  }
.nav ul {
  margin: 0px;
  }
.nav ul li {
  float: left;
  list-style: none;
  padding: 5px;
  }

```

### 嵌套使用父选择
在某些情况嵌套会不符合我们的意思，如：
```
    article a
		  color: red
		  \:hover
		    color: red
```

```
article a {
	  color: red; }
	  article a :hover {
    color: red; }
```

得到的css 是`article a :hover` 这会导致，`article a`下面的所有子元素使用hover，而不是我们的本意，想让`article a `使用hover。
可以使用`&`来满足
```
article a
  color: red
  \&:hover
    color: red
```

```
article a
  color: red
  \&:hover
    color: red
```

同时可以使用`&`来代替父类选择
```
.nav
  height: 100px
  ul
    margin: 0px
    li

      float: left
      list-style: none
      padding: 5px

      a
        display: block
        color: #000
        padding: 5px
        \:hover
          color: red

  & &-text
    font-size: 15px
```


```
.nav {
  height: 100px; }
  .nav ul {
    margin: 0px; }
    .nav ul li {
      float: left;
      list-style: none;
      padding: 5px; }
      .nav ul li a {
        display: block;
        color: black;
        padding: 5px; }
        .nav ul li a :hover {
          color: red; }
  .nav .nav-text {
    font-size: 15px; }
```
> 在sass格式的文件中的嵌套下面使用hover需要使用转义符`\`


### 嵌套属性
对于一些属性，也可以使用嵌套的方法来减少重复

```
body
  font:
    family: "Helvetica Neue Light", "Lucida Grande", "Calibri", "Arial", sans-serif
    size: 15px
    weight: bold

.nav
  border: 1px solid #000
    left: 0px
    right: 0px
```

```
body {
  font-family: "Helvetica Neue Light", "Lucida Grande", "Calibri", "Arial", sans-serif;
  font-size: 15px;
  font-weight: bold; }

.nav {
  border: 1px solid black;
    border-left: 0px;
    border-right: 0px; }

```


### 使用minxin
minxin类似于函数，使用`@minxi name(arg1, arg2)` 可以定义一个minxin，然后再其他的地方使用`@include name` 来使用minxin
```
@mixin alert
  color: antiquewhite
  background: red
  a
    color: blue

.alert
  @include alert
```

```
.alert {
  color: antiquewhite;
  background: red; }
  .alert a {
    color: blue; }
```

使用参数，minxin也支持固定参数的用法，定义参数的时候同样要使用`$`

```
@mixin alert($text-color, $background)
  color: $text-color
  background: $background
  a
    color: darken($text-color, 10%)

.alert
  @include alert(#000, #111)

.alert-info
  @include alert($background:#111, $text-color:#000)
```

```
.alert {
  color: black;
  background: #111111; }
  .alert a {
    color: black; }

.alert-info {
  color: black;
  background: #111111; }
  .alert-info a {
    color: black; }
```
>  `darken` 使当前颜色加深，后面跟百分比