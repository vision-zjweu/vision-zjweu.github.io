---
layout: spec
permalink: /hw2
latex: true

title: PS 2 – Signal Processing
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

# PS 2 – Signal Processing

<!-- <div class="primer-spec-callout warning" markdown="1">
   
   **Important:** Changes to rubric and submission format announced on Piazza [@58](https://piazza.com/class/lr0rfd6e5dm5wf/post/58){:target="_blank"} .

</div> -->

<!-- ## Image blending -->




## Image blending

我们将使用拉普拉斯金字塔（Laplacian pyramids）来融合两张图像。

<figure class="figure-container">
	<div class="flex-container">
		<figure>
			<img src="{{site.url}}/assets/hw2/hwk2.png" alt="vis_{{i}}" width="500px">
		</figure>
	</div>
	<figcaption>图 1: 使用 6 层的拉普拉斯金字塔进行融合。请注意，你的结果可能会与我们的有所不同。
  </figcaption>
</figure>



### 1 

*(60 points)* 


首先，我们将实现以下函数，这些函数将用于：从一幅图像构建拉普拉斯金字塔，以及从拉普拉斯金字塔重建图像。

请回忆：我们需要在高斯金字塔中进行下采样，并在拉普拉斯金字塔中进行上/下采样。在你的实现中，**金字塔的上采样与下采样都应使用高斯核**。用于上采样的核应与用于下采样的核相同，只是其核本身需要再乘以 4。（提示：在实现金字塔上采样时，`np.insert` 可能会很有用；在实现 `pyramid.upsample` 与 `pyramid.downsample` 时，`scipy.ndimage.gaussian_filter` 也可能派上用场——务必阅读其关于“radius”的参数说明，以便设置正确的核大小）。将高斯核的标准差设为 **σ = 1**。

- **pyramid upsample（金字塔上采样）**
- **pyramid downsample（金字塔下采样）**

现在你已经可以对图像进行下采样与上采样了，就可以实现 **高斯金字塔** 和 **拉普拉斯金字塔** 了。（提示：记住，构建拉普拉斯金字塔需要从**高斯金字塔的最高层**开始。）

- **gen gaussian pyramid（生成高斯金字塔）**
- **gen laplacian pyramid（生成拉普拉斯金字塔）**



	
对提供的宠物图片应用水平与垂直梯度滤波器 $$[1 -1]$$ 和 $$[1 − 1]^T$$，分别得到滤波响应 $$I_x$$ 与 $$I_y$$。 <span class="code">请编写函数 `convolve(im, h)`，其输入为灰度图像与二维滤波器，输出为两者的卷积结果</span>。请不要使用诸如 `scipy` 中的“黑箱”滤波函数（内置库函数，例如现成的卷积/相关 API）。你可以使用 `numpy.dot`（并非必须）；更建议将卷积实现为**嵌套的 for 循环**。随后按示例代码计算边缘强度，并可视化 $$I_x$$、$$I_y$$ 以及边缘强度图。边缘强度建议按 $$I_x^2 + I_y^2$$ 计算。  

函数返回的滤波响应应与输入图像具有**相同尺寸**。请使用**零填充**（zero padding），即假设越界的图像像素为 0。并且务必实现的是**卷积（convolution）**，而不是**互相关（cross-correlation）**（提示：卷积需要对滤波核进行翻转）。

需要注意，这种简单的滤波方法会有相当高的**错误率**——既会遗漏真实的物体边界，也会错误地检测出伪边缘。幸运的是，**Petsco** 团队将志愿对这些错误进行人工修正。

### 2 高斯滤波

*(20 points)* 


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

请解决此问题：创建一种**仅对较大空间尺度的边缘**作出响应的边缘检测器。为此，请在计算梯度之前，先使用**高斯滤波（Gaussian filter）**对图像进行模糊/平滑处理。<span class="code">请**自行实现**你的高斯滤波器。请不要使用任何“黑箱”式的高斯滤波函数（例如 `scipy.ndimage.gaussian_filter`）。</span>
> 提示：务必确保滤波核正确**居中**对齐。



在对图像应用高斯滤波和梯度滤波时，请使用**黑箱卷积函数**（内置库函数） `scipy.ndimage.convolve`，而不是你手写的卷积实现。


1. 先在不进行模糊处理的情况下计算边缘，以便对比“处理前/处理后”的效果。  

2.  使用 **σ = 2** 和 **11 × 11** 的高斯滤波器计算模糊图像。  

3. 将高斯滤波改为**方框滤波（box filter）**：即将 **11 × 11** 滤波器中的每个值都设为 $$1/11^2$$。  

4. 在上述两幅模糊图像上分别计算边缘。  


请按照与题目 1 相同的方式对边缘进行可视化。你的 notebook 应当包含以下结果：  


<ol type="i">
  <li>未进行模糊处理时计算得到的边缘； </li>
  <li>使用高斯模糊计算得到的边缘； </li>
  <li>使用方框滤波计算得到的边缘；  </li>
    <li>高斯模糊后的图像；   </li>
	  <li>方框滤波后的图像。 </li>
    <!-- <ol type="i">
      <li>Sub-item i</li>
      <li>Sub-item ii</li>
    </ol> -->
  
</ol>

<!-- i. 

ii.  

3. 使用方框滤波计算得到的边缘；  

4. 高斯模糊后的图像；  

5. 方框滤波后的图像。 -->

### 3 过滤器改进

*(20 points)* 


与其先对图像进行两次卷积来计算 $$I_x$$（即先用高斯模糊滤波器，再用梯度滤波器），不如创建一个单一的滤波器 $$G_x$$，其输出应与两次卷积的结果相同。你可以复用 （2） 部分中的函数。请使用提供的代码对该滤波器进行可视化。

### 4 可旋转滤波器

*(20 points)* 

 好消息！Petsco 的高管们对你的工作非常满意，并已决定为后续的若干作业继续提供资助。  
然而，在我们最近的一次会议上，他们又提出了新的要求：Petsco 的竞争对手 Petsmart 现在已经能够计算宠物图像中的**有方向性的边缘**。  
与仅估计水平或垂直梯度不同，他们可以在任意角度 $$\theta$$ 下获得梯度。  

不过，Petsmart 的方法在计算上非常昂贵：他们会为每一个角度构造一个新的滤波器，并用其对图像进行滤波。  
Petsco 认为，通过利用本课程中关于**可旋转滤波器（steerable filters）**的知识，有机会在这一领域超越对手。  

请编写一个函数 `oriented_grad(Ix, Iy, θ)`，在给定水平梯度 `Ix` 与垂直梯度 `Iy` 的情况下，返回在角度 θ 方向上的引导梯度。  使用该函数在输入图像的模糊版本上计算梯度，其中 θ ∈ {π/4, π/2, 3π/4}，并采用与 (2) 部分相同的高斯模糊核。  

请按照题目 1 中对梯度的可视化方式展示这些结果。

<!-- ### 5 噪声消除

*(25 points)* 


Petsco 还希望能够提供**提升低质量宠物照片**的功能。  
给定一张带噪声的宠物图像，请使用你在 (2) 部分中开发的**方框滤波器**和**高斯滤波器**去除噪声。  

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
(v) 使用 $$k = 7$$ 的中值滤波结果。   -->




# 任务清单

本节旨在帮助你理清并跟踪需要完成的各项任务：  


- [ ] **宠物图像边缘检测**:
  - [ ] 1 - {{ code }} **(40分)** 卷积
  - [ ] 2 - {{ code }} **(20分)** 高斯滤波器
  - [ ] 3 - {{ code }} **(20分)** 过滤器改进
  - [ ] 4 - {{ code }} **(20分)** 可旋转滤波器
  <!-- - [ ] 4 - {{ code }} **(25分)** 噪声消除 -->


# 提交清单

学习通里提交文件清单:
- [ ] `你的学号.ipynb`
- [ ] `你的学号.pdf`

# 参考结果

1.  

<img src="{{site.url}}/assets/hw1/ref1.png" alt="vis_{{i}}" width="500px">

2.

<img src="{{site.url}}/assets/hw1/ref2.png" alt="vis_{{i}}" width="500px">

4.

<img src="{{site.url}}/assets/hw1/ref3.png" alt="vis_{{i}}" width="500px">