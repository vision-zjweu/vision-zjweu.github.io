---
layout: spec
permalink: /lab2
latex: true

title: Homework 1 – Numbers and Images
# due: 11:59 p.m. on Wednesday January 31st, 2024
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

# Lab2


### Lab说明


所有的代码/数据都位于文件夹 `numpy/` 中。  每个任务都要求你在函数（位于 `tests.py` 和 `warmup.py` 中）中填补空白，并返回函数注释中描述的值。  
在 `run.py` 和 `common.py` 中有我们的预先设置代码，你无需阅读或理解他们。  




1.  在 `tests.py` 和 `warmups.py` 中填写代码存根。  在 学习通 中附上以下命令的终端输出截图：  

   ```console
   $ python run.py --allwarmups
   $ python run.py --alltests
   ```



我必须答对所有题目吗？不需要。我们会给 **部分分**：  
- 每个 **warmup 练习** 占该题总分的 **4%**  
- 每个 **test** 占该题总分的 **6%**

---

### 测试说明

当你打开这两个文件之一时，你会看到类似下面的初始代码：

```python
def sample1(xs):
    """
    输入:
    - xs: 一个数值列表
    返回:
    列表中的第一个元素
    """
    return None
```



你应该按照下面的方式完成函数的实现：

```python
def sample1(xs):
	"""
	输入:
	- xs: 一个数值列表
	返回:
	列表的第一个元素
	"""
	return xs[0]
```

你可以通过运行测试脚本来检验你的实现：

```bash
python run.py --test w1     # 检查 warmups.py 中的 w1 练习题
python run.py --allwarmups  # 检查所有 warmup 练习题
python run.py --test t1     # 检查 tests.py 中的 t1 测试题
python run.py --alltests    # 检查所有测试题

# 检查所有 warmup 练习题；如果有任何失败，就启动 pdb 调试器
# 这样你就可以找到差异
python run.py --allwarmups --pdb
```

如果你在检查所有 warmup 练习题（或测试题），理想的输出结果应该是：

```bash
python run.py --allwarmups
Running w1
Running w2
...
Running w20
Ran warmup tests
20/20 = 100.0
```

### 热身练习题

你需要解决 `warmups.py` 中的全部 20 个热身练习题。它们都可以用**一行代码**解决。  

### 测试题目

你需要解决 `tests.py` 中的全部 20 个测试题。很多题目无法用一行代码解决。  
你**不能使用循环**来解决任何题目，不过你可能想先写一个慢的 for 循环解法，以确保你知道正确的计算方式，然后再将 for 循环解法改写成非循环解法。  
在所有题目中，唯一允许使用循环的例外是 **t10**（不过它同样可以不用循环来解决）。  

下面是一个示例：  


```python
def t4(R, X):
	"""
	Inputs:
	- R: A numpy array of shape (3, 3) giving a rotation matrix
	- X: A numpy array of shape (N, 3) giving a set of 3-dimensional vectors
	Returns:
	A numpy array Y of shape (N, 3) where Y[i] is X[i] rotated by R
	Par: 3 lines
	Instructor: 1 line
	Hint:
	1) If v is a vector, then the matrix-vector product Rv rotates the vector
	   by the matrix R.
	2) .T gives the transpose of a matrix
	"""
	return None
```

### What We Provide


对于每一道题，我们提供以下信息：

- **输入 (Inputs)**：传递给函数的参数  
- **返回值 (Returns)**：函数应当返回的结果  
- **代码行数 (Par)**：预期需要的代码行数。我们不会直接根据这一点打分，但如果你写的行数远多于这个数，可能说明有更简洁的方法可以解决。除了 t10 外，你不应该使用显式的循环。  
- **参考实现 (Instructor)**：我们的解答所用的代码行数。你能写得更简洁吗？  
- **提示 (Hints)**：对解决该问题有帮助的函数和其他小技巧。  



### Walkthroughs and Hints

### 讲解与提示（Walkthroughs and Hints）

**测试 8（Test 8）：**  
如果把轴（axes）弄错，NumPy 会尽力通过广播让计算“跑”起来，但结果会是错的。  
如果你的均值变量是 $$1 \times 1$$（标量），你可能会得到一个“整矩阵均值为 0”的结果。  
如果你的标准差变量是 $$1 \times M$$，你可能会发现“每一列”的标准差都变成 1。

**测试 9（Test 9）：**  
这种函数形式在数据处理里随处可见。本题主要练习把多层嵌套的计算逐步写出来。  
建议把表达式拆成多行：每行实现一部分，并在每一步都检查张量/数组的形状（size/shape）是否正确。

**测试 10（Test 10）：**  
这是一次“处理奇怪数据格式”的练习。你可以先用 `for` 循环写出正确版本再优化。

1. 先写一个循环来计算向量 `C[i]`，它是 `Xs[i]` 中数据的“质心（centroid）”。为弄清含义，大可尝试不同操作并观察哪种得到期望的形状。这里需要明确质心是 **M 维** 的，以说明我们如何理解 `Xs[i]`：行表示样本向量，列表示维度；因此质心（即向量的平均）应当沿着**列方向**取平均（也就是说，按列求均值、对行求平均）。
2. 分配一个矩阵，用来存放所有成对（pairwise）距离。然后对质心的下标 i 和 j 做双重循环，计算并填入距离。

**测试 11（Test 11）：**  
给定一组向量 $$\xB_1, \ldots, \xB_N$$，其中每个向量 $$x_i \in \mathbb{R}^M$$。把这些向量按行堆叠得到矩阵 $$\XB \in \mathbb{R}^{N \times M}$$。你的目标是构造矩阵 $$\DB \in \mathbb{R}^{N \times N}$$，满足  
$$\DB_{i,j} = \|\xB_i - \xB_j\|.$$  
这里的 $$\|\xB_i - \xB_j\|$$ 是 L2 范数（欧氏长度）。给出的有用恒等式为  
$$\|\xB-\yB\|^2 = \|\xB\|^2 + \|\yB\|^2 - 2\xB^T \yB,$$  
它对任意向量对 $$\xB,\yB$$ 都成立，可用来高效计算距离。

在每一步中，你的目标是把“慢但正确”的代码逐步替换为“快且正确”的代码。一旦代码在某一步出错，你就能迅速定位到 bug 所在的步骤。


1. 首先，写一个正确但较慢的解法，使用两层 `for` 循环。在内层循环中，代入给定恒等式，令  
   $$\DB_{i,j} = \|\xB_i\|^2 + \|\xB_j\|^2 - 2 \xB_i^T \xB_j$$。  
   请将其分成三个独立的部分来实现。

2. 接下来，写一个版本，先计算一个包含所有点积的矩阵  
   $$\PB \in \mathbb{R}^{N \times N}$$，其中 $$\PB_{i,j} = \xB_i^T \xB_j$$。  
   这可以通过一次矩阵-矩阵运算完成。然后你可以使用  
   $$\DB_{i,j} = \|\xB_i\|^2 + \|\xB_j\|^2 - 2\PB_{i,j}$$ 来计算距离。

3. 最后，计算一个包含所有范数的矩阵  
   $$\NB \in \mathbb{R}^{N \times N}$$，其中 $$\NB_{i,j} = \|\xB_i\|^2 + \|\xB_j\|^2$$。  
   你可以分两步完成：首先，计算矩阵 $$\XB$$ 每一行的平方范数，即对每行的元素平方求和。设该数组为 $$\SB$$，即  
   $$\SB_{i} = \|\xB_i\|^2, \quad \SB \in \mathbb{R}^{N}$$。  
   但要注意结果的形状（shape）。如果你计算 $$\SB + \SB^T$$，就会得到 $$\NB$$。  
   现在可以在双重循环中通过  
   $$\DB_{i,j} = \NB_{i,j} - 2 \PB_{i,j}$$ 来计算距离。

4. 此时，双重循环已无必要。你可以直接对两个矩阵逐元素相加/缩放，得到结果。

---

**测试 18（Test 18）：**  
这里的任务是画一个圆：即找出矩阵中所有位于指定行 $$y$$ 和列 $$x$$ 的 **r 个单元格范围**内的元素。  
这不是一个特别有挑战性的任务，但它能帮助你练习编写（和调试）能够正确处理行列逻辑的代码。

1. 首先，写一个正确但较慢的解法，使用两层 `for` 循环。在内层循环中，插入对给定 i, j 的正确判断条件。确保测试通过，并注意行和列的区别。

2. 简单查看一下 `np.meshgrid` 的文档。然后调用它，参数设为 `np.arange(3)` 和 `np.arange(5)`。看看是否能创建两个数组，使得 `IndexI[i,j] = i` 且 `IndexJ[i,j] = j`。

3. 将你原来用两层 `for` 循环实现的判断条件，替换成直接使用 `IndexI` 和 `IndexJ` 的方式。






# 提交清单

In the `zip` file you submit to Canvas, the directory named after your uniqname should include the following files:
- [ ] `warmups.py`
- [ ] `tests.py`
