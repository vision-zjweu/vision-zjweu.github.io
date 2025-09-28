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
			<img src="{{site.url}}/assets/hw2/hw2.png" alt="Laplacian pyramids" width="800px">
		</figure>
	</div>
	<figcaption>图 1: 使用 6 层的拉普拉斯金字塔进行融合。请注意，你的结果可能会与我们的有所不同。
  </figcaption>
</figure>



### 1 

*(60 points)* 


首先，我们将实现以下函数，这些函数将用于：从一幅图像构建拉普拉斯金字塔，以及从拉普拉斯金字塔重建图像。

请回忆：我们需要在高斯金字塔中进行降采样（downsample），并在拉普拉斯金字塔中进行升采样（upsample）。在你的实现中，**金字塔的升采样与降采样都应使用高斯核**。用于升采样的核应与用于降采样的核相同，只是其核本身需要再乘以 4。（提示：在实现金字塔升采样时，`np.insert` 可能会很有用；在实现 `pyramid.upsample` 与 `pyramid.downsample` 时，`scipy.ndimage.gaussian_filter` 也可能派上用场——务必阅读其关于“radius”的参数说明，以便设置正确的核大小）。将高斯核的标准差设为 $$\sigma = 1$$。

- `pyramid_upsample`	
-  `pyramid_downsample` 

现在你已经可以对图像进行降采样与升采样了，就可以实现 **高斯金字塔** 和 **拉普拉斯金字塔** 了。（提示：记住，构建拉普拉斯金字塔需要从**高斯金字塔的最高层**开始。）

- `gen_gaussian_pyramid `
- `gen laplacian pyramid`


现在你已经能够生成拉普拉斯金字塔了，可以用它来重建原始图像。请使用 **4 层** 的拉普拉斯金字塔。回顾课堂内容：要重建原始图像，需要**从高斯金字塔的最高层开始**，反复进行**上采样**，然后与**下一层金字塔的拉普拉斯**相加。

请绘制：**原始图像**、**拉普拉斯金字塔**以及**重建后的图像**。

（另外要注意：在图像相减时，`numpy` 与 `cv2` 可能会进行裁剪（clipping）。请尝试使用**其他方式**进行图像相减，以确保不会发生裁剪！）

	
对提供的宠物图片应用水平与垂直梯度滤波器 $$[1 -1]$$ 和 $$[1 − 1]^T$$，分别得到滤波响应 $$I_x$$ 与 $$I_y$$。 <span class="code">请编写函数 `convolve(im, h)`，其输入为灰度图像与二维滤波器，输出为两者的卷积结果</span>。请不要使用诸如 `scipy` 中的“黑箱”滤波函数（内置库函数，例如现成的卷积/相关 API）。你可以使用 `numpy.dot`（并非必须）；更建议将卷积实现为**嵌套的 for 循环**。随后按示例代码计算边缘强度，并可视化 $$I_x$$、$$I_y$$ 以及边缘强度图。边缘强度建议按 $$I_x^2 + I_y^2$$ 计算。  


### 2 

*(30 points)* 


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