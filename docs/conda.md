---
layout: spec
permalink: /conda
latex: true

title: VSCode 环境
due: 11:59 p.m. on Wednesday January 31st, 2024
---

<link href="style.css" rel="stylesheet">
<div style="display:none">
	<!-- Define LaTeX commands here -->
	\(
		\DeclareMathOperator*{\argmin}{arg\,min}

		\newcommand{\DB}{\mathbf{D}}
		\newcommand{\NB}{\mathbf{N}}
		\newcommand{\PB}{\mathbf{P}}
		\newcommand{\SB}{\mathbf{S}}
		\newcommand{\XB}{\mathbf{X}}

		\newcommand{\xB}{\mathbf{x}}
		\newcommand{\yB}{\mathbf{y}}
	\)

</div>

{% capture code %}<i class="fa fa-code icon-large"></i>{% endcapture %}
{% capture autograde %}<i class="fa fa-robot icon-large"></i>{% endcapture %}
{% capture report %}<i class="fa fa-file icon-large"></i>{% endcapture %}

# Anaconda

当你对 Python 的工具有疑问时，可以参考 [Python 标准库文档](https://docs.python.org/3.7/library/index.html){:target="_blank"}
。

我们建议你安装最新的 [Anaconda](https://www.anaconda.com/download/){:target="_blank"} （Python 3.12 版本）。Anaconda 是一个 Python 包管理器，其中已经包含了本课程所需的大部分模块。

在本课程中，我们将大量使用以下软件包：

 - [Numpy](https://numpy.org/doc/stable/user/quickstart.html){:target="_blank"} 
 - [Matplotlib](https://matplotlib.org/stable/tutorials/introductory/pyplot.html){:target="_blank"} 
 - [OpenCV](https://opencv.org/){:target="_blank"}
 - PyTorch

