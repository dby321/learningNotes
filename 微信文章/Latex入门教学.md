# Latex教学（入门篇）

```markmap
---
markmap:
  zoom: false
  pan: false
  height: 300px
  backgroundColor: "#f8f8f8"
---

# Latex入门教学
## 1. 参考资料
## 2. 安装教程
### 2.1 Latex安装
### 2.2 Latex编辑器安装
```



> 相信大家进行科研写作时候，难免会被`word`的公式格式插入所烦恼，`Latex`就是来解决这个问题的，它和`Markdown`都在各自的领域发光发热。下面就是我粗浅的Latex入门教学啦~

## 1. 参考资料

[ 科研必会——在Mac上配置LaTeX写作环境 - 知乎](https://zhuanlan.zhihu.com/p/560361957)

[LaTeX 安装与配置 | 菜鸟教程](https://www.runoob.com/latex/latex-install.html)

[Learn LaTeX in 30 minutes - Overleaf, Online LaTeX Editor](https://www.overleaf.com/learn/latex/Learn_LaTeX_in_30_minutes#What_is_LaTeX?)

## 2. 安装教程

### 2.1 Latex安装



> 最好使用科学上网方式，安装方式可以参考`1.参考资料`的安装方法，本文文档代码演示环境使用`MacTex.pkg`

- Mac安装

  - 方法一：[MacTeX Download](https://link.zhihu.com/?target=https%3A//www.tug.org/mactex/)下载pkg安装包

  - 方法二：brew安装
    - 安装brew： `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
    - 安装mactex：`brew install mactex --cask`

- windows安装：参考`1.参考资料`
- linux安装：参考`1.参考资料`

----

测试命令行是否可用：

![image-20250616233243077](/Users/dongbinyu/learningNotes/微信文章/images/Latex入门教学/image-20250616233243077.png)



### 2.2 Latex编辑器与插件安装

> 无脑推荐Vscode或支持Vscode生态编辑器（Trae、Cursor、Tongyi IDE）

1. 安装Vscode https://code.visualstudio.com/

2. 安装Vscode插件 LaTeX Workshop https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop

![image-20250616233312341](/Users/dongbinyu/learningNotes/微信文章/images/Latex入门教学/image-20250616233312341.png)

## 3. HelloWorld

1. 新建文件`01-helloworld.tex`编写文件

```tex
\documentclass[UTF8]{ctexart}
\begin{document}
你好，世界！
\end{document}
```



![image-20250616233408724](/Users/dongbinyu/learningNotes/微信文章/images/Latex入门教学/image-20250616233408724.png)

2. 使用xelatex渲染pdf文件

![image-20250616233441112](/Users/dongbinyu/learningNotes/微信文章/images/Latex入门教学/image-20250616233441112.png)

3. (可选)根据情况设置.gitignore文件内容

   ![image-20250616233625165](/Users/dongbinyu/learningNotes/微信文章/images/Latex入门教学/image-20250616233625165.png)