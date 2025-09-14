---
layout: spec
permalink: /hw2
latex: true

title: Homework 1 – Filtering
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

# Project 1: Filtering

<!-- <div class="primer-spec-callout warning" markdown="1">
   
   **Important:** Changes to rubric and submission format announced on Piazza [@58](https://piazza.com/class/lr0rfd6e5dm5wf/post/58){:target="_blank"} .

</div> -->

## 说明

项目截止时间 **{{ page.due }}**.

<!-- The submission includes two parts:

1. **To Canvas**: submit a `zip` file containing a **single** directory with your **uniqname** as the name that contains all your code (subdirectories are fine).

   {{ code }} -
   <span class="code">We have indicated questions where you have to do something in code in red. **If gradecope asks for it, also submit your code in the report with the formatting below.** </span>  

   Starter code is given to you on Canvas under the "Homework 1" assignment. You can also download it [here](https://drive.google.com/file/d/1Bpqc8LLT67uLR4ZmbKYeMwvvYMrw_Gl-/view?usp=sharing). Clean up your submission to include only the necessary files.

   <div class="primer-spec-callout info" markdown="1">
	 **Submission Tip:** Use the [Tasks Checklist](#tasks-checklist) and [Canvas Submission Checklist](#canvas-submission-checklist) at the end of this homework. We also provide a script that validates the submission format [here](https://raw.githubusercontent.com/eecs442/utils/master/check_submission.py){:target="_blank"}.
   </div>

2. **To Gradescope**: submit a `pdf` file as your write-up, including your answers to all the questions and key choices you made.


## Overview

In this assignment, you’ll work through three tasks that help set you up for success in the class as well as a short assignment involving playing with color. The assignment has three goals.

1. **Show you bugs in a low-stakes setting**. You’ll encounter a lot of programming mistakes in the course and we want to show you common bugs early on. Here, the programming problems are deliberately easy!
2. **Learn to write reasonably good Python and NumPy code**. Having layers of nested `for` loops will cause bugs and is not feasible for us to debug, use NumPy effectively! 

The assignment has four parts and corresponding folders in the starter code:

- Numpy Intro (folder `numpy/`)
- Data Interpretation and Visualization (folder `visualize/`)
- Lights on a Budget (folder `dither/`)
- Colorspaces -->



## 宠物图像边缘检测

一家大型宠物零食供应商**Petsco** 认为，计算机视觉可以帮助他们在与长期竞争对手 **Petsmart** 的较量中取得优势。  为了避免采用目前行业内普遍的做法（逐一手工标注猫狗图像中的边缘），他们希望我们能够提供一种自动化的解决方案。




### 1 卷积

*(25 points)* 

<figure class="figure-container">
	<div class="flex-container">
		<figure>
			<img src="{{site.url}}/assets/hw1/dog-1.jpg" alt="vis_{{i}}" width="500px">
		</figure>
	</div>
	<figcaption>图 1: 由Pestco提供的一张宠物照片。  </figcaption>
</figure>

	
对提供的宠物图片应用水平与垂直梯度滤波器 $$[1 -1]$$ 和 $$[1 − 1]^T$$，分别得到滤波响应 $$I_x$$ 与 $$I_y$$。 <span class="code">请编写函数 `convolve(im, h)`，其输入为灰度图像与二维滤波器，输出为两者的卷积结果</span>。请不要使用诸如 `scipy` 中的“黑箱”滤波函数（内置库函数，例如现成的卷积/相关 API）。你可以使用 `numpy.dot`（并非必须）；更建议将卷积实现为**嵌套的 for 循环**。随后按示例代码计算边缘强度，并可视化 $$I_x$$、$$I_y$$ 以及边缘强度图。边缘强度建议按 $$I_x^2 + I_y^2$$ 计算。  

函数返回的滤波响应应与输入图像具有**相同尺寸**。请使用**零填充**（zero padding），即假设越界的图像像素为 0。并且务必实现的是**卷积（convolution）**，而不是**互相关（cross-correlation）**（提示：卷积需要对滤波核进行翻转）。

需要注意，这种简单的滤波方法会有相当高的**错误率**——既会遗漏真实的物体边界，也会错误地检测出伪边缘。幸运的是，**Petsco** 团队将志愿对这些错误进行人工修正。

### 2 高斯滤波

*(15 points)* 


<figure class="figure-container">
	<div class="flex-container">
		<figure>
			<img src="{{site.url}}/assets/hw1/dog-2.jpg" alt="vis_{{i}}" width="500px">
		</figure>
	</div>
	<figcaption>图 2:简单边缘检测器的一个失败案例。 </figcaption>
</figure>


尽管你提交的边缘检测器在某些宠物上表现良好，但工程师们报告了大量失败案例，这让 Petsco 的高管非常不满。更令人担忧的是，它在处理毛发蓬松的狗（例如 “doodle” 杂交犬）时经常失效 —— 这是对我们的赞助商而言极具商业价值的市场（见图 2）。  

Petsco 的团队认为，这一错误的根源在于：梯度滤波器常常会在高度纹理化区域触发许多微小而虚假的边缘响应。  

请解决此问题：创建一种**仅对较大空间尺度的边缘**作出响应的边缘检测器。为此，请在计算梯度之前，先使用**高斯滤波（Gaussian filter）**对图像进行模糊/平滑处理。请**自行实现**你的高斯滤波器。请不要使用任何“黑箱”式的高斯滤波函数（例如 `scipy.ndimage.gaussian_filter`）。
> 提示：务必确保滤波核正确**居中**对齐。



在对图像应用高斯滤波和梯度滤波时，请使用**黑箱卷积函数**（内置库函数） `scipy.ndimage.convolve`，而不是你手写的卷积实现。


1. 先在不进行模糊处理的情况下计算边缘，以便对比“处理前/处理后”的效果。  

2.  使用 **σ = 2** 和 **11 × 11** 的高斯滤波器计算模糊图像。  

3. 将高斯滤波改为**方框滤波（box filter）**：即将 **11 × 11** 滤波器中的每个值都设为 $$1/11^2$$。  

4. 在上述两幅模糊图像上分别计算边缘。  


请按照与题目 1.1 相同的方式对边缘进行可视化。你的 notebook 应当包含以下结果：  	

(i) 未进行模糊处理时计算得到的边缘； 

(ii) 使用高斯模糊计算得到的边缘；  

(iii) 使用方框滤波计算得到的边缘；  

此外，还需展示：  

(iv) 高斯模糊后的图像；  

(v) 方框滤波后的图像。

### 3 过滤器改进

*(15 points)* 


与其先对图像进行两次卷积来计算 $$I_x$$（即先用高斯模糊滤波器，再用梯度滤波器），不如创建一个单一的滤波器 $$G_x$$，其输出应与两次卷积的结果相同。你可以复用 （2） 部分中的函数。请使用提供的代码对该滤波器进行可视化。

### 4 可旋转滤波器

*(15 points)* 

 好消息！Petsco 的高管们对你的工作非常满意，并已决定为后续的若干作业继续提供资助。  
然而，在我们最近的一次会议上，他们又提出了新的要求：Petsco 的竞争对手 Petsmart 现在已经能够计算宠物图像中的**有方向性的边缘**。  
与仅估计水平或垂直梯度不同，他们可以在任意角度 $$\theta$$ 下获得梯度。  

不过，Petsmart 的方法在计算上非常昂贵：他们会为每一个角度构造一个新的滤波器，并用其对图像进行滤波。  
Petsco 认为，通过利用本课程中关于**可旋转滤波器（steerable filters）**的知识，有机会在这一领域超越对手。  

请编写一个函数 `oriented_grad(Ix, Iy, θ)`，在给定水平梯度 `Ix` 与垂直梯度 `Iy` 的情况下，返回在角度 θ 方向上的引导梯度。  使用该函数在输入图像的模糊版本上计算梯度，其中 θ ∈ {π/4, π/2, 3π/4}，并采用与 (c) 部分相同的高斯模糊核。  

请按照题目 1 中对梯度的可视化方式展示这些结果。

### 5 噪声消除

*(30 points)* 


Petsco 还希望能够提供**提升低质量宠物照片**的功能。  
给定一张带噪声的宠物图像，请使用你在 (c) 部分中开发的**方框滤波器**和**高斯滤波器**去除噪声。  

随后，请实现一个函数 `median(im, k)`，利用 $$k \times k$$ 的窗口进行中值滤波。  
具体来说，对于噪声图像中的每一个像素，你需要计算其邻域内 $$k \times k$$ 窗口中其他像素的**中值强度值**。  

你的代码应与 `convolve` 的实现非常相似，但在内部循环中，不再计算加权平均，而是计算**中值**。  
你可以使用内置函数（例如 `np.median`）来计算列表或数组的中值。  

请包含 $$k = 3$$ 和 $$k = 7$$ 两种情况的实验结果。  

你的解答应展示以下 5 张图像：  
(i) 含噪声的原始图像；  
(ii) 经过方框滤波的含噪声图像；  
(iii) 经过高斯滤波的含噪声图像；  
(iv) 使用 $$k = 3$$ 的中值滤波结果；  
(v) 使用 $$k = 7$$ 的中值滤波结果。  

<div class="primer-spec-callout warning" markdown="1">
  
   **Beware:**
	
   1. There are a bunch of edge cases in the equation for the color: it won't always return an integer between $$0$$ and $$N-1$$. It will also definitely blow up under certain input conditions (also, watch the type).
   2. You're asked by the code to return a `HxW uint8` image. There are a lot of shortcuts/implied sizes and shapes in computer vision notation -- since this is a `HxW` *color* image, it should have 3 channels (i.e., be `HxWx3`). Since it's `uint8`, you should make the image go from $$0$$ to $$255$$ before returning it (otherwise everything gets clipped to $$0$$ and $$1$$, which correspond to the two lowest brightness settings). Like all other jargon, this is annoying until it is learned; after it is learned, it is then useful.
   3. If you choose to save the results using opencv, you may have blue and red flipped -- `opencv` assumes blue is first and the rest of the world assumes red is first. You can identify this by the fact that the columns of the are defined as Red/Green/Blue and there is a lot of blue and not much red in the lowest entry.

</div>


# 任务清单

本节旨在帮助你理清并跟踪需要完成的各项任务：  


- [ ] **宠物图像边缘检测**:
  - [ ] 1 - {{ code }} 卷积
  - [ ] 2 - {{ code }} 高斯滤波器
  - [ ] 3 - {{ code }} 过滤器改进
  - [ ] 4 - {{ code }} 可旋转滤波器
  - [ ] 4 - {{ code }} 噪声消除


# 提交清单

学习通里提交文件清单:
- [ ] `你的学号.ipynb`
- [ ] `你的学号.pdf`

