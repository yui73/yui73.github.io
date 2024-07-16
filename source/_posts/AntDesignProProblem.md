---
title: Ant Design Pro项目问题记录
date: 2023-02-08 10:12:00
tags: Front/Interface
---

> 2023年2月8日，由于后来就直接上手项目了，因此用本博客记录一些难以解决的问题。

## 1 异步同步问题

过去太久忘记了，只记得是拆开写，以后再遇到再记下来。

## 2 表单对齐问题

ANTD PRO的表单有三种布局格式：horizontal，vertical，inline。

常用horizontal。当想让标签后的输入框对齐时，可以给form加个布局。

```js
const formLayout = {
    labelCol:{
      flex:'80px'
    },
    wrapperCol:{
      flex:'1',
    },
    // 下面这也是一种，但不好用。
    // labelCol:{
    //   xs:{span:24},
    //   sm:{span:5},
    //   md:{span:4},
    //   xxl:{span:2},
    // },
    // wrapperCol:{
    //   xs:{span:24},
    //   sm:{span:19},
    //   md:{span:20},
    //   xxl:{span:22},
    // },
  };
<ProForm {...formLayout}/>
```

效果如下：

![Form](/img/img_in_posts/AntDesignProLearning/1_Form.png)

## 3 登录问题

*起因：需要搭建一个新的前端，使用最新的ANTD Pro的框架。ANTD Pro V5最新版本将原先的Umi3升级到Umi4并且脚手架也不再支持JS，默认全为TS。因此，登录等涉及到脚手架的内容需要重写，将原先的JS代码转换为TS代码。*

### 3.1 如何实现登录

此处涉及到三个关键文件：

*下面的很多代码都是脚手架会自动生成的，只要注意几个配置的关键点，即可。*

- `./User/Login/index.tsx`
  
  登录页面的提交登录表单的方法：

  ```ts
  const handleSubmit = async (values: API.LoginParams) => {
    try {
      // 登录
      const msg = await login({ ...values },{});
      if (msg.success) {
        const defaultLoginSuccessMessage = intl.formatMessage({
          id: 'pages.login.success',
          defaultMessage: '登录成功！',
        });
        message.success(defaultLoginSuccessMessage);
        // 使用localStorage来存储用户token
        localStorage.setItem('token', msg.data.token);
        // 获取用户信息
        await fetchUserInfo();
        const urlParams = new URL(window.location.href).searchParams;
        // 跳转至主页
        history.push(urlParams.get('redirect') || '/');
        return;
      }
      console.log(msg);
      // 如果失败去设置用户错误信息
      setUserLoginState(msg);
    } catch (error) {
      const defaultLoginFailureMessage = intl.formatMessage({
        id: 'pages.login.failure',
        defaultMessage: '登录失败，请重试！',
      });
      console.log(error);
      message.error(defaultLoginFailureMessage);
    }
  };
  ```

  登录页面获取用户信息的方法：

  ```ts
  const fetchUserInfo = async () => {
    const userInfo = await initialState?.fetchUserInfo?.();
    console.log(userInfo);
    if (userInfo) {
      // 使用flushSync不然会出现登录两次才能登录上的问题
      flushSync(() => {
        setInitialState((s) => ({
          ...s,
          currentUser: userInfo,
        }));
      });
    }
  };
  ```

*下面两个是运行时配置文件。*

- `access.tsx`
  
  *悄悄吐槽一下umi的文档，确实有点不知所云了。*

  参考文档：[umi3/plugin-access](https://v3.umijs.org/zh-CN/plugins/plugin-access)

  文档原文：我们约定了 src/access.ts 为我们的权限定义文件，该文件需要默认导出一个方法，导出的方法会在项目初始化时被执行。该方法需要返回一个对象，对象的每一个值就对应定义了一条权限。

  ```ts
  export default function access(initialState: { currentUser?: API.CurrentUser } | undefined) {
  const { currentUser } = initialState ?? {};
  return {
      canAdmin: currentUser && currentUser.access === 'admin',
    };
  }
  ```

- `app.tsx`

  由于使用token作为令牌，保持会话的登录状态，因此需要使用拦截器，对请求头统一加上token。

  ```ts
  //拦截器
  const authHeaderInterceptor = (url:any, options:any) => {
    if (localStorage.getItem('token') && url !== loginApiPath) {
      const token = 'Bearer ' + localStorage.getItem('token');
      options.headers = {
        ...options.headers,
        Authorization: token,
      };
      options = { ...options, interceptors: true };
    }

    return {
      url: `${url}`,
      options: { ...options },
    };
  };

  /**
  * @name request 配置，可以配置错误处理
  * 它基于 axios 和 ahooks 的 useRequest 提供了一套统一的网络请求和错误处理方案。
  * @doc https://umijs.org/docs/max/request#配置
  */
  export const request = {
    // 新增自动添加AccessToken的请求前拦截器
    requestInterceptors: [authHeaderInterceptor],
    // ...errorConfig,
    // errorConfig是脚手架默认生成的，配置在另一个ts文件里，这里我将拦截器直接加在了app.tsx中
  };
  ```

### 3.2 登录时必须点击两次登录才能登录上

由于获取用户信息是异步操作，即使登录成功得到token后，在跳转欢迎页面时，会发生currentUser为空，因此认为当前状态还未登录，又重定向到login页面的问题。

解决方法就是不要改脚手架生成的源码T A T，在调用获取用户信息的函数前加上`flushSync`提高异步执行优先级。

## 4 脚手架安装问题

起因：安装脚手架时，根据官方文档分为以下两步。

```sh
# 使用 npm
npm i @ant-design/pro-cli -g
pro create myapp
```

然而`pro create myapp`一直报错，显示不认识`Pro XXX`指令。

分析问题和报错，认为是环境变量当中没有npm全局安装的node_modules，所以识别不到`Pro XXX`指令。

在环境变量Path中添加`C:\Users\user.name\AppData\Roaming\npm`(也就是你全局安装node_modules的位置)

遂解决。

使用`npm config ls`指令可以查询node_modules全局安装的位置。
