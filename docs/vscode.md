---
layout: spec
permalink: /vscode
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

# VSCode 环境

<div class="primer-spec-callout warning" markdown="1">
   
   **Important:** Changes to rubric and submission format announced on Piazza [@58](https://piazza.com/class/lr0rfd6e5dm5wf/post/58){:target="_blank"} .

</div>

## 简介

VSCode 是一个基于图形界面的文本编辑器和开发环境。系里的电脑上已经预装了 VSCode，本地电脑则可以从 VSCode 官网下载安装。安装完成后，我们会将 VSCode 配置为使用我们的虚拟环境。VSCode 是本课程唯一正式支持的 IDE。

*本指南假设你已经在电脑上安装了 conda 并设置好了虚拟环境。*


## 安装Python和Jupyter Notebbok插件

当你第一次在 VSCode 中打开一个 `Python` 文件时，它会提示你安装 `Python` 扩展，请务必进行安装！

我们建议你确保已安装来自 Microsoft 的 `Python` 、`Python Debugger` 和 `Pylance` 扩展。

![Alt text](assets/envi/python.png "Jupyter")

对于Jupyter Notebook，安装 Microsoft 的 `Jupyter` 扩展。 

![Alt text](assets/envi/jupyter.png "Jupyter")


## 在 VSCode 中选择 Python 解释器

当你打开一个 Python 文件时，请查看 VSCode 界面右下角。你会看到 VSCode 已经选择了一个 Python 解释器。
如果你安装的是anaconda，这里应该显示base（Python 3.X.X），那么就一切就绪，可以开始使用了。

![Alt text](assets/envi/in2.png "Jupyter")

如果它没有显示为 3.X.XX (base)，请点击该文字（实际上它是一个按钮）。
这时会弹出如下菜单：

![Alt text](assets/envi/interpreter.png "Jupyter")

如果你对使用conda配置python虚拟环境比较有经验，可以自行配置并选择对应解释器。

## 终端 vs 运行按钮

在 VSCode 中打开终端（CTRL+~）时，并不会自动进入 conda 环境，因此请务必手动激活它！

另外需要注意，使用 VSCode 的 运行按钮 执行代码，与在终端中运行代码的行为可能会有所不同。
作业 HW1 会提供一份指南，帮助你正确配置运行按钮，使其能够自动启动正确的 conda 环境。

# Debugging 代码调试

安装好 `Python` 扩展后，我们就可以进行代码调试了。相比于使用 `print` 语句，这种方式更加强大和灵活，因为我们能够交互式地检查程序的状态。

在 调试控制台`（Debug Console）` 中，我们甚至可以执行任意代码，比如交互式地查询变量或编写条件语句。

## Debugging Steps

1. 通过点击代码行左侧或在该行按下 `F9` 来设置断点。
2. 按下 `F5` 启动调试器。如果出现下拉菜单，请选择 `Python File`，以便正确配置调试器来调试该 `Python` 文件：
![Alt text](assets/envi/F5.png "Jupyter")
3. 点击左侧工具栏中的调试器图标，可以展开调试器视图。
4. 使用其中的工具可以逐步执行代码，你可以：
- 运行代码直到遇到断点
- 单步进入（step into） 函数（查看函数内部的代码）
- 单步跳过（step over） 函数（跳到当前函数的下一行）
- 单步跳出（step out） 函数（返回到当前函数调用后的下一行）
- 重新启动程序（restart）
- 停止程序（stop）3