# SCSS的语法
  * Sass，一种缩进语法

  最开始Sass是Haml的一部分，Haml是一种预处理器，由Ruby开发者设计和开发，Sass类似Ruby的语法，****没有花括号，没有分号，具有严格的缩进****
  * SCSS，一种CSS-like语法

  <font color=red>SCSS 是 Sass 3 引入新的语法，其语法完全兼容 CSS3，并且继承了 Sass 的强大功能</font>。也就是说，任何标准的 CSS3 样式表都是具有相同语义的有效的 SCSS 文件。

## 常用语法
普通变量的声明
```SCSS
$fs:12px;
p{
  font-size: $fs;
}
```
默认变量的声明
```SCSS
$fs:12px !default;
p{
  font-size:$fs;
}
```
声明带参数的宏，可以在参数指定默认的参数
```SCSS
@mixin li-unstyle($width, $display){
    list-style: none;
}
```
使用宏
```SCSS
$width:50px
$display:inline-block
ul{
  @include li-unstyle($width,$display)
}
```
## CSS样式
& : 父类选择符

## 逻辑控制结构
@if @else的用法
```SCSS
@mixin tt($b:false){
  @if $b{

    border-right:5px;
  }
  @else{
    @for $i from 1 through 3 {
      .item-#{$i} { width: 2em * $i; }
    }
  }

}

.b1{
  @include tt($b:true);
}
.b2{
  @include tt;
}
```
插值
```SCSS
$test:(margin,border);
@mixin t($t1, $t2){
  @each $t3 in $test{
    #{$t3}-#{$t1}:$t2;
  }
}

.btn{
  @include t(right,5px);

}


/*
.btn {
  margin-right: 5px;
  border-right: 5px;
}

*/
```
@for循环
```
@for $i from <start> through <end>
@for $i from <start> to <end>
```
```SCSS
@for $i from 1 through 3 {
  .item-#{$i} { width: 2em * $i; }
}

/*
.item-1 {
  width: 2em;
}

.item-2 {
  width: 4em;
}

.item-3 {
  width: 6em;
}

*/
```
@while
```SCSS
$num : 2;
$height: 10px;
@while $num > 0 {
  .page-#{height}{
    height:$height * $num;
  };
  $num : $num - 1;
}

/*
.page-height {
  height: 20px;
}

.page-height {
  height: 10px;
}

*/
```
@each
```SCSS
$test:top,right,bottom,left;

@mixin btn-extend{
  @each $i in  $test{
    border-#{$i}:5px;
  }
}

.btn{
  @include btn-extend;
}

/*
.btn {
  border-top: 5px;
  border-right: 5px;
  border-bottom: 5px;
  border-left: 5px;
}

*/
```
通过@at-root表示不想嵌套选择器，当使用&选择器时，就算你不想嵌套选择器，Sass也会自动嵌套。

#{&}的用法
SCSS
```SCSS
```
@at-root和#{&}结合
Sass有脚本模式#{}，他和&不同之处是，**&只用作选择器，它只能出现在一个复合的开始选择器，类似于一个类型选择器**，如a或者h1。但<font color=red><b>#{}他表示的是一个插值，它可以用在任何地方</b></font>。同样的，当@at-root和#{&}一起使用时，可以给我们的开发带来极大的方便与优势。

SCSS
```SCSS
.block {
  color:red;
  @at-root #{&}__element{
    color:green;
  }
  @at-root #{&}--modifier {
    color:blue;
  }
}
```
CSS
```CSS
.block {
  color: red;
}
.block__element {
  color: green;
}
.block--modifier {
  color: blue;
}
```
@media 可以更加窗口大小不同使用不同的字体

BEM是让css的类选择器可以拼接生成 使用@at-root和#{&}来实现
