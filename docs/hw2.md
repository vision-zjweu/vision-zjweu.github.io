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



### 1 图像金字塔

*(50 points)* 


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

- `reconstruct_img`


### 2 图像融合

*(20 points)* 

实现函数 **pyramid_blend(im1, im2, mask, num_levels)**。其输入为两幅图像和一个二值掩码（mask,指示每个像素来自哪一幅图像）。该函数应构建一个包含 **num_levels** 层的**拉普拉斯金字塔**来融合这两幅输入图像。

请使用你实现的函数，对我们在 jupyter 笔记本中提供的**橙子**与**苹果**图像进行融合。并在 **num_levels ∈ {1, 2, 3, 4, 5, 6}** 的情况下分别绘制融合结果。<span class="code">请**描述**当拉普拉斯金字塔层数变化时，融合图像的差异：当我们增加层数时，结果如何变化？请把这段文字说明直接写在 **.ipynb** 文件对应的单元格中，而不是另传一个单独文档</span>。

为了得到**彩色图像**，你可以对每个**颜色通道**独立进行融合。在我们的实现中，这不需要额外代码（由于 NumPy 的广播机制（broadcasting），同一套代码同时适用于单通道与多通道）。不过，你的实现可能有所不同。


### 3 自由发挥

*(10 points)*

请使用你的代码来融合你自己的图像，你可以通过改变mask的边界获取需要的融合边界。（可以尝试着有创意一点！）


## 傅立叶变换

### 1
*(20 points)* 

使用高斯滤波器对给定图像进行卷积：

1. 在**空间域**中直接卷积；
2. 在**频率域**中通过卷积定理进行相乘。

执行 DFT 和逆 DFT 时，请使用 `scipy.fft` 中的 `fft2` 与 `ifft2`。进行二维卷积时，请使用 `scipy.signal.convolve2d`。

注意：在空间域与频率域实现之间，图像**边界处理**可能会略有差异。对于本题来说，这是**可以接受的**。


# 任务清单

本节旨在帮助你理清并跟踪需要完成的各项任务：  


- [ ] **宠物图像边缘检测**:
  - [ ] 1 - {{ code }} **(40分)** 
	- [ ] 1.1 - {{ report }} Terminal Output
  - [ ] 2 - {{ code }} **(20分)** 高斯滤波器

  <!-- - [ ] 4 - {{ code }} **(25分)** 噪声消除 -->


# 提交清单

学习通里提交文件清单:
- [ ] `你的学号.ipynb`
- [ ] `你的学号.pdf`

# 参考结果
注意：这里仅提供部分结果
1.  

<img src="{{site.url}}/assets/hw2/hw2_ref1.png" alt="vis_{{i}}" width="500px">

<img src="{{site.url}}/assets/hw2/hw2_ref2.png" alt="vis_{{i}}" width="500px">

2.

<img src="{{site.url}}/assets/hw2/hw2_ref3.png" alt="vis_{{i}}" width="500px">

<img src="{{site.url}}/assets/hw2/hw2_ref4.png" alt="vis_{{i}}" width="500px">

<img src="{{site.url}}/assets/hw2/hw2_ref5.png" alt="vis_{{i}}" width="500px">

