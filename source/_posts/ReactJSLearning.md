---
title: ReactJS 学习
date: 2022-02-22 9:00:00
tags: Front/Interface
---

# ReactJS 学习

> 学习视频：[ReactJS入门](https://www.bilibili.com/video/BV164411s7RA?p=18)

## 0 准备阶段

### i 前端开发的四个阶段

- 静态阶段
  后端使用MVC模式，前端静态网页
- Ajax阶段
- 前端MVC阶段
  后来还有人提出了MVVM模式，使用ViewModel代替Controller
- SPA阶段
  网页前端变为一个应用程序，单网页应用程序称为SPA `single-page-application`,跑在浏览器里的应用程序。
  当前的Vue,Angular,React都属于SPA开发框架。

### ii ReactJS什么

React是一个用户构建用户界面的JavaScript框架。

- Flux: Flux补充了React的组合视图组件，它更是一种模式，非框架
- Redux: Redux是JavaScript状态容器，使React组件状态共享变得简单。
- Ant Design of React
  集成了多种框架，包含了Flux、Redux；提供了丰富组件。

> 本次学习使用Ant Design of React

## 1 搭建环境

### 1.1 创建项目

- 使用UmiJS作为构建工具。
  - 在已有node.js和yarn的情况下，使用 `yarn global add umi`全局安装umi
  - 使用`umi -v`确认是否安装成功
  - 若出现`‘umi‘ 不是内部或外部命令，也不是可运行的程序 或批处理文件或者提示 umi: command not found`，使用`yarn global bin`命令，将路径添加到系统环境变量中，重启cmd，再次尝试。
- IntelliJ创建web项目的方法：先创建Java空项目，然后再add framework support，选择web application。
- 使用命令`tyarn init -y`进行初始化。生成一个`package.json`文件
- 使用命令`tyarn add umi --dev`添加Umi脚手架的依赖，增加了一个`node_modules`的一个文件夹，里面是各种各样的依赖。
- Umi约定的目录结构

```
.
├── package.json
├── .umirc.ts
├── .env
├── dist
├── mock
├── public
└── src
    ├── .umi
    ├── layouts/index.tsx
    ├── pages
        ├── index.less
        └── index.tsx
    └── app.ts
```

### 1.2 Hello World!

- 创建config目录，在该目录下创建config.js文件(Umi的全局配置文件)

```
  //导出一个对象，暂时设置为空对象
  export default {};
```

- 创建pages目录，在pages目录下创建HelloWorld.js文件，输入

```js
  //在js文件中写html代码叫做JSX,React自创的
  export default () =>{
    return <div> Hello World</div>
    }
```

- 使用`umi dev`命令运行工程
- 进入`http://localhost:8000/HelloWorld`

> umi会给pages底下的内容自动添加路由映射

### 1.3 添加React插件

- 使用命令`tyarn add umi-plugin-react --dev`，添加react插件。

> `--dev`将安装内容加入package.json的devDependencies

- 在config.js文件中引入插件（旧版本）

```js
  export default{
      plugins:[
          [`umi-plugin-react`,{
              //暂时不启用任何功能
          }]
      ]
  }
```

- 此处有问题，现在umi版本已经大于3，因此安装的React插件版本应该为`"@umijs/preset-react": "^1"`,config.js应该如下，才可以成功使用`umi build`命令。

```js
  export default {
    dva: {},
    antd: {}
  };
```

## 2 快速入门

### 2.1 JSX语法

JSX就是在js文件中插入html代码片段，会被`Babel`等转码工具转码，得到正常的js文件后在执行。
JSX语法注意点：

- 所有html的标签必须闭合
- 只能有一个根标签，不能有多个
- 在html中插入通过`{}`插入js脚本

### 2.2 组件

#### 2.2.1 定义组件

```js
import React from 'react';//导入React

class HelloWorld extends React.Component{//继承Component类
    render(){//重写render方法，render方法渲染页面
        return <div>HelloWorld!</div>
    }
}
export default HelloWorld;//导出HelloWorld类
```

#### 2.2.2 引用组件

```js
import React from 'react';
import HelloWorld from "./HelloWorld";

class Show extends React.Component{
    render() {
        return (
            <HelloWorld></HelloWorld>
        );
    }
}

export default Show;
```

#### 2.2.3 组件参数传递方式

**属性**

```js
  // Show类
  import React from 'react';
  import HelloWorld from "./HelloWorld";

  class Show extends React.Component{
      render() {
          return (
              //将该对象的props赋值为zhangsan
              <HelloWorld name="zhangsan"></HelloWorld>
          );
      }
  }
  export default Show;
  
  //HelloWorld类
  import React from 'react';

  class HelloWorld extends React.Component{
      render(){
          //返回{this.props.name}
          return <div>HelloWorld!Component! name = {this.props.name}</div>
      }
  }
  export default HelloWorld;
```

  效果如下：
  HelloWorld类没有属性参数
  ![HelloWorld](/imgInPosts/ReactJSLearning/HelloWorld.png)

  Show类有属性参数
  ![Show](/imgInPosts/ReactJSLearning/Show.png)

**标签包裹**

```js
  //Show类
  import React from 'react';
  import HelloWorld from "./HelloWorld";
  class Show extends React.Component{
    render() {
        return (
            <HelloWorld name="zhangsan">标签包裹的内容</HelloWorld>
        );
    }
  }

  export default Show;

  //HelloWorld类
  import React from 'react';

  class HelloWorld extends React.Component{
    render(){
        return <div>HelloWorld!Component! Content={this.props.children}</div>
    }
  }
  export default HelloWorld;
```

  效果如下：
  HelloWorld类没有获取到包裹参数
  
  ![HelloWorld2](/imgInPosts/ReactJSLearning/HelloWorld2.png)

  Show类有包裹参数
  
  ![Show2](/imgInPosts/ReactJSLearning/Show2.png)

#### 2.2.4 组件状态

每个组件都有个状态，其保存在this.state中，当状态改变时，React会自动调用Render()方法，重新渲染页面。
注意点：

- this.state的值要在构造函数中设置
- 要修改state值要在this.setState()方法中设置

```js
import React from 'react';

class List extends React.Component{
    constructor(props) {
        super(props);
        this.state = {
            datalist : [1,2,3],
            MaxNum:3
        }
    }
    render(){
        return (
            <div>
                <ul>
                    {
                        this.state.datalist.map((value,index) => {
                            return <li>{value}</li>
                        })
                    }
                </ul>
                <button onClick={()=>{
                    let MaxNum=this.state.MaxNum+1;
                    let newArray=[...this.state.datalist,MaxNum];
                    this.setState(
                        {
                            datalist : newArray,
                            MaxNum:MaxNum,
                        }
                    )
                }}>点我</button>
            </div>
        );
    }
}

export default List;
```

一些解释

- 构造函数必须要有`props`参数
- map函数：返回一个数组 / 不检测空数组 / 不检测原始数组
- 箭头函数`()=>{...}`：无构造函数/this/arguments/super/new
- super函数：调用父类/父对象的...

```js
  super([arguments]):调用父类/父对象的构造函数
  super.functionOnParent([arguments]):调用父类/父对象的方法
```

### 2.3 ReactJS-Model层

> 与数据相关

#### 2.3.1 一些概念

**前端**

- Page层：与用户打交道，渲染页面
- Model层：完成业务逻辑，给Page做数据
- Service层：与HTTP接口对接，进行纯粹的数据读写，不会对数据进行处理

**后端**

- Controller层
- Service层
- Data Access层

#### 2.3.2 使用DVA框架对数据进行分层管理

**1 一些关于DVA的概念** 
DVA是基于Redux,Redux-saga和React-router的前端框架。
官网地址：[DVA官网](https://dvajs.com/)
> 待更新。。。

# React Hooks学习笔记

## 1 React的基础知识

### 1.1 组件

React组件有以下两类：

- 内置组件：映射到HTML节点(小写字母)
- 自定义组件：自己创建的组件(大写驼峰)
React组件只有一个根组件。

### 1.2 使用state和props管理状态

- state：用于保存状态的机制；使用useState这样一个Hook来保存状态。（具体后面说）
- props：类似于HTML的属性，在父子组件传递状态用。

### 1.3 脚手架

由于使用React时还需要路由管理，状态管理，UI层等……配置太麻烦，因此使用脚手架来创建项目。
好用的工具：

- codesandbox.io：网页，在线代码编辑器，用于分享少量代码。
- create-react-app：命令行，需要Node.js环境，用于创建基础的React项目。

## 2 理解Hooks

Hooks把某个结果钩到某个可能会变化的数据源或者事件源上。当被沟到的数据或者事件发生变化时，产生目标结果的代码重新执行。

如此实现了**逻辑复用**和**关注分离**。

## 3 内置Hooks

常用的Hooks有：`useState` / `useEffect` / `useCallback` / `useMemo` / `useRef` / `useContext`。

#### Hooks的使用规则

- 只能在函数组件的顶级作用域使用
- 只能函数组件或者其他Hooks中使用
- **所有Hooks必须要被执行到，必须按顺序执行。**

#### Hooks的常用辅助工具

` eslint-plugin-react-hooks`：使用npm安装，加入两个规则：

  - `rules-of-hooks`：检查Hooks使用规则
  - `exhaustive-deps`：检查依赖项的声明

> 下面先介绍两个最常用的Hooks。

### 3.1 useState

让函数组件具有维持状态的能力。

1. `useState(initialState)`：initialState是创建State的初始值。

2. useState()的返回值是有着两个元素的数组：第一个用于读取state的值；第二个用于设置state的值。**state只能通过第二个元素来设置。**

3. state中不保存可以通过计算得到的值：

   -  从props传来的
   -  从URL读到的值
   -  从cookie/localStorage中读取到的值 

### 3.2 useEffect

执行副作用，其中代码执行不影响渲染出来的UI。

1. `useEffect(callback, dependencies)`：callback是执行的函数；dependencies是可选的依赖数组。

2. useEffect是每次组件render完后判断依赖并执行。
    - 没有依赖项，每次render后会重新执行
    - 空数组，只有首次执行时触发
    - **依赖项比较使用浅比较**

3. useEffect允许你返回函数，用于组件销毁时做一些清理的操作。组件unmount后执行。

### 3.3 useCallback

缓存回调函数，进行性能优化。API签名如下：

`useCallback(fn,deps)`：fn定义的回调函数；deps依赖的变量数组。

只有当deps变化时，才会重新声明fn。

### 3.4 useMemo

缓存计算的结果，避免重复计算，避免子组件的重复渲染。

`useMemo(fn,deps)`：fn定义的计算函数；deps依赖的变量数组。

### 3.5 useRef

在多次渲染之间共享数据，可以用于保存某个DOM节点的引用。

```js
const myRefContainer = useRef(initialValue)
```

### 3.6 useContext

定义全局状态，但是会造成调试困难，复用困难。

```js
const value = useContext(myContext)
const MyContext = React.createContext(initialValue)
```

## 4 Hooks的四个典型使用场景

由于Hooks的目标是：**逻辑复用** + **关注分离**

### 4.1 抽取业务逻辑

    自定义Hooks函数，函数名：`useXXX`，并在其中用到了其他Hooks函数。

### 4.2 封装通用逻辑

自定义一个`useAsync`Hooks函数。
    
例如：发起一个异步请求获取数据并显示在页面上。
    
分析需要三个state：loading / error / data，将三个state和异步请求封装在useAsync函数里，实现如下（代码来源于课程）：

```js
import { useState } from 'react';

const useAsync = (asyncFunction) => {
// 设置三个异步逻辑相关的 state
const [data, setData] = useState(null);
const [loading, setLoading] = useState(false);
const [error, setError] = useState(null);
// 定义一个 callback 用于执行异步逻辑
const execute = useCallback(() => {
    // 请求开始时，设置 loading 为 true，清除已有数据和 error 状态
    setLoading(true);
    setData(null);
    setError(null);
    return asyncFunction()
    .then((response) => {
        // 请求成功时，将数据写进 state，设置 loading 为 false
        setData(response);
        setLoading(false);
    })
    .catch((error) => {
        // 请求失败时，设置 loading 为 false，并设置错误状态
        setError(error);
        setLoading(false);
    });
}, [asyncFunction]);

return { execute, loading, data, error };
};
```
业务逻辑部分代码变得更为清晰。

```js
import React from "react";
import useAsync from './useAsync';

export default function UserList() {
// 通过 useAsync 这个函数，只需要提供异步逻辑的实现
const {
    execute: fetchUsers,
    data: users,
    loading,
    error,
} = useAsync(async () => {
    const res = await fetch("https://reqres.in/api/users/");
    const json = await res.json();
    return json.data;
});

return (
    // 根据状态渲染 UI...
);
}
```

### 4.3 监听浏览器状态
    
自定义一个`useScroll`Hooks函数。（同上）

### 4.4 拆分复杂组件

将相关的逻辑做成独立的Hooks，在函数组件中使用Hooks。

## 5 Redux

> 这块内容还没来得及实践，日后有机会希望亲自体验。

### 5.1 Redux 的一些概念

- Redux Store 特点：全局唯一；树状结构。
- 好处：可预测性；易于调试。
- 三个基本概念：State、Action和Reducer
  - State：即Store，一个纯JavaScript Object。
  - Action：一个Object，用于描述发生的动作。
  - Reducer：一个函数，用于接收State和Action，计算新的Store。

### 5.2 在 React 中使用 Redux

将React和Redux建立联系主要是以下两点：

1. 当store变化时，React会重新Render
2. React中，在某些时机能dispatch一个action去触发Store的更新

因此使用`react-redux`工具库去实现二者的联通。

因为Redux Store具有全局唯一性，因此使用React的Context去存放Store信息，同时Context作为整个React应用程序的根节点。


> 待更新。。。
## 6 复杂状态管理
## 7 函数组件的设计模式
## 8 React事件机制
## 9 按领域组织文件夹结构
## 10 React中使用表单和对话框
### 10.1 表单
### 10.2 对话框
## 11 路由机制
## 12 按需加载
## 13 单元测试
## 14 第三方库