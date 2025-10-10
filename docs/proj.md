---
layout: spec
permalink: /proj
latex: true

title: 实训项目提案指南
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

## 实训项目提案指南

对于你的期末项目，你将更深入地研究计算机视觉的某个领域——例如将计算机视觉应用到一个对你而言重要的任务中。

你可以从下方第二部分提供的项目想法列表中选择一个，也可以提出**你自己的新课题**。我们**强烈鼓励你自主提出项目想法**——这样你会学得更多，也会更有乐趣！






- 每个小组只需提交一份项目提案。

- 你的项目提案应为 **一页 PDF 文件**，并回答以下问题：  

- 如果你希望在提交之后对项目的研究重点进行重大调整，你需要征求老师同意。

- 项目提案将占你实训期末项目成绩的 10%（请注意，整个期末项目占实训课程总成绩的 60%）。

- 期末项目的书面报告和ppt报告预计将在第17周提交

- 你的项目提案里需要包括以下内容：
    - **项目标题**：  
    你的项目名称是什么？

    - **小组成员**：  
    小组成员的姓名和学号是什么？

    - **问题陈述**：  
    你们要解决的核心问题是什么？你为什么对这个题目感兴趣？

    - **研究方法**：  
    你们打算如何解决这个问题？不需要所有细节都完全确定，但应有一个大致的实施思路。

    - **数据集**：  
    你们计划使用什么数据集？  
    项目常见的失败原因是“想法很酷，但没有合适的数据”。  
    **建议不要自己收集数据集**，因为这会大幅增加复杂度和工作量。应尽量使用现有的公开数据集。

    - **结果评估**：  
    你们将如何判断项目是否成功？  
    将使用什么指标？  
    是否有简单的基准模型(baseline model)用于比较？


## 参考项目题目

为了帮助你构思项目，我们在下方提供了一些示例想法。  
请注意，这些项目仅涵盖了你**可能尝试内容的一小部分**。  

我们**鼓励你提出自己有创意的项目想法**，可以将这些示例作为出发点！  

- **语义分割（Semantic Segmentation）**：使用深度学习实现一种**语义分割算法**，[B站参考](https://www.bilibili.com/video/BV1bC411b7Po/?spm_id_from=333.337.search-card.all.click&vd_source=c5682721378130716e842e0a8190baf4)。
- **历史照片上色（Colorizing Historical Photographs）**： 实现一种基于深度学习的**图像上色算法**，  并将其应用于**为老旧的黑白照片添加颜色**，  从而实现对历史影像的视觉复原与增强，[B站参考](https://www.bilibili.com/video/BV1eu411X7m7/?spm_id_from=333.337.search-card.all.click&vd_source=c5682721378130716e842e0a8190baf4)。
- **目标检测（Object Detection）**：开发你自己的**目标检测系统**。  例如，可以考虑设计：
    - 基于深度学习的果蔬检测识别系统,[B站参考](https://www.bilibili.com/video/BV1Ym421p7BC/?spm_id_from=333.337.search-card.all.click&vd_source=c5682721378130716e842e0a8190baf4)
    - 基于深度学习的交通标志检测识别系统,[B站参考](https://www.bilibili.com/video/BV1aF4m1L7tb/?spm_id_from=333.337.search-card.all.click&vd_source=c5682721378130716e842e0a8190baf4)
    - 姿态识别, [B站参考](https://www.bilibili.com/video/BV1zUCTYvEtW/?spm_id_from=333.337.search-card.all.click&vd_source=c5682721378130716e842e0a8190baf4)
- **图像描述生成（Image Captioning）**：实现一个**神经网络图像描述模型（neural captioning model）**，  使系统能够根据输入图像**自动生成相应的文字说明或描述性句子**。[B站参考](bilibili.com/video/BV1zt411x7pt/?spm_id_from=333.337.search-card.all.click)
- **Vit（Vision Transformer）**: 做一个使用ViT技术的应用，[B站参考](https://www.bilibili.com/video/BV1fH4y1H7mV/?spm_id_from=333.337.search-card.all.click&vd_source=c5682721378130716e842e0a8190baf4)
