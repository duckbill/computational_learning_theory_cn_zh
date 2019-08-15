### Introduction（导引）
#### 1.1 Course Overview（课程概括）
  1. Building machines that learn from experience is an important research goal of artificial intelligence, and has therefore been an active area of research. Most of the work in machine learning is empirical research. In such research, learning algorithms typically are judged by their performance on sample data sets. Although these ad hoc comparisons may provide some insight, it is difficult to compare two learning algorithms carefully and rigorously, or to understand in what situations a given algorithm might perform well, without a formally specified learning model with which the algorithms may be evaluated.
   建立从经验中学习是人工智能研究的一个很重要的研究目标，因此也是一个很活跃的研究领域。机器学习大部分工作是"实验法研究“，在这样的研究中，评判学习算法通常根据它们在有样本数据集上的性能表现。尽管这些临时的比较可能会提供一些见解，但是很难仔细和严格地比较两种学习算法，或者在没有正式指定的学习模型的情况下理解给定算法可能在什么情况下表现良好的算法。
  2. Recently, considerable research attention has been devoted to the theoretical study of machine learning. In computational learning theory one defines formal mathematical models of learning that enable rigorous analysis of both the predictive power and the computational efficiency of learning algorithms. The analysis made possible by these models provides a framework in which to design algorithms that are provably more efficient in both their use of time and data.
    最近，相当多的研究注意力集中在了机器学习的理论研究上了。在计算学习理论中，人们定义了正式的学习数学模型，该模型能够对学习算法的预测能力和计算效率进行严格地分析。通过这些模型进行的分析提供了一个框架，在该框架下可以设计出在时间和数据使用方面更有效的算法。
  3. During the first half of this course we will cover the basic results in compoutational learning theory. 
    在本课程的第一部分，我们将介绍计算学习理论的基本结果。
  This portion will include a discussion of the distribution-free（or PAC) learning model,the model of learning with queries,and the mistake-bound (or on-line) learning model.
    这部分将讨论无分布（或PAC)学习模型，学习查询模型和错误约束（或在线）学习模型。
  The primary goal is to understand how these models relate to one another and what *classes of concepts* are efficiently learnable in the various models.
    主要目标是了解这些模型如何相互关联，以及在各种模型中可以有效学习哪些*概念类*。
  Thus we will present efficient algorithms for learning  various concept classes under each model.
    因此，我们将提出有效的学习算法来学习每个模型下各种概念类。
  (And in some case we will consider what can be done if the computation time is not restricted to be polynomial.)
    （在某些情况下，如果计算时间不限于多项式，我们将考虑可以做什么。）
    >个人理解这里提到的多项式其实是多项式算法，在《算法分析与设计》中多项式算法都是好算法，一般属于NP问题。这里提到的非多项式算法应该是NP-Hard问题。
   In contrast to these positive reuslts we present hardness results for some concept classes indicating that no efficient learning algorithm exists.
    与这些正结果相比,我们给出了一些概念类的难度结，同时也表明不存在有效的学习算法
    >个人理解也就是说NP-hard没有有效的算法，这也是《算法设计与分析》里的内容。
    
    In addition to studying the basic noise-free versions of these learning models，we will also discuss various models of noise and techniques for designing algorithms that are robust against noise.
    除了研究这些学习模型的基础无噪声版本，我们还将讨论各种噪声版本以及抗噪声算法的设计。
  Finally,during the seconde half of this course we will study a selection of topics that follow up on the material persented during the first half of the course.
    最后，在课程的后半部分，我们将学习一系列主题，这些主题使用前半部分提供的资料。
    These topics were selected by the students,and are just a sample of the types of other results that have been obtained. 
    这些主题由学生选择，而且这些主题只是已经获得其他结果类型的样本。
    We warn the reader that this course only covers a small portion of the models,learning techniques,and methods for proving hardness results that are currently available in the literature.
   我们提醒读者，本课程仅提供一小部分模型，学习技术和用于目前文献中可获得难度值（hardness）结果的方法
    
#### 1.2 Intruduction（引言）
1. In this section we give a very basic overview of the area of compulational learning theroy. 
  在本节当中我们将对计算学习领域进行非常基本的概述。
   Portions of this introduction are taken from Chapter 2 of Goldman's thesis[18].
  本引言的部分内容摘自高曼（Goldman)论文第二章。
   Also see Chapter 2 of Kearn's thesis[27] for additional definitions and background material.
 另请参考Kearns的论文[27]第二章，了解其他定义和背景材料。
 
##### 1.2.1 A Research Methodology（研究方法论）
1. Before describing formal models of learning,it is useful to outline a research methodlology for applying the formalism of computational learning theory to "real-life" learning problems. 
   在正式描述学习模式之前，有必要概述一下将计算学习理论的形式应用于“现实生活学习问题”的研究方法。
   There are four steps to the methodology. 
   该方法有四个步骤。
    1. Precisely define the problem, preserving key features while simplifying as much as possible 
    精确的定位问题，保留关键特征同时尽量简化问题。
    2. Select an appropriate formal learning model
    选择一个合适的形式（formal）的学习模型。
    3. Design a learning algorithm.
    设计一个学习算法。
    4. Analyze the performance of the algorithm using the formal model.
    分析使用改形式模型的算法性能。
    >个人总结就是：定位问题->模型->算法->性能
    
 2. In Step 2, selecting an appropriate formal learning model, there are a number of questions to consider. These include:
    在第二步中，在选择一个合适形式学习模型的时候有以下一些问题需要考虑进内：
    - What is being learned? 
    要学到什么？
    - How does the learner interact with the environment (e.g, Is there a helpful teacher? An adversary?)
    学习器如何跟外界交互（比如：是否有一个有用的teacher？一个adversary？）
    - What is the prior knowledge of the learner? 
    学习器的先验知识是什么?
    - How is the learners hypothesis represented? 
     如何表示学习器假设。
    - What are the criteria for successful learning? 
    成功学到了的标准是什么？
    - How efficient is the learner in tlme, data and space?
    学习器在时间和数据空间效率上怎么样？
3. It is critical that the model chosen accurately reflect the real-life learning problem.
    最重要的是选择的模型准确地反应现实生活中的学习问题。
 
##### 1.2.2 Definitions(定义）

1.  In this coures, we consider a restricted type of learning problem called concept learning.
    在这次课程中，我们考虑一种称为概念学习的一种受限类型的学习问题。
    
    In a concept learning problem there are a set of instance and a single target concept that classifies each instance as a positive or a negative instance.
    在概念学习问题中存在一个实例集合和一个能把每一个实例分成正例和负例的一个但目标概念（single target concept)
     The instance space denotes the set of all instance that the learner may see.
     实例空间表示学习器能够看到的所有实例集合。
     >个人觉得那个instance space其实就是训练集和测试的统称
     
     The concept space denotes the set of all concepts from which the target concept can be chosen.
     概念空间表示可以被选择的目标概念的所有概念集合。
     
     The learner's goal is to devise a hypotheis of the target concept that acurately classfies each instance as positive or negative.
     学习器的目标是设计一个目标概念的假设，该假设能够精确的把每个实例分成正例和负例。
    
2. For example, one might wish to teach a child how to disginguish chairs from other furniture in a room.
   例如，人们可能希望教会孩子如何将椅子和房间里其他的家具区分开来。
    
   Each item of furniture is an instance; the chairs are positive instances and all other items are negative instance.
   每一个家具项是一个实例，椅子项是正例且所有其他项是负例。
   The goal of the learner(in this case the child) is to develop a rule for the concept of a chair. 
   学习器的目标（在这个例子中是孩子）是开发一套关于椅子概念的规则。
   >不太确认concept如何翻译，其实从上面这句话可以看得出concept其实表示是一种类别。比如做分类中一类事物。
   
   In our models of learning, one possibility is that the rules are Boolean functions of feactures of the items presented,such as has-four-legs or is-wooden. 
   在我们的模型中，一种可能性是规则是提供项特征的布尔函数，比如有四条腿或是木制。
  
 3. Since we want the complexity of the learning problem to depend on the "size" of the target concept,often we assign to it some natural size measure $n$.
   If we let $X_{n}$ denote the *instance* space, and each $x\in X$ is an *instance*. 
   For each $n < 1$,we define each $C_{n} \subseteq 2^{X_{n}}$to be a family of concepts over $X_{n}$,and $C = U_{n\geq 1}C_{n}$ to be a concept class over $X$.
   For $c\in C_{n}$ and $x\in X_{n}$, $c(x)$ denotes the classfication of $c$ on instance $x$.    
   That is,$c(x)=1$ if and only if $x\in c$. 
   We say that $x$ is a positive instance of $c$ if $c(x)=1$ and $x$ is a negative instance of $c$ if $c(x)=0$.
   Finally, a hypothesis h for $C_n$ is a rule that fiven any $x\in X_n$ outputs in polynomial time a prediction for $c(x)$.
   The *hypothesis space* is the set of all hypotheses $h$ that the learning algorithm may output.
   The hypothesis must make a prediction for each$x \in X_n$.
   For the learning models which we will study here,it is not acceptable for the hypothesis to answer "I don't konw" for some instances.
  4. To illustrate these definitions,consider the concept class of monomials.(A monomial is a conjunction of literals, where each literal is either some boolean variable or its negation.)
     For this concept class,$n$ is the number of variables.
     Thus $\left|X_n\right| = 2^n$ where each $x\in X_n$ represents an assignment of 0 or 1 to each variable.
     Observe that each variable can be palced in the target concept unnegated,placed in the target concept negated, or not put in the target concept at all.
     Thus, $\left|C_n\right|=3^n$.
     One possible target concept for the class of monomials over five variables is $x_1\overline{x_4}x_5$.
     For this target concept, the instance "10001" is a positive instance and "00001" is negative instance.
     
  5. We will model the learning process as consisting of two phases, a training phase and a performance phase.
     In the training phase the learner is presented with labeled examples(i.e.m an example is chossen from the instance space and labeled according to the target concept).
     Based on thes examples the learner must devise a hypotheis of the target concept.
     In the performance phase the hypothesis is used to classify examples from the instance space, and the accuracy of the hypothesis is evaluated.
     The various models of learning will differ primarily in the way the allow the learner to interact with the environment during the training phase and how the hypothesis is evaluated.
     
     
     ![b2ba010d74f9b3a885621de004f66a31.png](evernotecid://69AE67DD-EFDA-49A7-A5DF-915AD7BC5C89/wwwevernotecom/145401932/ENResource/p256)
     
     
     
     
     
     
   





