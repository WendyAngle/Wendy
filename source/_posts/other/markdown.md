---
title: markdown语法
categories: 其他
tag: [前端]
date: 2017-08-10 10:00:00
comments: true
---
## 1. 图片
语法   
`![Alt text](/path/to/img.jpg "Optional title")`   
示例  
`![Foo](http://i.weather.com.cn/images/cn/life/2017/04/11/11141533DF572FBBA092E37E6E843C656C318272.jpg)`


## 2. 换行
在文本中输入的换行会从最终生成的结果中删除，浏览器会根据可用空间自动换行。如果想强迫换行，可以在行尾插入至少两个空格。


## 3.强调
语法   
`**text** 或者 _text_ `   
示例
`**这是重点** ` 显示为 **这是重点**    
` _这是重点_ ` 显示为 _这是重点_    
` *这是重点* ` 显示为 *这是重点*


## 4. 标题
可以在标题内容前输入特定数量的井号('#')来实现对应级别的HTML样式的标题(HTML提供六级标题)   
语法      
`## text`    
注意，有空格，`#`一个就是H1，类推 ,需要独占一行  
`# 标题1 ` 显示为 
# 标题1

## 5.引用
语法   
` > text `  
需要单独一行， `>`可以多个出现 
示例   
` > 引用信息` 显示为 
> 引用信息


## 6.链接
语法   
`[链接文字](链接地址)`   
和图片非常相似，少一个 `!`符号


## 7.水平分区线
要生成水平分区线，可以在单独一行里输入3个或以上的短横线、星号或者下划线实现。短横线和星号之间可以输入任意空格。以下每一行都产生一条水平分区线。
```
* * *
***
*****
- - -
---------------------------------------
```

## 8.列表
无序列表,如下是等同的
```
-   Red
-   Green
-   Blue

+   Red
+   Green
+   Blue

-   Red
-   Green
-   Blue 
```
有序列表
```
1.  Bird
2.  McHale
3.  Parish
```


## 9.删除线
语法   
`~~text~~`   
示例   
`~~text~~` 显示为 ~~text~~


## 10.表格
```
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
```
显示为

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |



其他：
Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号：
```
\   反斜线
`   反引号
*   星号
_   底线
{}  花括号
[]  方括号
()  括弧
#   井字号
+   加号
-   减号
.   英文句点
!   惊叹号
```

如果是支持 table, |也是需要转移的，曾今发生过这样的尴尬


## 引用 
[Markdown 维基百科](https://zh.wikipedia.org/zh-cn/Markdown)    
[markdown-syntax-zhtw](https://github.com/othree/markdown-syntax-zhtw)   
[Markdown 语法说明 (简体中文版)](http://www.appinn.com/markdown/)   
[Markdown 指南](https://www.binarization.com/archive/2016/markdown-guide/)   
[Markdown 语法](https://www.binarization.com/archive/2016/markdown-syntax/)
