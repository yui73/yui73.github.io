---
title: Ant Design Pro 学习
date: 2022-02-10 14:14:00
tags: Front/Interface
---
# Ant Design Pro 学习
> 文档：[Ant Design Pro文档](https://pro.ant.design/zh-CN/docs/overview)

## 0 准备工作
### 推荐技术栈
- 包管理：tyarn 安装`npm install yarn tyarn -g`
  文档：[tyarn](https://www.npmjs.com/package/tyarn)
- Terminal：选择Windows Terminal
  
### 快速开始
- 初始化脚手架
```
#使用yarn
yarn create umi app
```
- 遇到的问题
  ![yarnError](./AntDesignProLearning/yarnError.png)

- 解决方案：[官方解释](https://docs.microsoft.com/zh-cn/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7)
  </br>因此修改一下powershell的保护机制即可解决。使用`set-executionpolicy remotesigned`的命令。效果如下：
  ![yarnErrorSolution](./AntDesignProLearning/yarnErrorSolution.png)

  再次安装，效果如下，成功安装：
  ![yarnSuccess](./AntDesignProLearning/yarnSuccess.png)

## 1 基础结构
### 目录结构
```
├── config                   # umi 配置，包含路由，构建等配置
├── mock                     # 本地模拟数据
├── public
│   └── favicon.png          # Favicon
├── src
│   ├── assets               # 本地静态资源
│   ├── components           # 业务通用组件
│   ├── e2e                  # 集成测试用例
│   ├── layouts              # 通用布局
│   ├── models               # 全局 dva model
│   ├── pages                # 业务页面入口和常用模板
│   ├── services             # 后台接口服务
│   ├── utils                # 工具库
│   ├── locales              # 国际化资源
│   ├── global.less          # 全局样式
│   └── global.ts            # 全局 JS
├── tests                    # 测试工具
├── README.md
└── package.json
```

### 文件结构

#### 页面开发使用图表
使用Ant Design的图表：`yarn add @ant-design/charts`

### 一些报错
#### 1 网页控制台
![ReactError](./AntDesignProLearning/React.png)
- 解决方案


> 待更新 > v <
