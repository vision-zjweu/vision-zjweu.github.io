---
layout: spec
permalink: /hw2
latex: true

title: PS 3 – Introduction to Machine Learning(basecamp)
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




## Nearest Neighbor Classification



在本题中，我们将实现 **k 近邻（k-nearest neighbor, KNN）算法** 来识别微小图像（尺寸很小的图片）中的物体。  

我们将使用 **Imagenette** 数据集中的图像，它是 **ImageNet** 的一个较小且容易分类的子集（见图 1）。  



**注意：**  
在起始代码中有一个名为 `DEBUG` 的标志（flag），你可以在调试代码时将其设置为 `True`。  
当该标志被启用时，程序只会加载训练集的 **20%**，因此其余代码的运行时间会显著缩短。  

然而，在回答问题并报告最终结果之前，请务必将该标志重新设置为 `False`，并 **重新运行所有代码单元**！  

此外，代码中还提供了一个可以使用 **不同图像尺寸** 运行的选项，你可以自由尝试修改这一参数（但在提交前，请再次将其恢复为默认设置）。


**(a)** 对于笔记本中定义的 `KNearestNeighbor` 类，请完成以下方法的实现：

**i. (1 分)**  
请阅读方法 `compute_distance_two_loops` 的函数头（header），理解其输入与输出。  
根据笔记本中的提示，补全该方法的剩余部分，用以计算测试集图像与训练集图像之间的 **L2 距离**。

**说明：**  
L2 距离的计算方式是：
$$
\text{L2}(x, y) = \sqrt{\sum_i (x_i - y_i)^2}
$$
即对两幅图像中对应像素的差值平方求和后再开方。

**提示：**  
你可以使用 `np.linalg.norm` 来计算 L2 距离。


**ii. (1 分)**  
在后续的作业中，编写**高效的向量化代码（vectorized code）**将非常重要——即尽量减少 `for` 循环，通过一次性处理多个样本来提升运行速度。  
作为练习，请完成以下两个方法的实现：

- **`compute_distance_one_loop`**：仅使用**一个** `for` 循环来计算 L2 距离（即**部分向量化**版本）。  
- **`compute_distance_no_loops`**：完全不使用循环来计算 L2 距离（即**完全向量化**版本）。

**提示：**  
利用以下等式可以避免显式的循环计算：
$$
\|x - y\|_2^2 = \|x\|_2^2 + \|y\|_2^2 - 2x^T y
$$

在实现时，你可以先计算训练集和测试集的平方范数，再通过矩阵乘法高效地得到所有样本之间的距离。

**iii. (1 分)**  
完成方法 **`predict_labels`** 的实现，用于为每一张测试图像找到其 **k 个最近邻样本（k nearest neighbors）**，并根据这些邻居的标签来预测测试图像的类别。

**提示：**  
你可以使用 `np.argsort` 函数来获取距离矩阵中每一行（即每个测试样本对应的所有训练样本距离）的**从小到大排序索引**。  
通过选择前 `k` 个最小距离对应的训练样本标签，再采用**投票法（majority vote）**确定最终预测类别。

基本步骤如下：
1. 对每个测试样本，取出其与所有训练样本的距离。  
2. 使用 `np.argsort` 得到距离的升序排列索引。  
3. 选取前 `k` 个索引对应的训练集标签。  
4. 统计出现次数最多的标签，作为该测试样本的预测结果。


**(b)（0 分）**  
运行后续的代码单元，以便检查你在上面完成的实现。你将使用 `KNearestNeighbor` 来对测试图像进行预测，并计算这些预测的**准确率**。  
我们已经为 `k = 1` 和 `k = 3` 实现了评估代码。对于 **k = 1**，测试集准确率预计约为 **29%**（上下有小幅波动属正常）。


**(c)（1 分）**  
在验证集上用 **网格搜索（grid search）** 寻找最优的 `k`：对每个候选 `k`，用验证集计算准确率，选出最高者；在下方提供的单元中**报告**最高准确率和对应的 `k`。随后，请运行我们提供的代码，用最优 `k` 在**测试集**上计算准确率，并查看最近邻的**可视化结果**。

**（可选，0 分）**  
运行下方提供的代码单元，观察**归一化（normalization）** 对模型准确率的影响。  


<figure class="figure-container">
	<div class="flex-container">
		<figure>
			<img src="{{site.url}}/assets/hw2/hw3_1.png" alt="Laplacian pyramids" width="600px">
		</figure>
	</div>
	<figcaption>图 1: 使用 6 层的拉普拉斯金字塔进行融合。请注意，你的结果可能会与我们的有所不同。
  </figcaption>
</figure>



### 1 图像金字塔

*(50 points)* 


首先，我们将实现以下函数，这些函数将用于：从一幅图像构建拉普拉斯金字塔，以及从拉普拉斯金字塔重建图像。

请回忆：我们需要在高斯金字塔中进行降采样（downsample），并在拉普拉斯金字塔中进行升采样（upsample）。在你的实现中，**金字塔的升采样与降采样都应使用高斯核**。用于升采样的核应与用于降采样的核相同，只是其核本身需要再乘以 4。
>（提示：在实现金字塔升采样时，`np.insert` 可能会很有用；在实现 `pyramid.upsample` 与 `pyramid.downsample` 时，`scipy.ndimage.gaussian_filter` 也可能派上用场——务必阅读其关于“radius”的参数说明，以便设置正确的核大小）。将高斯核的标准差设为 $$\sigma = 1$$。

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

### 1 卷积定理
*(20 points)* 

使用高斯滤波器对给定图像进行卷积：

1. 在**空间域**中直接卷积；
2. 在**频率域**中通过卷积定理进行相乘。

执行 DFT 和逆 DFT 时，请使用 `scipy.fft` 中的 `fft2` 与 `ifft2`。进行二维卷积时，请使用 `scipy.signal.convolve2d`。

>注意：在空间域与频率域实现之间，图像**边界处理**可能会略有差异。对于本题来说，这是**可以接受的**。


# 任务清单

本节旨在帮助你理清并跟踪需要完成的各项任务：  


- [ ] **图像融合**:
  - [ ] 1 - {{ code }} **(50分)** 图像金字塔
  - [ ] 2 - {{ code }} **(20分)** 图像融合
  - [ ] 3 - {{ code }} **(10分)** 自由发挥
- [ ] **傅立叶变换**:
  - [ ] 1 - {{ code }} **(20分)** 卷积定理

  <!-- - [ ] 4 - {{ code }} **(25分)** 噪声消除 -->


# 提交清单

学习通里提交文件清单（请不要提交打包zip文件）:
- [ ] `你的学号.ipynb`
- [ ] `你的学号.pdf`

# 参考结果
注意：这里仅提供部分结果
1.  

<img src="{{site.url}}/assets/hw2/hw2_ref1.png" alt="vis_{{i}}" width="400px">

<img src="{{site.url}}/assets/hw2/hw2_ref2.png" alt="vis_{{i}}" width="400px">

2.

<img src="{{site.url}}/assets/hw2/hw2_ref3.png" alt="vis_{{i}}" width="400px">

<img src="{{site.url}}/assets/hw2/hw2_ref4.png" alt="vis_{{i}}" width="400px">

<img src="{{site.url}}/assets/hw2/hw2_ref5.png" alt="vis_{{i}}" width="400px">

