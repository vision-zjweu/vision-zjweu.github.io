---
layout: spec
permalink: /hw1
latex: true

title: Homework 1 – Numbers and Images
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

# Homework 1 – Numbers and Images

<div class="primer-spec-callout warning" markdown="1">
   
   **Important:** Changes to rubric and submission format announced on Piazza [@58](https://piazza.com/class/lr0rfd6e5dm5wf/post/58){:target="_blank"} .

</div>

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

   {{ report }} -
   <span class="report">We have indicated questions where you have to do something in the report in green. **Some coding questions also need to be included in the report.**</span>

   The write-up must be an electronic version. **No handwriting, including plotting questions.** $$\LaTeX$$ is recommended but not mandatory.

   For including code, **do not use screenshots**. Generate a PDF using a [tool like this](https://www.i2pdf.com/source-code-to-pdf){:target="_blank"} or using this [Overleaf LaTeX template](https://www.overleaf.com/read/wbpyympmgfkf#bac472){:target="_blank"}. If this PDF contains only code, be sure to append it to the end of your report and match the questions carefully on Gradescope.

### Python Environment

Consider referring to the [Python standard library docs](https://docs.python.org/3.7/library/index.html){:target="_blank"} when you have questions about Python utilties.

We recommend you install the latest [Anaconda](https://www.anaconda.com/download/){:target="_blank"} for Python 3.12. This is a Python package manager that includes most of the modules you need for this course. We will make use of the following packages extensively in this course:

- [Numpy](https://numpy.org/doc/stable/user/quickstart.html){:target="_blank"}
- [Matplotlib](https://matplotlib.org/stable/tutorials/introductory/pyplot.html){:target="_blank"}
- [OpenCV](https://opencv.org/){:target="_blank"}

## Overview

In this assignment, you’ll work through three tasks that help set you up for success in the class as well as a short assignment involving playing with color. The assignment has three goals.

1. **Show you bugs in a low-stakes setting**. You’ll encounter a lot of programming mistakes in the course and we want to show you common bugs early on. Here, the programming problems are deliberately easy!
2. **Learn to write reasonably good Python and NumPy code**. Having layers of nested `for` loops will cause bugs and is not feasible for us to debug, use NumPy effectively! If you have not had any experience with NumPy, read this [tutorial](http://cs231n.github.io/python-numpy-tutorial/){:target="_blank"} before starting.

The assignment has four parts and corresponding folders in the starter code:

- Numpy Intro (folder `numpy/`)
- Data Interpretation and Visualization (folder `visualize/`)
- Lights on a Budget (folder `dither/`)
- Colorspaces

## Numpy Intro

All the code/data for this is located in the folder `numpy/`. Each assignment requires you to fill in the blank in a function (in `tests.py` and `warmup.py`) and return the value described in the comment for the function. There’s driver code you do not need to read in `run.py` and `common.py`.

**Note**: All the `python` below refer to `python3`. As we stated earlier, we are going to use Python 3.12 in this assignment. Python 2 was [sunset](https://www.python.org/doc/sunset-python-2/){:target="_blank"} on January 1, 2022.

1. *(40 points)* {{ report }} <span class="report">Fill in the code stubs in tests.py and warmups.py. Put the terminal output in your pdf from</span>:

```console
$ python run.py --allwarmups
$ python run.py --alltests
```

**Do I have to get every question right?** We give partial credit: each warmup exercise is worth 2% of the total grade for this question and each test is worth 3% of the total grade for this question.

### Tests Explained

When you open one of these two files, you will see starter code that looks like this:

```python
def sample1(xs):
	"""
	Inputs:
	- xs: A list of values
	Returns:
	The first entry of the list
	"""
	return None
```

You should fill in the implementation of the function, like this:

```python
def sample1(xs):
	"""
	Inputs:
	- xs: A list of values
	Returns:
	The first entry of the list
	"""
	return xs[0]
```

You can test your implementation by running the test script:

```bash
python run.py --test w1     # Check warmup problem w1 from warmups.py
python run.py --allwarmups  # Check all the warmup problems
python run.py --test t1     # Check the test problem t1 from tests.py
python run.py --alltests    # Check all the test problems

# Check all the warmup problems; if any of them fail, then launch the pdb
# debugger so you can find the difference
python run.py --allwarmups --pdb
```

If you are checking all the warmup problems (or test problems), the perfect result will be:

```bash
python run.py --allwarmups
Running w1
Running w2
...
Running w20
Ran warmup tests
20/20 = 100.0
```

### Warmup Problems

You need to solve all 20 of the warmup problems in `warmups.py`. They are all solvable with one line of code.

### Test Problems

You need to solve all 20 problems in `tests.py`. Many are not solvable in one line. You may not use a loop to solve any of the problems, although you may want to first figure out a slow for-loop solution to make sure you know what the right computation is, before changing the for-loop solution to a non for-loop solution. The one exception to the no-loop rule is t10 (although this can also be solved without loops).

Here is one example:

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


# Canvas Submission Checklist

In the `zip` file you submit to Canvas, the directory named after your uniqname should include the following files:
- [ ] `warmups.py`
- [ ] `tests.py`
- [ ] `dither.py`
- [ ] `mystery_visualize.py`
