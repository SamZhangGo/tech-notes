# Sass与SCSS的关系
  * Sass，一种缩进语法

  最开始Sass是Haml的一部分，Haml是一种预处理器，由Ruby开发者设计和开发，Sass类似Ruby的语法，****没有花括号，没有分号，具有严格的缩进****
  * SCSS，一种CSS-like语法


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
