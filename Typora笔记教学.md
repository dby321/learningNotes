[TOC]



# Typora笔记教学

[Typora安装教程](https://blog.csdn.net/qq_61621323/article/details/141036982)

```markmap
---
markmap:
  zoom: false
  pan: false
  height: 300px
  backgroundColor: "#f8f8f8"
---

# Typora笔记教学
## 1. 从安装Typora开始
## 2. Markdown语法
## 3. 配置Typora
### 3.1 给Typora选个好皮肤😀
### 3.1 目录折叠😎
### 3.2 本地图片存储😁
### 3.3 markdown配置😊
## 4. colorful你的笔记
### 4.1 展示效果
### 4.2 操作原理
### 4.3 操作步骤
## 5. 插件
```

## 1. 从安装Typora开始



[git笔记链接](https://github.com/dby321/learningNotes.git)

[Typora 官方中文站 (typoraio.cn)](https://typoraio.cn/)

> 激活方法建议自己百度，可能有替换文件和激活码两种方法
>
> 0.11.18 版本以下均为免费版，[免费版下载地址](https://www.ghxi.com/typora.html)

![安装Typora](.\images\image-20230715192728436.png)

## 2. Markdown语法

> 我是一段引用

**我是加粗** 我是普通字体 ==我是加强== `我是引用` **<font color='red'>红色</font>** *斜体*

[百度超链接](www.baidu.com) 

1. 有序列表
2. 有序列表

- 无序列表
- 无序列表

- [] 多选框
- [] 多选框

```css
div{
    display:block;
    background-color:red;
}
```

## 3. 配置Typora

### 3.1 给Typora选个好皮肤😀

> 方式一：上[Github](https://www.github.com)搜索**Typora Theme**
>
> 方式二：上([Themes Gallery — Typora (typoraio.cn)](https://theme.typoraio.cn/)官网搜索
>
> 推荐主题：
>
> - vue⭐⭐⭐⭐⭐
> - DrakeTypora⭐⭐⭐⭐⭐[liangjingkanji/DrakeTyporaTheme: 十二种主题风格 - Material Google JetBrains Vue Juejin Purple Ayu Dark (github.com)](https://github.com/liangjingkanji/DrakeTyporaTheme)

### 3.1 目录折叠😎

![image-20230715194359335](.\images\image-20230715194359335.png)

### 3.2 本地图片存储😁

> 因为所有文件最后会同步到github，所以不使用图床
>
> 同步到github推荐使用[Github Destop](https://desktop.github.com/)来操作

![image-20230715194711724](images/image-20230715194711724.png)

### 3.3 markdown配置😊

![image-20230715194823706](images/image-20230715194823706.png)

## 4. colorful你的笔记

### 4.1 展示效果

**<font color='red'>红色</font>**

**<font color='orange'>橙色</font>**

**<font color='yellow'>黄色</font>**

**<font color='green'>绿色</font>**

**<font color='cornflowerblue'>蓝色</font>**

**<font color='cyan'>青色</font>**

### 4.2 操作原理

> HTML+CSS+JS，没学前端的小伙伴不用太纠结，不影响使用

### 4.3 操作步骤

> 1. 下载[AutoHotKey](https://www.autohotkey.com/)并安装
> 2. 新建一个`xxx.ahk`的文件，将下面的代码复制进去
> 3. 使用AutoHotkey软件运行这个脚本文件

```ahk
; 分号以及分号后的内容代表注释
#IfWinActive ahk_exe Typora.exe
{
    ; alt+0 黑色
    !0::addFontColor("black")
    ; alt+1 红色
    !1::addFontColor("red")
    ; alt+1 橙色
    !2::addFontColor("orange")
    ; alt+3 黄色
    !3::addFontColor("yellow")
    ; alt+4 绿色
    !4::addFontColor("green")
    ; alt+5 浅蓝色
    !5::addFontColor("cornflowerblue")
    ; alt+6 青色
    !6::addFontColor("cyan") 
    ; alt+7 紫色
    !7::addFontColor("purple")
}

; 快捷增加字体颜色
addFontColor(color){
    clipboard := "" ; 清空剪切板
    Send {ctrl down}c{ctrl up} ; 复制
    ; SendInput {Text} ; 解决中文输入法问题
    SendInput {TEXT}**<font color='%color%'>
    SendInput {ctrl down}v{ctrl up} ; 粘贴
    If(clipboard = ""){
        SendInput {TEXT}</font>** ; Typora 在这不会自动补充
    }else{
        SendInput {TEXT}</** ; Typora中自动补全标签
    }
}
```

## 5. 插件

https://www.cnblogs.com/xiaodaidaiPro/p/18253837

```kanban
use strict
# Today's task

## Todo
* 语法说明(一级标题表示看板标题\n二级标题表示看板\n- 或 * 表示任务)
- 描述框样式(支持在描述框添加 markdown 行内样式：\n**加粗**，*斜体*，`代码`，~~删除~~，[链接](https://github.com/obgnail/typora_plugin)，![图片](https://avatars.githubusercontent.com/u/48992887?s=96&v=4))
- 严格模式(在首行添加 use strict 进入严格模式，将不会忽略语法错误)
- 当描述为空时隐藏描述框

## In-Progress
- 数量、配色(看板和任务都可以无限添加\n\n可以前往配置修改你喜欢的颜色)
- 当任务数量太多，出现滚动条时(可以将鼠标置于看板下，使用【鼠标滚轮】滚动任务)
- 当看板数量太多，出现滚动条时(可以将鼠标置于看板下，使用【ctrl+鼠标滚轮】滚动看板)

## Completed
- NOTE(语法是插件开发者定义的，无法通用，只建议【每日任务】临时使用)
```

```chat
---
# 配置
# 进入严格模式（不忽略语法错误）
useStrict: false
# 展示用户名
showNickname: true
# 展示头像
showAvatar: true
# 不允许展示时间
notAllowShowTime: false
# 允许使用markdown语法
allowMarkdown: true
# 右侧发送者的用户名
senderNickname: me
# 时间的用户名
timeNickname: time
# 用户头像
avatars:
  me: https://avatars.githubusercontent.com/u/48992887?s=96&v=4
  nickname1: ./assets/1.jpg
---

time: 昨天 18:21

testUser1: 使用冒号分割用户名和消息

me: 右侧的用户名为 me，时间为 time。

me: 支持部分 markdown 语法。比如 **加粗**，*斜体*，`代码`，~~删除~~，[链接](https://github.com/obgnail/typora_plugin)，\n![图片](https://avatars.githubusercontent.com/u/48992887?s=96&v=4)

nickname1: 支持使用 yaml 格式的 front matter 修改配置（就和 markdown 一样）\n\n其中的 avatars 选项用于配置用户的头像，如果没有配置头像，默认为用户名的首字符\n\n你可以试试在本文件所属目录下添加 `./assets/1.jpg` 文件，便可以看到对应用户的头像被修改了

注意: 支持导出成 HTML、PDF 等格式。语法是插件开发者自定义的，不具备通用性
```

```timeline
# 使用一级标题表示 timeline 的标题

## 2022-10-1
### 使用二级标题表示时间，三到六级标题表示内容
支持简单的 markdown 语法
- 这是无序列表项 1
- 这是无序列表项 2
---
支持 **加粗**，*斜体*，`代码`，~~删除~~，[链接](https://github.com/obgnail/typora_plugin)，![图片](https://avatars.githubusercontent.com/u/48992887?s=96&v=4)
不支持代码块，因为开发者不希望代码块发生嵌套

## 2023-10-1
语法是开发者自定义的，谨慎使用。
```

> [!WARNING]
> 这是一条警告






