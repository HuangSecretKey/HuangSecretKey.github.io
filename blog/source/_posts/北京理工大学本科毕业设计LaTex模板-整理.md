---
title: 北京理工大学本科毕业设计LaTex模板(整理)
date: 2020-12-21 21:27:25
tags: -LaTex
---

Latex毕设模板教程（整理）

<!-- more -->

### 教程目录

[LaTex的下载与安装](#1.LaTex的下载与安装)

[LaTex基本介绍](#2.LaTex基本介绍)

[模板的下载与使用](#下载和使用BIThesis)

[格式转化](#格式转化)

### LaTex的下载与安装

#### 准备工作-安装latex环境，包括latex发行版以及latex编辑器。

下载合适的latex的发行版，安装不同系统，从官网下载。

<p align="center">
<a>
  <img src="images/LaTex/1.png" alt="LaTex下载网址" >
</a >
</p >

北京理工大学校园网镜像资源网站离线安装

打开镜像资源网，选择需要的版本下载

https://mirrors.bit.edu.cn/CTAN/systems/texlive/Images/

<p align="center">
<a>
  <img src="images/LaTex/2.png" alt="北理工镜像资源网">
</a >
</p >

ubuntu为例，

```sudo apt-get install textlive```

#### 确认安装

打开终端

```xelatex --version```

<p align="center">
<a>
  <img src="images/LaTex/4.png" alt="xelatex版本">
</a >
</p >

```biber --version```

<p align="center">
<a>
  <img src="images/LaTex/5.png" alt="biber版本">
</a >
</p >

#### 选择LaTex编辑器

vscode texstudio texpad

 在这里主要介绍vscode配合LaTex Workshop插件完成LaTex编辑。

##### 下载安装vscode studio code -code editing.Redefined

https://code.visualstudio.com/

<p align="center">
<a>
  <img src="images/LaTex/6.png" alt="vscode官网下载安装">
</a >
</p >

##### 安装完vscode后，需要安装LaTex Workshop插件

<p align="center">
<a>
  <img src="images/LaTex/7.png" alt="vscode下载安装LaTex workshop插件">
</a >
</p >

使用vscode作为LaTex编辑器时，需要特别配置编译工具及编译工具链。对于包含目录、参考文献以及图片表格引用的LaTex文档，往往需要多个编译工具串联编译。

### LaTex基本介绍

#### 编译设置

LaTex模板需要我们用合适的工具进行编译，才能生成最终PDF文件
BIThesis的模板编译方式大同小异，我们都会使用xelatex、biber以及latexmk等工具来编译
编译BIThesis有两种方法，直接使用latexmk进行编译，只需要使用一次latexmk即可编译整个模板，自动识别中文环境与参考文献编译器，增量编译，推荐使用。
使用xelatex配合biber，全量编译，时间较长。

#### 使用vscode配合latex workshop，并使用latexmk进行编译

打开vscode，用ctrl+shift+p打开命令面板，输入setting.json打开配置文件

<p align="center">
<a>
  <img src="images/LaTex/8.png" alt="setting.json文件">
</a >
</p >

在setting.json中添加如下内容定义latexmk这一工具

<p align="center">
<a>
  <img src="images/LaTex/9.png" alt="latex mk文件">
</a >
</p >

再添加如下内容定义整个工具链

<p align="center">
<a>
  <img src="images/LaTex/10.png" alt="定义整个工具链">
</a >
</p >

保存设置后，在用ctrl+shift+p打开命令执行栏，搜索LaTex Workshop：Build with recipe.选择自己需要的recipe

<p align="center">
<a>
  <img src="images/LaTex/11.png" alt="搜索LaTex Workshop">
</a >
</p >

#### 新建LaTex文档

快捷键ctrl + N 新建文档，点击右下角“纯文档”，在弹出的框选择latex，保存文档，在文档中输入以下文字，保存文档后，保存右上角的第二个按钮view latex pdf file，可以预览文档。

<p align="center">
<a>
  <img src="images/LaTex/13.png" alt="搜索LaTex Workshop">
</a >
</p >

 可以看到文档带有默认格式包括首行缩进，首字母大写等，模板中第一行代码声明了文档的类型，称为类class，class控制着文档的整体类型，在这以后，需要将文章的内容写在，\begin{document} 和 \end{document}之间。

对文章内容修改以后,单击recipe，选择latemk，可以在状态栏看到相关状态信息，同时在文档文件夹生成pdf文件。

#### 常用语法介绍

接下来需要给文档添加标题，作者和日期，将写在\begin{document}之前的内容称为序言，标题，作者和日期应该写在序言中，示例代码如下。

<p align="center">
<a>
  <img src="images/LaTex/14.png" alt="标题作者单位等">
</a >
</p >

注意需要在正文中使用\maketitle 命令，生成title信息。

#### 文本格式化命令

简单的文本格式化命令
粗体：
斜体：
下滑线：
示例代码如下：

```
\documentclass{article}
\begin{document}
some of the \textbf{greatest} discoveries in \underline{science} were made by \textbf{\textit{accident}}
\end{document}
```

**文本格式化命令是可以嵌套使用的**
在文件中添加图片,示例代码如下所示，由于LaTex不能独立管理图片，因此，需要引如package包管理， 引入graphics包管理图片，其中graphicspath命令指定了图片的文件路径，在引用图片的位置处，使用includegraphics插入图片即可，

<p align="center">
<a>
  <img src="images/LaTex/15.png" alt="使用graphics插入图片">
</a >
</p >

对插入的图片可以插入标题，标签和参考， 示例代码如下

<p align="center">
<a>
  <img src="images/LaTex/16.png" alt="标题标签和参考">
</a >
</p >

在引入图片时，需要将图片引入代码包含在figure 标签中，标题和标签内容使用caption和label 命令添加在图片名称之后，
在正文中，可以使用ref和pageref命令来引用图片的标题和所在页码。

添加表格，示例代码如下，

<p align="center">
<a>
  <img src="images/LaTex/表17.png" alt="添加表格">
</a >
</p >

利用\tabular 即可创建表格，需要事先指定元素数{ccc}，每个元素之间使用&连接，使用双反斜杠\\换行，
latex中表格快速生成网址

``` https://www.tablesgenerator.com/ ```

创建一个无序号列表，使用itemizem命令包含列表部分，在每一项前加item即可。有序列表即将
itemize换成enumerate即可。
示例代码如下：

<p align="center">
<a>
  <img src="images/LaTex/无序表格.png" alt="无序列表">
</a >
</p >

<p align="center">
<a>
  <img src="images/LaTex/有序列表.png" alt="有序列表">
</a >
</p >

添加数学公式，示例代码如下，示例中包含行内公式和独立公式，行内公式用$命令包含，
独立公式用equation命令，普通引用使用单equation即可，使用一对equation命令可以给公式编号。

<p align="center">
<a>
  <img src="images/LaTex/有序列表.png" alt="有序列表">
</a >
</p >

使用abstract命令添加论文摘要，示例代码如下

<p align="center">
<a>
  <img src="images/LaTex/论文摘要.png" alt="论文摘要">
</a >
</p >

**使用两次换行将会另起一段**

给文章添加章节，示例代码如下，LaTex的章节序号是自动添加的，可以使用section，subsection和subsubsection
等命令添加不同层次的章节。

<p align="center">
<a>
  <img src="images/LaTex/给文章添加章节.png" alt="给文章添加章节">
</a >
</p >

添加目录，示例代码如下，使用tablecontents命令，即可添加LaTex自动生成的目录。

<p align="center">
<a>
  <img src="images/LaTex/目录.png" alt="目录">
</a >
</p >

有关LaTex的更多技巧，可访问网址，

```
https://www.overleaf.com/learn/latex/Main_Page
```

### 下载和使用BIThesis

在github仓库可以下载模板

```
https://github.com/spencerwooo/BIThesis/releases/
```

以本科生模板为例，介绍模板的使用方法

#### 文档结构

<p align = "justify">文档中各文件直接的关系如下图所示，main.tex是主文件，chapters文件夹包含整个毕业论文的摘要和正文的全部章节，
abstract是论文的摘要，包含中文摘要与英文摘要，chapter_1是正文第一章，正式章节。misc文件夹包含有毕业论文模板中的封面，
后置章节和参考文献，cover是毕业论文的封面。originality.tex是毕业论文的原创性声明，toc.tex是毕业论文的目录，一般情况无序更改，
由LaTex自动生成，conclusion.tex是论文的结论，reference.tex是毕业论文的参考文献，appendix是毕业论文的附录，acknowledgements.tex是
毕业论文的致谢。ref是参考文献BibTex的数据库，</p>

<p align="center">
<a>
  <img src="images/LaTex/bithesis文档之间的依赖关系.png" alt="bithesis文档之间的依赖关系">
</a >
</p >

#### 模块介绍

毕业设计论文分为多个模块，BIThesis将每个模块单独提取出来，生成单独的LaTex文件，使用main主文件统一引用，
方便进行分模块的修改，
开始，定义中英文标题，关系到封面的渲染以及以后各模块中的标题
中英文标题的定义在main.tex的第65到第67行。

<p align="center">
<a>
  <img src="images/LaTex/标题的定义.png" alt="标题的定义">
</a >
</p >

定义个人信息，将被渲染在毕业论文的封面。

<p align="center">
<a>
  <img src="images/LaTex/个人信息定义.png" alt="个人信息定义">
</a >
</p >

**中英文摘要**

模板摘要位于abstract.tex中，中文摘要位于abstract.tex的第41行到第48行，

<p align="center">
<a>
  <img src="images/LaTex/中文摘要.png" alt="中文摘要">
</a >
</p >

英文摘要位于abstract.tex的第71行到第76行，

<p align="center">
<a>
  <img src="images/LaTex/英文摘要.png" alt="英文摘要">
</a >
</p >

**正文**

模板中在第一章节创建了相应的模板，可以模仿创建其他章节的tex文件，并在main.tex的
第199行中添加对应章节文件的相对路径引用。

<p align="center">
<a>
  <img src="images/LaTex/章节引用.png" alt="章节引用">
</a >
</p >

之后可以在每个章节的独立tex文件中撰写每一章节的内容

#### 后续模块

结论，参考文献，附录和致谢模块。
不需要撰写参考文献模块，只需要撰写结论，附录和致谢模块，
这三个模块的撰写逻辑与前面正文章节的撰写逻辑是一致的。

#### 参考文献管理

需要将论文的bibtex引用粘贴到ref.bib中，引用格式如下

```正文，正文，正文\cite{sss}正文正文```

### 格式转化

#### 论文格式转换 利用pandoc进行格式转换

pandoc安装说明官方文档

```https://pandoc.org/installing.html```

windows上，可以直接在github的release页面，
```https://github.com/jgm/pandoc/release/tag/2.9.2.1```
下载windows的msi进行手动安装。

完善word格式的模板文件

#### 进行格式转换

 直接转换
 利用word模板进行转换