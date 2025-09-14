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

## Instructions

This homework is **due at {{ page.due }}**.

The submission includes two parts:

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
- Colorspaces



## 宠物图像边缘检测

一家大型宠物零食供应商**Petsco** 认为，计算机视觉可以帮助他们在与长期竞争对手 **Petsmart** 的较量中取得优势。  为了避免采用目前行业内普遍的做法（逐一手工标注猫狗图像中的边缘），他们希望我们能够提供一种自动化的解决方案。


<figure class="figure-container">
	<div class="flex-container">
		<figure>
			<img src="{{site.url}}/assets/hw1/dog-1.jpg" alt="vis_{{i}}" width="300px">
			<figcaption>(a) 一张输入图片</figcaption>
		</figure>
				<figure>
			<img src="{{site.url}}/assets/hw1/dog-2.jpg" alt="vis_{{i}}" width="300px">
			<figcaption>(b) 失败案例 </figcaption>
		</figure>
	</div>
	<figcaption>图 1: (a) 由Pestco提供的一张宠物照片。  (b) 简单边缘检测器的一个失败案例。  这些图像已在起始代码中提供。  </figcaption>
</figure>

### 1 卷积

*(25 points)* 
	
对提供的宠物图片应用水平与垂直梯度滤波器 $$[1 -1]$$ 和 $$[1 − 1]^T$$，分别得到滤波响应 $$I_x$$ 与 $$I_y$$。 <span class="code">请编写函数 `convolve(im, h)`，其输入为灰度图像与二维滤波器，输出为两者的卷积结果</span>。请不要使用诸如 `scipy` 中的“黑箱”滤波函数（例如现成的卷积/相关 API）。你可以使用 `numpy.dot`（并非必须）；更建议将卷积实现为**嵌套的 for 循环**。随后按示例代码计算边缘强度，并可视化 $$I_x$$、$$I_y$$ 以及边缘强度图。边缘强度建议按 $$I_x^2 + I_y^2$$ 计算。  

函数返回的滤波响应应与输入图像具有**相同尺寸**。请使用**零填充**（zero padding），即假设越界的图像像素为 0。并且务必实现的是**卷积（convolution）**，而不是**互相关（cross-correlation）**（提示：卷积需要对滤波核进行翻转）。

需要注意，这种简单的滤波方法会有相当高的**错误率**——既会遗漏真实的物体边界，也会错误地检测出伪边缘。幸运的是，**Petsco** 团队将志愿对这些错误进行人工修正。

### 2 高斯滤波

*(15 points)* 

尽管你提交的边缘检测器在某些宠物上表现良好，但工程师们报告了大量失败案例，这让 Petsco 的高管非常不满。更令人担忧的是，它在处理毛发蓬松的狗（例如 “doodle” 杂交犬）时经常失效 —— 这是对我们的赞助商而言极具商业价值的市场（见图 1b）。  

Petsco 的团队认为，这一错误的根源在于：梯度滤波器常常会在高度纹理化区域触发许多微小而虚假的边缘响应。  

请友善地解决此问题：创建一种**仅对较大空间尺度的边缘**作出响应的边缘检测器。为此，请在计算梯度之前，先使用**高斯滤波（Gaussian filter）**对图像进行模糊/平滑处理。请**自行实现**你的高斯滤波器^4。请不要使用任何“黑箱”式的高斯滤波函数（例如 `scipy.ndimage.gaussian_filter`）。

在对图像应用高斯滤波和梯度滤波时，请使用**黑箱卷积函数** `scipy.ndimage.convolve`，而不是你手写的卷积实现。

1. 先在不进行模糊处理的情况下计算边缘，以便对比“处理前/处理后”的效果。  

2.  使用 **σ = 2** 和 **11 × 11** 的高斯滤波器计算模糊图像。  

3. 将高斯滤波改为**方框滤波（box filter）**：即将 **11 × 11** 滤波器中的每个值都设为 $$1/11^2$$。  

4. 在上述两幅模糊图像上分别计算边缘。  

	> 注：对于 **11 × 11** 的方框滤波器，通常每个元素为 **1/121**（即 $$1/11^2$$）。上文的 **1/112** 可能是笔误；若作业要求未明确修正，请以原文为准或在报告中加以说明。

请按照与题目 1.1 相同的方式对边缘进行可视化。你的 notebook 应当包含以下结果：  	(i) 未进行模糊处理时计算得到的边缘；  (ii) 使用高斯模糊计算得到的边缘；  (iii) 使用方框滤波计算得到的边缘；  此外，还需展示：  (iv) 高斯模糊后的图像；  (v) 方框滤波后的图像。

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


Throughout the course, a lot of the data you have access to will be in the form of an image. These won't be stored and saved in the same format that you're used to when interacting with ordinary images, such as off your cell phone: sometimes they'll have negative values, really really big values, or invalid values. If you can look at images quickly, then you'll find bugs quicker. If you **only** print debug, you'll have a bad time. To teach you about interpreting things, I've got a bunch of mystery data that we'll analyze together. You'll write a brief version of the important `imsave` function for visualizing.

Let's load some of this mysterious data.

```console?lang=python&prompt=>>>
>>> X = np.load("mysterydata/mysterydata.npy")
>>> X
array([[[0, 0, 0, ..., 0, 0, 1],
		[0, 0, 0, ..., 0, 0, 0],
		[0, 0, 0, ..., 0, 0, 0],
		...,
		[0, 0, 0, ..., 0, 0, 0],
		[0, 0, 0, ..., 0, 0, 0],
		[0, 0, 0, ..., 0, 0, 0]],

	   ... (some more zeros) ...

	   [[0, 0, 0, ..., 0, 0, 0],
		[0, 0, 0, ..., 0, 0, 0],
		[0, 0, 0, ..., 0, 0, 0],
		...,
		[0, 0, 0, ..., 0, 0, 0],
		[0, 0, 0, ..., 0, 0, 0],
		[0, 0, 0, ..., 0, 0, 0]]], dtype=uint32)
```

Looks like it's a bunch of zeros. Nothing to see here folks! For better or worse: Python only shows the sides of an array when printing it and the sides of images that have gone through some processing tend to not be representative.

After you print it out, you should always look at the **shape** of the array and **the data type**.

```console?lang=python&prompt=>>>
>>> X.shape
(512, 512, 9)
>>> X.dtype
dtype('uint32')
```

The shape and datatype are really important. If something is an unexpected shape, that's **really bad**. This is similar to doing a physics problem where you're trying to measure the speed of light (which should be in m/s), and getting a result that is in gauss. The computer may happily chug along and produce a number, but if the units or shape are wrong, the answer is almost certainly wrong. The datatype is also really important, because data type conversion can be lossy: if you have values between 0 and 1, and you convert to an integer format, you'll end up with 0s and 1s.

In this particular case, the data has height 512 (the first dimension), width 512 (the second), and 9 channels (the last). These order of dimensions here is a _convention_, meaning that there are multiple equivalent options. For instance, in other conventions, the channel is the first dimension. Generally, you'll be given data that is $$HxWxC$$ and we'll try to be clear about how data is stored. If later on, you're given some other piece of data, figuring out the convention is a bit like rotating a picture until it looks right: figure out what you expect, and flip things until it looks like you'd expect.

If you've got an image, after you print it, you probably want to _visualize_ the output.

We don't see in 9 color channels, and so you can't look at them all at once. If you've got a bug, one of the most important things to do is to look at the image. You can look at either the first channel or all of the channels as individual images.

```console?lang=python&prompt=>>>
>>> plt.imsave("vis.png",X[:,:,0])
```

You can see what the output looks like in Figure 1. This is a false color image. Given the image, you want to find the minimum value (`vmin`) and the maximum value (`vmax`) and assign colors based on where each pixel's value falls between those. These colors look like: `Low` <img src="{{ site.url }}/assets/hw1/viridis.png" alt="viridis" width="60px" height="10px"> `High`. `plt.imsave` finds `vmin` and `vmax` for you on its own by computing the minimum and maximum
of the array.

If you'd like to look at all 9 channels, save 9 images:

```console?lang=python&prompt=>>>,...
>>> for i in range(9):
...     plt.imsave("vis_%d.png" % i,X[:,:,i])
```

If you’re inside a normal python interpreter, you can do a for loop. If you’re inside a debugger, it’s sometimes hard to do a for loop. If you promise not to tell your programming languages friends, you can use side effects to do a for loop to save the data.

```console?lang=python&prompt=(Pdb)
(Pdb) [plt.imsave("vis_%d.png" % i,X[:,:,i]) for i in range(9)]
[None, None, None, None, None, None, None, None, None]
```

The list of `None`s is just the return value of `plt.imsave`, which saves stuff to disk and returns a `None`.

If you'd like to change the colormap, you can specify this with `cmap`. For instance,

```console?lang=python&prompt=>>>,...
>>> for i in range(9):
...     plt.imsave("vis_%d.png" % i,X[:,:,i],cmap='plasma')
```

produces the outputs in Figure 2. These use the plasma colormap which looks like: `Low` <img src="{{ site.url }}/assets/hw1/falseColor/plasma_bar.png" alt="plasma" width="60px" height="10px"> `High`.

### Pixel Value Ranges

<figure class="figure-container">
	<div class="flex-container">
		{% for i in (0..8) %}
		<figure>
			<img src="{{site.url}}/assets/hw1/visualizeFig/vis2_{{i}}.png" alt="vis2_{{i}}" width="90px">
			<figcaption>vis2_{{i}}.png</figcaption>
		</figure>
		{% endfor %}
	</div>
	<figcaption>Figure 3: The Mystery Data #2</figcaption>
</figure>

1. *(2 points)* Try loading `mysterydata2.npy` and visualizing it. You should get something like Figure 3. It's hard to see stuff because one spot is _really_ bright. In this case, it's because there's a solar flare that'sproducing immense amounts of light. A common trick for making things easier to see is to apply a nonlinear correction. Here are a few options:

   $$
	p^\gamma \ \textrm{with}\  \gamma \in [0,1] \quad\textrm{or}\quad \log(p) \quad\textrm{or}\quad \log(1+p)
   $$

   where the last one can be done with `np.log1p`. Apply a nonlinear correction to the second mystery data and visualize it. {{ report }} <span class="report"> Put two of the channels i.e. `X[:,:,i]` as images into the report.</span> You can stick them side-by-side.

### Invalid Pixel Values

<figure class="figure-container">
	<div class="flex-container">
		{% for i in (0..8) %}
		<figure>
			<img src="{{site.url}}/assets/hw1/visualizeFig/vis3_{{i}}.png" alt="vis3_{{i}}" width="90px">
			<figcaption>vis3_{{i}}.png</figcaption>
		</figure>
		{% endfor %}
	</div>
	<figcaption>Figure 4: The Mystery Data #3, Visualized! (Blank is Intentional)</figcaption>
</figure>

Let's try this again, using one of the other data.

```console?lang=python&prompt=>>>,...
>>> X = np.load("mysterydata/mysterydata3.npy")
>>> for i in range(9):
...     plt.imsave("vis3_%d.png" % i,X[:,:,i])
```

The results are shown in Figure 4. They're all white. What's going on?

If you've got an uncooperative piece of data that won't visualize or produces buggy results, it's worth checking to see if all the values are reasonable. One option that'll cover all your cases is `np.isfinite`, which is `False` for values that are `NaN` (not a number) or $$\pm \infty$$ and `True` otherwise. If you then take the mean over the array, you get the fraction of entries that are normal-ish. If the mean value is _anything_ other than 1, you may be in trouble. Here:

```console?lang=python&prompt=>>>,...
>>> np.mean(np.isfinite(X))
0.6824828253851997
```

Alternatively, this also works:

```console?lang=python&prompt=>>>,...
>>> np.sum(~np.isfinite(X))
749117
```

Other things that check things are `np.isnan` (which returns `True` for `NaNs`) and `np.isinf` (which returns `True` for infinite values). Even a single `NaN` in your data is a problem: any number that touches another `NaN` turns into a `NaN`. The totally white values happen because `plt.imsave` tries to find `vmin`/`vmax`,
and the minimum of a value and a `NaN` is a `NaN`. The resulting color is a `NaN` as well. If you've got `NaN`s in your data, many functions you may want to use (e.g., `mean`, `sum`, `min`, `max`, `median`, `quantile`) have a "nan" version.

{:start="2"} 
2. *(2 points)* Fix the images by determining the right range (use `np.nanmin`, `np.nanmax`) and then pass arguments into `plt.imsave` to set the range for the visualization. To figure out what arguments to set, look at the documentation for `plt.imsave`. {{ report }} <span class="report">Put two images from `mysterydata3.npy` in the report.</span>

### Rolling Your Own `plt.imsave`

You'll make your own `plt.imsave` by filling in `colormapArray`. Here's how the false color image works:
You're given a $$H \times W$$ matrix $$X$$ and a colormap matrix $$C$$ that is $$N \times 3$$ where each row is a color red/green/blue. You produce a $$H \times W \times 3$$ matrix $$O$$. Each scalar-valued pixel $$X[i,j]$$ gets converted into red/green/blue values for $$O[i,j,:]$$ following this rule: if the pixel has value $$v$$ the corresponding output color comes from a row determined by (approximately)

$$
(N-1) \frac{(v-v_\textrm{min})}{(v_\textrm{max} - v_\textrm{min})}.
$$

However, you'll have to be very careful -- this precise equation won't always work. As an exercise -- can you spot something that might cause the expression to be a `NaN`?

{:start="3"} 
3. *(3 points)* {{ code }} <span class="code">Fill in `colormapArray`.</span> To test, you'll have to write some calling code in the main part. You can use either `plt.imsave` or `cv2.imwrite` to save the image to an file. {{ report }} <span class="report">Include source code in the report</span>.

4. *(3 points)* {{ report }} <span class="report">Visualize `mysterydata4.npy` using your system without it crashing and put all nine images into your report</span>. You may have to make a design decision about
what to do about results that are undefined. If the results are undefined, then any option that seems reasonable is fine. Your colormap should look similar to Figure 2. If the colors look inverted, see Beware 3!

<div class="primer-spec-callout warning" markdown="1">
  
   **Beware:**
	
   1. There are a bunch of edge cases in the equation for the color: it won't always return an integer between $$0$$ and $$N-1$$. It will also definitely blow up under certain input conditions (also, watch the type).
   2. You're asked by the code to return a `HxW uint8` image. There are a lot of shortcuts/implied sizes and shapes in computer vision notation -- since this is a `HxW` *color* image, it should have 3 channels (i.e., be `HxWx3`). Since it's `uint8`, you should make the image go from $$0$$ to $$255$$ before returning it (otherwise everything gets clipped to $$0$$ and $$1$$, which correspond to the two lowest brightness settings). Like all other jargon, this is annoying until it is learned; after it is learned, it is then useful.
   3. If you choose to save the results using opencv, you may have blue and red flipped -- `opencv` assumes blue is first and the rest of the world assumes red is first. You can identify this by the fact that the columns of the are defined as Red/Green/Blue and there is a lot of blue and not much red in the lowest entry.

</div>


# 任务清单

本节旨在帮助你理清并跟踪需要完成的各项任务：  


- [ ] **Pet edge detection**:
  - [ ] 1 - {{ code }} 2 images from `mysterydata2.npy`
  - [ ] 2 - {{ code }} 2 images from `mysterydata3.npy`
  - [ ] 3 - {{ code }}  `colorMapArray`
  - [ ] 4 - {{ code }} 9 images from `mysterydata4.npy`
  - [ ] 4 - {{ code }} 9 images from `mysterydata4.npy`


# 提交清单

学习通里提交文件清单:
- [ ] `你的学号.ipynb`
- [ ] `你的学号.pdf`

