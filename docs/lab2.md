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

For each problem, we provide:

- **Inputs**: The arguments that are provided to the function
- **Returns**: What you are supposed to return from the function
- **Par**: How many lines of code it should take. We don’t grade on this, but if it takes more lines than this, there is probably a better way to solve it. Except for t10, you should not use any explicit loops.
- **Instructor**: How many lines our solution takes. Can you do better? Hints: Functions and other tips you might find useful for this problem.

### Walkthroughs and Hints

**Test 8:** If you get the axes wrong, numpy will do its best to make sure that the computations go through but the answer will be wrong. If your mean variable is $$1 \times 1$$, you may find yourself with a matrix where the full matrix has mean zero. If your standard deviation variable is $$1 \times M$$, you may find each column has standard deviation one.

**Test 9:** This sort of functional form appears throughout data-processing. This is primarily an exercise in writing out code with multiple nested levels of calculations. Write each part of the expression one line at a time, checking the sizes at each step.

**Test 10:** This is an exercise in handling weird data formats. You may want to do this with for loops first.

1. First, make a loop that calculates the vector `C[i]` that is the centroid of the data in `Xs[i]`. To figure out the meaning of things, there is no shame in trying operations and seeing which produce the right shape. Here we have to specify that the centroid is M-dimensions to point out how we want `Xs[i]` interpreted. The centroid (or average of the vectors) has to be calculated with the average going down the columns (i.e., rows are individual vectors and columns are dimensions).
2. Allocate a matrix that can store all the pairwise distances. Then, double for loop over the centroids i and j and produce the distances.

**Test 11:**
You are given a set of vectors $$\xB_1, \ldots, \xB_N$$ where each vector $$x_i \in \mathbb{R}^M$$. These vectors are stacked together to create a matrix $$\XB \in \mathbb{R}^{N \times M}$$. Your goal is to create a matrix $$\DB \in \mathbb{R}^{N \times N}$$ such that $$\DB_{i,j} = \|\xB_i - \xB_j\|$$. Note that $$\|\xB_i - \xB_j\|$$ is the L2-norm or the Euclidean length of the vector. The useful identity you are given is that $$\|\xB-\yB\|^2 = \|\xB\|^2 + \|\yB\|^2 - 2\xB^T \yB$$. This is true for any pair of vectors $$\xB$$ and $$\yB$$ and can be used to calculate the distance quickly.

At each step, your goal is to replace slow but correct code with fast and correct code. If the code breaks at a particular step, you know where the bug is.

1. First, write a correct but slow solution that uses two for loops. In the inner body, you should plug in the given identity to make $$\DB_{i,j} = \|\xB_i\|^2 + \|\xB_j\|^2 - 2 \xB_i^T \xB_j$$. Do this in three separate terms.
2. Next, write a version that first computes a matrix that contains all the dot products, or $$\PB \in \mathbb{R}^{N \times N}$$ such that $$\PB_{i,j} = \xB_i^T \xB_j$$. This can be done in a single matrix-matrix operation. You can then calculate the distance by doing $$\DB_{i,j} = \|\xB_i\|^2 + \|\xB_j\|^2 - 2\PB_{i,j}$$.
3. Finally, calculate a matrix containing the norms, or a $$\NB \in \mathbb{R}^{N \times N}$$ such that $$\NB_{i,j} = \|\xB_i\|^2 + \|\xB_j\|^2$$. You can do this in two stages: first, calculate the squared norm of each row of $$\XB$$ by summing the squared values across the row. Suppose $$\SB$$ is this array (i.e., $$\SB_{i} = \|\xB_i\|^2$$ and $$\SB_{i} \in \mathbb{R}^{N}$$), but be careful that you look at the _shape_ that you get as output. If you compute $$\SB + \SB^T$$, you should get $$\NB$$. Now you can calculate the distance inside the double for loop as $$\DB_{i,j} = \NB_{i,j} - 2 \PB_{i,j}$$.
4. The double for loop is now not very useful. You can just add/scale the two arrays together elementwise.

**Test 18:** Here you draw a circle by finding all entries in a matrix that are within $$r$$ cells
of a row $$y$$ and column $$x$$. This is a not particularly intellectually stimulating exercise, but it is practice in writing (and debugging) code that reasons about rows and columns.

1. First, write a correct but slow solution that uses two for loops. In the inner body, plug in the correct test for the given i, j. Make sure the
   test passes; be careful about rows and columns.
2. Look at the documentation for `np.meshgrid` briefly. Then call it with `np.arange(3)` and `np.arange(5)` as
   arguments. See if you can create two arrays such that `IndexI[i,j] = i` and `IndexJ[i,j] = j`.
3. Replace your test that uses two for loops with something that just uses `IndexI` and `IndexJ`.



# Tasks Checklist

This section is meant to help you keep track of the many tasks you have to complete:

- [ ] **NumPy Intro**:
  - [ ] 1.1 - {{ report }} Terminal Output
- [ ] **Data Interpretation and Visualization**:
  - [ ] 2.1 - {{ report }} 2 images from `mysterydata2.npy`
  - [ ] 2.2 - {{ report }} 2 images from `mysterydata3.npy`
  - [ ] 2.3 - {{ code }}{{ report }} `colorMapArray`
  - [ ] 2.4 - {{ report }} 9 images from `mysterydata4.npy`
- [ ] **Lights on a Budget**:
	- [ ] 3.1 Naive Approach
		- [ ] 1 - {{ code }} `quantize`
		- [ ] 2 - {{ code }}{{ report }} `quantizeNaive`
		- [ ] 3 - {{ report }} Quantize Runtime
		- [ ] 4 - {{ report }} Intensity Values vs Palette Values
		- [ ] 5 - {{ report }} Two input/output pairs: `aep.jpg` + your choice
	- [ ] 3.2 Floyd-Steinberg
		- [ ] 1 - {{ code }} `quantizeFloyd`
		- [ ] 2 - {{ report }} Why does dithering work?
		- [ ] 3 - {{ report }} 3 results from `gallery/` including `aep.jpg`
	- [ ] 3.3 Resizing Images
		- [ ] 1 - {{ code }}{{ report }} `resizeToSquare`
	- [ ] 3.4 Handling Color
		- [ ] 1 - {{ code }}{{ report }} `quantize` (scalar and vector)
		- [ ] 2 - {{ code }}{{ report }} `quantizeFloyd` (multi-channel)
		- [ ] 3 - {{ report }} 4 results
	- [ ] 3.5 Gamma Correction (*optional*)
- [ ] **Colorspaces**:
	- [ ] 4.1 - {{ code }}{{ report }} R,G,B plots
	- [ ] 4.2 - {{ code }}{{ report }} L,A,B plots
	- [ ] 4.3 - {{ report }} RGB vs LAB
	- [ ] 4.4 - {{ report }} Two images and their Luminance plots

# Canvas Submission Checklist

In the `zip` file you submit to Canvas, the directory named after your uniqname should include the following files:
- [ ] `warmups.py`
- [ ] `tests.py`
- [ ] `dither.py`
- [ ] `mystery_visualize.py`
