---
title: WPF + MVVM 学习
date: 2021-07-01 10:30:00
tags: CSharp
---

# WPF学习
>学习视频：[刘铁猛](https://www.bilibili.com/video/BV1ht411e7Fe)

>学习教材：《深入浅出WPF》

### 0 预备知识
- 什么是WPF
  三层结构：`（数据层->业务逻辑层->表示层）`
- 什么是XAML
  是WPF技术中专门用于设计UI的语言。
  实现了UI与逻辑分离。
  是一种单纯的声明型语言，无法加入程序逻辑，与逻辑相关的代码统统集中在程序逻辑层
  - “高内聚-低耦合”
  - Template功能。
  - Atrribute-属性
-  将cs文件编译完成过后会生成.dll文件，别的程序员可以直接引用这个.dll文件
- Solution层
- Project层(窗口/控制台应用程序/类库)：编译的结果叫做
  `项目集Assembly` 
    - Properties装着资源文件的描述 
    - Resources装着类库：`{}`是一个命名空间；
    - 两对`.xaml`文件：`APP.xaml`对应应用程序本身，其中`StartupUri:MainWindow.xaml`表示MainWindow为程序启动时的主窗体。
- Canvas可以进行拖拽式界面设计

### Lesson 1
#### 1.1 解析.xaml和.cs文件
- `partial`关键字：分部类，类可以分开在两个地方写，编译时会进行合并（也就是将`.xaml`和`.cs`合并在一个类里）

#### 1.2 浅析用户界面的树形结构
- Dos用户界面 - 文字用户界面
- GUI - 图形用户界面 - 友好的用户界面

WPF制作的用户界面是树形结构。Visual C++、Delphi、VB都是平面结构的。

### Lesson 2
##### 在`.xaml`中为对象属性赋值
对象的数据存储方式有：字段+属性；属性在对外暴露的场景里更加安全，当遇到脏数据时，可以使用set方法对脏数据进行过滤。
###### 方法一：使用Attribute进行赋值
这种方法为三种之中最简单的，也很方便。
缺陷：只能使用简单的字符串赋值，没有办法赋非常复杂的值。
```
<Grid>
  <Path Data="M 0,0 L 200,100 L 100,200 Z" Stroke="Black" Fill="Red" />
</Grid>
```
使用TypeConverter类进行映射（在MVVM中有自带的）
> 当前程序集的名空间一般定义为：local
```
xmlns:local="clr-namespace:XXX"
```
###### 方法二：用属性标签赋值
> 对应对象属性。
> 缺陷：程序变得很繁琐。
```
<Button />          #空标签
<Button></Button>   #标签的内容 与对象的内容区分
```
###### 方法三：使用标签扩展的方式赋值
方法如下：
```
Text="{StaticResource ResourceKey=stringHello}"
Text="{Binding ElementName=sld, Path=Value}"

# 标签扩展的属性用`逗号`隔开
```

### Lesson 3
#### 3.1 事件处理器与代码后置
> .NET平台的处理机制为事件

事件五个关键点
- 事件拥有者
- 事件拥有者 拥有 哪些事件
- 事件响应者
- 事件响应者 用哪些方法 响应 事件
- 其间的事件订阅关系
```
<Button x:Name="Button1" Content="Click Me!" Width="200" Height="100" Click="Button_Click" />
# Click属性为事件处理器 方法的命名一般是Pascal法
```
#### 3.2 导入程序集和引用其中的名称空间
> 模块化程序设计 - User Control - 项目组件 
```
<Window xmlns:uc="clr-namespace:RPARobot.UserControls">

<uc:....>
#要先add Reference 才可以使用

```
#### 3.3 XAML的注释
选中需要注释的内容，有个注释按钮。
其注释方法与HTML是一样的。
```
<!-- XXX -->
#注释之间不可嵌套
#快捷键是：crtl+e+c;crtl+e+u;crtl+k+c;crtl+k+u;
```

### Lesson 4
#### 4.1 x名称空间的由来和作用
xmlns：xml语言的namespace
x名空间专门用于映射：
`http://schemas.microsoft.com/winfx/2006/xaml`
该类专门用于解析并分析`.xaml`代码的。

#### 4.2 x名称空间里都有什么
详情见书。

#### 4.3 x:Class
x:Class里的类就是最后要进行合并的类。
IntializeComponent();这个方法是由.xaml里生成的。

#### 4.4 x:ClassModifier
> ClassModifier：类修饰符 默认为Public

#### 4.5 x:Name
> .xaml见到一个标签会创建一个实例，不是变量

x:name相当于给了一个变量引用这个实例，后台可以直接引用这个变量。

当一个控件有Name属性时，使用起来x:name等于name。

#### 4.6 x:FieldModifier
> 控制类字段的访问级别

### Lesson 5
> MVVM入门与提高

#### 5.1 基本常识
##### 5.1.1 开发环境
- Visual Studio
- Microsoft Prism
- Microsoft Blend SDK

##### 5.1.2 Nuget Package Manager 安装
无需安装 Visual Studio 已经集成。

##### 5.1.3 必备知识的准备
- Data Binding

- Dependency Property

- Dependency Object

- 了解WPF中的命令
> 委托式命令

- Lambda表达式

##### 5.1.4 Code Snippet
> 代码模板

**使用/创建**

#### 5.2 MVVM设计模式详解
> MVVM = Model + View + ViewModel

*View 和 ViewModel 的解耦*
##### 5.2.1 基础概念
- Model： 现实世界中对象的抽象结果
- View = UI
- ViewModel = Model for View
- ViewModel与View的沟通
  - 传递数据 -- 数据属性
  - 传递操作 -- 命令属性
> 模块化技术 依赖注入技术 -- 更高阶内容

*View做操作 ViewModel响应操作*

*ViewModel是桥梁的作用？*

*MVVM适合去写企业级程序*

#### 5.3 案例讲解
>预备知识：Binding+Command
##### 5.3.1 小程序
>`crtl+.`可以自动添加类库 <br>界面一动；后台就动；如何更新？
- 重要类库：NotificationObject
- 重要类库：DelegateCommand
- View和ViewModel交互

# 一些小知识
> 学习文档：[C#官方文档](https://docs.microsoft.com/zh-cn/dotnet/csharp/)
### 0 基础知识
##### ??运算符
```
??  #双问号运算符 
```
提供了一种快捷方式，可以在处理可空类型和引用类型时表示null值的可能性，这个运算符放在两个操作数之间。
第一个操作数必须是一个可空类型或者引用类型，第二个操作数必须与第一个操作数类型相同，或者可以隐式转换为第一个操作数的类型。
 1、如果第一个操作数不是null，整个表达式就等于第一个操作数的值；
2、如果第一个操作数是null， 整个表达式就等于第二个操作数的值
例子如下：
```C#
int? a = null;
int b;
b = a ?? 10;
a = 3;
Console.WriteLine(b); // 10
```
##### =>运算符
[lambda表达式](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/lambda-expressions)


# 一些文档
### 1 WPF文档
{% pdf /doc/doc_in_posts/WPFLearning/Prism5forWPF.pdf %}
