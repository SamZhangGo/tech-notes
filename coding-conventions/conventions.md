# 编码规范
### 大括号
```java
//bad practice
Class Logger
{
  ...
}

//recommend
Class Logger {
  ...
}
```
### 空格和空行
```java
int param=2 //别这么紧凑吧
int param = 1;

public int getParam() {
  return param;
}
```
> 使用IDE自带的格式化快捷键，就ok啦啦
## 命名规范
> 1.驼峰表示法
* 类首字母大写，类名为名词或名词短语
* 方法首字母小写，方法名为动词或动词短语

> 2.包名全部小写，不使用下划线。

#### 类名示例
> * 测试类，以Test结尾，如：UserTest
* 异常类，以Exception 结尾,如：NumberFormatException
* 抽象类，以Abstract开头，如：AbstractAccessTask
* 设计模式相关类，建议在类名中体现出具体模式，如：OrderFactory，LoginProxy，ResourceObserver
* 接口名不使用大写I 开头，实现类以Impl结尾

#### 避免命名之间的混淆
````java
Category findCategoryById(int cateId);
Category findCategoryById(int categoryId); //recommend
````
#### 用具体的名字代替抽象的名字
```
day => elapsedTimeInDays
size => pageSize
Config => SystemConfig
Data => StudentData
```
#### 常用命名可能用到的词汇
> * add/remove
* up/down
* send/receive
* lock/unlock
* first/last
* start/stop
* open/close
* next/previous
* show/hide
* source/target
* old/new
* insert/delete
* mix/max
* begin/end
* Factory
* Template
* Composite
* Proxy
* Visitor
* Command
* Builder
* Adapter
* Decorator
* Observer
* Strategy
####  代码注释
> 添加尽量少必要的代码注释，<font color=red>尽量做到code可以self-explanatory</font>

## 最佳实践

#### 使用Guard Clause
是不是不易看懂？
```java
  public Foo merge (Foo a, Foo b) {
    Foo result;
    if (a != null) {
      if (b != null) {
        // complicated merge code goes here.
      } else {
        result = a;
      }
    } else {
      result = b;
    }
    return result;
  }
```
来改造一下,下面是不是更易懂、简洁、美观
```java
  public Foo merge (Foo a, Foo b) {
    if (a == null) return b;
    if (b == null) return a;
    // complicated merge code goes here.
  }
  ```
#### 方法参数尽量少
```java
void saveAddress(String firstName, String lastName, String street, String city, String state, String country, String zipcode, String telephoneNumber) {
  //do save address
}
//convert to the follow
void saveAddress(Address address){
  //do save address
}
```
#### Don't let <font color=red>Magic Number</font> play around

```java
if(code == 200) {
  //do if
}
```
```java
pubic static int CODE_REQUEST_SUCCESS = 200;
if(code == CODE_REQUEST_SUCCESS) {
  //do if
}
```
#### 优先处理<font color=red>正向逻辑</font>
```java
if(!isOk) {

} else {

}
```
```java
if(isOk) {

} else {

}
```
#### 使用枚举类的场景
一个类对象的数量是固定的，如季节，春夏秋冬。
```java
public static final int SPRING = 1;
public static final int SUMMER = 2;
public static final int AUTUMN = 3;
public static final int WINTER = 4;

public enum Season {
  SPRING, SUMMER, AUTUMN, WINTER;
}
```
> 注意：Android开发中并不推荐使用枚举，过多的枚举会带来性能的损耗

#### 返回长度为0的数据或集合，而不是null
```java
List<Cheese> cheeseList;
public Cheese[] getCheese() {
  if(cheeseList.size() == 0) {
    return null;
  }
}
```
改为下面的形式
```java
List<Cheese> cheeseList;
public Cheese[] getCheese() {
  if(cheeseList.size() == 0) {
    return new Cheese[0];
  }
}
```
#### 不要忽略异常
```java
try {
  doSomething();
} catch (Exception e) {
  handleException();
}
```
#### 优先使用接口引用对象
```java
ArrayList<Student> studentList
    = new ArrayList<Student>();//XXXX don't do that
List<Student> studentList
    = new ArrayList<Student>(); //user interface
```
#### 代码审查工具
* PMD
* findBugs
* sonar
* Jtest
