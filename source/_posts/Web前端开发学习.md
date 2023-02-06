---
title: Web前端开发学习
date: 2021-04-25 18:48:03
tags: Front/Interface
---

>仅用于自己学习时知识点的整理
>学习视频：[鱼C-小甲鱼](https://www.bilibili.com/video/BV1QW411N762?p=11)

## 引用
- q元素
用于定义比较短的元素
```html
<!DOCTYPE html>
<html>
	<head>
		<title>q元素</title>
	</head>
	<body>
		<p>
			<q>孔子有云：<q>学而不思则罔，思而不学则殆</q></q>
		</p>
	</body>
</html>
```
- blockquote元素
通过缩进的形式区分
```html
<!DOCTYPE html>
<html>
	<head>
		<title>blockquote元素</title>
	</head>
	<body>
		<p>未被引用段</p>
    <blockquote>
   		<p>被引用段</p>
    </blockquote>
	</body>
</html>
```
- cite元素
用于定义作品的标题，默认为斜体
```html
<!DOCTYPE html>
<html>
	<head>
		<title>引用</title>
	</head>
	<body>
		<cite>CITE元素</cite>
	</body>
</html>
```
- abbr元素
用于定义缩写，与全局属性title配合使用可展示完整含义
```html
<!DOCTYPE html>
<html>
	<head>
		<title>abbr元素</title>
	</head>
	<body>
		<p><abbr title="abbreviate">abbr</abbr></p>
	</body>
</html>
```
- dfn元素
用于定义术语（试了一下，默认也是斜体）
```html
<!DOCTYPE html>
<html>
	<head>
		<title>abbr元素</title>
	</head>
	
	<body>
		<p><abbr title="abbreviate">abbr</abbr></p>
	</body>
</html>
```
- address元素
用于定义联系信息的元素，默认斜体
```html
<!DOCTYPE html>
<html>
	<head>
		<title>address元素</title>
	</head>
	
	<body>
		<address><a href="https://www.baidu.com">百度网址</a></address>
	</body>
</html>
```
- ruby、rt、rp元素
rt：用于标记注音符号
rp：当浏览器不支持ruby替代的显示内容
```html
<!DOCTYPE html>
<html>
	<head>
		<title>ruby元素</title>
	</head>
	
	<body>
		<ruby>我<rp>(</rp><rt>wǒ</rt><rp>)</rp></ruby>
		<ruby>爱<rp>(</rp><rt>ài</rt><rp>)</rp></ruby>
		<ruby>上<rp>(</rp><rt>shàng</rt><rp>)</rp></ruby>
		<ruby>海<rp>(</rp><rt>hǎi</rt><rp>)</rp></ruby>
	</body>
</html>
```
- bdo元素
具有dir属性，有两个值：
`lfr`(left to right)：为默认的
`rtl`(right to left)
```html
<!DOCTYPE html>
<html>
	<head>
		<title>bdo元素</title>
	</head>
	
	<body>
		<bdo dir="rtl">
			<p>从右往左</p>
			<ruby>我<rp>(</rp><rt>wǒ</rt><rp>)</rp></ruby>
			<ruby>爱<rp>(</rp><rt>ài</rt><rp>)</rp></ruby>
			<ruby>上<rp>(</rp><rt>shàng</rt><rp>)</rp></ruby>
			<ruby>海<rp>(</rp><rt>hǎi</rt><rp>)</rp></ruby>
		</bdo>
	</body>
</html>
```
## 样式格式
- 粗体
strong元素：粗体+表示重要的含义
b元素：仅表示粗体

- 斜体
em元素：斜体+表示强调的含义
i元素：仅表示斜体
**也使用css样式表来代替实现b和i元素**

- del元素
~~删除内容~~
含义正确，只是做一个更新
- ins元素
改为下划线
- s元素
~~删除内容~~
含义不正确
- u元素
下划线，含义为拼写错误的单词或者汉语的专有名词。
- mark元素
标记文本的作用，默认黄色
```html
<!DOCTYPE html>
<html>
	<head>
		<title>mark元素</title>
	</head>
	
	<body>
		<p><mark>标记1</mark>、<mark>标记2</mark></p>
	</body>
</html>
```
- sup元素
表示上标
- sub元素
表示下标
- small元素
把文本变小

## 列表
- 无序列表(Unordered List)
`ul`来定义列表
`li`来包裹列表项
左边默认为 ：
**·** 这样的样式
**·** 这样的样式

```html
<!DOCTYPE html>
<html>
	<head>
		<title>Unordered List</title>
	</head>
	
	<body>
		<ul>
			<li>Element1</li>
			<li>Element2</li>
			<li>Element3</li>
		</ul>
	</body>
</html>
```
- 有序列表(Ordered List)
`ol`来定义列表
`li`来包裹列表项
左边默认为 ：
**1.** 这样的样式
**2.** 这样的样式
- 表格
`tr`来定义表格
*HTML5之后的表格边框样式都要使用CSS来实现：*
`border`属性
`border-collapse`属性-用于合并边框线
## 表单
- 一个简单的表单
```html
<!DOCTYPE html>
<html>
	<head>
	    <meta charset="utf-8">
		<title>一个简单的表单</title>
	</head>
	
	<body>
		<form action="welcome.php" method="post">
			<label>名字：<input type="text" name="name"></label><br><br>
			<label>邮箱：<input type="password" name="password"></label>br><br>
		</form>
	</body>
</html>
```
`text`：明文
`method`：post(将参数整合到url中)/post
`bottom`：submmit/bottom/reset；可以覆盖表单的一些属性（formmethod）
`autocomplete`：浏览器是否帮你自动填充
`value`：设置默认值
`autofocus`：会自动聚焦到某个input框
`disable`：禁用元素，FormData不提交这个数据了
`readonly`：不允许修改默认值，FormData依然提交这个数据

---------------------------------------------------------------------------------------------
`fieldset`：分组元素 -> `legend`：fieldset子元素，用于设计分组的title
`select`&`option`：用于实现下拉框
`optgroup`：对下拉框进行分组

