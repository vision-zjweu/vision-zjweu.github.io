---
layout: spec
permalink: /hw3
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

# PS 3  – Introduction to Machine Learning

<!-- <div class="primer-spec-callout warning" markdown="1">
   
   **Important:** Changes to rubric and submission format announced on Piazza [@58](https://piazza.com/class/lr0rfd6e5dm5wf/post/58){:target="_blank"} .

</div> -->

<!-- ## Image blending -->

分为基础部分65分和挑战部分50分，超出100以上的分数可以溢出到其他项目作业

## 基础部分（65分）
### Nearest Neighbor Classification



在本题中，我们将实现 **k 近邻（k-nearest neighbor, KNN）算法** 来识别微小图像（尺寸很小的图片）中的物体。  

我们将使用 **Imagenette** 数据集中的图像，它是 **ImageNet** 的一个较小且容易分类的子集（见图 1）。  



**注意：**  
在起始代码中有一个名为 `DEBUG` 的标志（flag），你可以在调试代码时将其设置为 `True`。  当该标志被启用时，程序只会加载训练集的 **20%**，因此其余代码的运行时间会显著缩短。  然而，在回答问题并报告最终结果之前，请务必将该标志重新设置为 `False`，并 **重新运行所有代码单元**！  此外，代码中还提供了一个可以使用 **不同图像尺寸** 运行的选项，你可以自由尝试修改这一参数（但在提交前，请再次将其恢复为默认设置）。

<figure class="figure-container">
	<div class="flex-container">
		<figure>
			<img src="{{site.url}}/assets/hw3/hw3_1.png" alt="Laplacian pyramids" width="600px">
		</figure>
	</div>
	<figcaption>**图 1:** 来自 **Imagenette** 的部分示例图像。
  </figcaption>
</figure>



**(a)** 对于笔记本中定义的 `KNearestNeighbor` 类，请完成以下方法的实现：

- **i. (15 分)**  
请阅读方法 `compute_distance_two_loops` 的函数头（header），理解其输入与输出。  
根据笔记本中的提示，补全该方法的剩余部分，用以计算测试集图像与训练集图像之间的 **L2 距离**。

	**说明：**  L2 距离的计算方式是：
	$$
	\text{L2}(x, y) = \sqrt{\sum_i (x_i - y_i)^2}
	$$
	即对两幅图像中对应像素的差值平方求和后再开方。

	>**提示：**  你可以使用 `np.linalg.norm` 来计算 L2 距离。


- **ii. (20 分)**  
在后续的作业中，编写**高效的向量化代码（vectorized code）**将非常重要——即尽量减少 `for` 循环，通过一次性处理多个样本来提升运行速度。  
作为练习，请完成以下两个方法的实现：

	- **`compute_distance_one_loop`**：仅使用**一个** `for` 循环来计算 L2 距离（即**部分向量化**版本）。  
	- **`compute_distance_no_loops`**：完全不使用循环来计算 L2 距离（即**完全向量化**版本）。

	>**提示：**  利用以下等式可以避免显式的循环计算：
	$$
	\|x - y\|_2^2 = \|x\|_2^2 + \|y\|_2^2 - 2x^T y
	$$


- **iii. (15 分)**  
完成方法 **`predict_labels`** 的实现，用于为每一张测试图像找到其 **k 个最近邻样本（k nearest neighbors）**，并根据这些邻居的标签来预测测试图像的类别。

	>**提示：**  你可以使用 `np.argsort` 函数来获取距离矩阵中每一行（即每个测试样本对应的所有训练样本距离）的**从小到大排序索引**。  



**(b)**  运行后续的代码单元，以便检查你在上面完成的实现。你将使用 `KNearestNeighbor` 来对测试图像进行预测，并计算这些预测的**准确率**。  我们已经为 `k = 1` 和 `k = 3` 实现了评估代码。对于 **k = 1**，测试集准确率预计约为 **29%**（上下有小幅波动属正常）。


**(c)（15 分）**  在验证集上用 **网格搜索（grid search）** 寻找最优的 `k`：对每个候选 `k`，用验证集计算准确率，选出最高者；在下方提供的单元中**报告**最高准确率和对应的 `k`。随后，请运行我们提供的代码，用最优 `k` 在**测试集**上计算准确率，并查看最近邻的**可视化结果**。


## 挑战部分（50分）

### Linear classifier with Multinomial Logistic (Softmax) Loss

在本题中，我们将使用 **Softmax*（公式 (2)）配合**随机梯度下降（SGD）**来训练一个用于图像分类（见图 1）的**线性分类器**。

**(a)（25 分）**

**损失与梯度估计**  

请按照规范，使用我们提供的公式，完成 `softmax_loss_naive` 函数及其**梯度**的实现。请注意，我们是在一个包含 **N** 张图像的**小批量（minibatch）**上计算损失。输入为 $$(x_1, y_1), (x_2, y_2), \ldots, (x_N, y_N)$$，其中 $$x_i$$ 表示批次中的第 $$i$$ 张图像，$$y_i$$ 为其对应的标签。


我们首先为每个目标类别计算其得分（score），即图像属于某一特定类别的**未归一化概率**。  
对于**单张图像**，我们将其各类别的得分表示为 $$ s_1, s_2, \ldots, s_C $$，其中 $$ C $$ 为类别总数。  
这些得分可通过下式计算：

$$
\mathbf{s} = W \mathbf{x}_i
$$

单张图像的 **Softmax 损失** $$ L_i $$ 定义为：

$$
L_i(\mathbf{s}, y) = -\log \frac{e^{s_{y_i}}}{\sum_{j=1}^{C} e^{s_j}}
\tag{1}
$$


整个小批量（minibatch）的总损失 $$\mathcal{L}$$ 可以通过对所有样本的单个损失取平均值来计算：



$$
\mathcal{L}(W) = \frac{1}{N} \sum_{i=1}^{N} L_i
\tag{2}
$$


**注意：**  
在 Softmax 层中对较大的数值进行指数运算时，结果可能会非常大，从而导致数值溢出（即出现 `inf`）。  
为避免这种数值问题，可以在进行指数运算前，**先从每个样本的得分中减去该样本得分的最大值**，如下所示：

$$
L_i = -\log \frac{e^{s_{y_i} - \max_k(s_k)}}{\sum_{j=1}^{C} e^{s_j - \max_k(s_k)}}
\tag{3}
$$


**梯度（Gradients）**  

我们提供了关于梯度的公式 $$ \frac{\partial L}{\partial W} $$，该结果也需要由 `softmax_loss_naive` 返回：


$$
\frac{\partial L_i}{\partial W_{y_i}} =
\left(
\frac{e^{s_{y_i} - \max_k(s_k)}}{\sum_{j=1}^{C} e^{s_j - \max_k(s_k)}} - 1
\right) x_i
\tag{4}
$$

$$
\frac{\partial L_i}{\partial W_j} =
\left(
\frac{e^{s_j - \max_k(s_k)}}{\sum_{j=1}^{C} e^{s_j - \max_k(s_k)}}
\right) x_i,
\quad j \neq y_i
\tag{5}
$$

如笔记本中所述，在实现完这些公式后，请运行指定的代码单元以进行 **loss 检查** 和 **梯度检查（gradient check）**，确保结果与预期一致。


**(b)（25 分）**  
对于笔记本中定义的 **`LinearClassifier`** 类，请完成以下部分的实现：

- **i. 随机梯度下降（Stochastic Gradient Descent）**  

	阅读方法 `train` 的函数头，并按照提示补全相应代码部分，用于从训练数据中**随机抽取样本**以形成小批量输入（mini-batch），并使用**梯度下降（gradient descent）**执行参数更新。  

	（注意：损失函数和梯度的计算部分已由我们提供，无需重新实现。）

- **ii. 运行分类器（Running the classifier）**  

	类似地，请编写代码实现 `predict` 方法，该方法应返回由线性分类器预测得到的类别结果。


**(c)**  
请运行我们提供的剩余代码，其中使用 **`LinearClassifier`** 在数据集的训练集部分进行训练，并计算训练集和验证集的准确率。  
同时观察测试集上的准确率，结果应大约为 **38%**。

**(d)**  
最后，请查看模型学习到的分类器的**可视化结果**。  
在这些可视化中，我们将分类器的权重视为图像进行绘制。  
通过这些图像，你可能会观察到各分类器在分配权重方式上呈现出一些**有趣的模式**。



# 任务清单

本节旨在帮助你理清并跟踪需要完成的各项任务：  


- [ ] PS 3 基础部分 **KNN**:
  - [ ] (a)-i - {{ code }} **(15分)** KNN的L2距离计算
  - [ ] (a)-ii - {{ code }} **(20分)** KNN的代码向量化
  - [ ] (a)-iii - {{ code }} **(15分)** KNN预测部分
  - [ ] (c) - {{ code }} **(15分)** 数据集的交叉验证

  <!-- - [ ] 4 - {{ code }} **(25分)** 噪声消除 -->


# 提交清单

学习通里提交文件清单（请不要提交打包zip文件）:
- [ ] `你的学号.ipynb`
- [ ] `你的学号.pdf`

