# Latex入门教学

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

```
(base) dongbinyu@dongbinyudeMacBook-Pro ~ % pdflatex
This is pdfTeX, Version 3.141592653-2.6-1.40.27 (TeX Live 2025) (preloaded format=pdflatex)
 restricted \write18 enabled.
```



### 2.2 Latex编辑器安装

> 无脑推荐Vscode或支持Vscode生态编辑器（Trae、Cursor、Tongyi IDE）

