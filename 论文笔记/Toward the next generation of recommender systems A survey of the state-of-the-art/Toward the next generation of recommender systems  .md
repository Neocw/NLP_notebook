# <推荐系统论文阅读系列>1. 一篇经典的综述
简介：第一篇为Adomavicius G, Tuzhilin A. **Toward the next generation of recommender systems: A survey of the state-of-the-art and possible extensions**[J]. Knowledge and Data Engineering, IEEE Transactions on, 2005, 17(6): 734-749.。

这是一篇综述类的论文，被称为推荐系统最经典的综述论文之一。虽然是比较老的一篇文章，里面提到的对推荐系统的展望有很多在现在已经被研究了，但仍是入门推荐系统必读的一篇经典论文。本文主要包含两部分：对当时技术的总结和对推荐系统发展的看法。

## 一、The survey of Recommender Systems
### 1.content-based 基于内容的推荐系统
#### 1) 要点记录：
* content-based:The user will be recommended items similar to the ones the user preferred in the past.

* 主要应用于文本项目，因为内容通常用关键字描述

* 文中特别介绍了一下TF-IDF(Term Frequency-Inverse Document Frequency)：词频-逆文档频率（主要思想是评估词对于文档的重要程度，当一个词在该文档中出现次数越多越重要，同时它在其他越多的文档中出现，对于该文档重要性会降低）：
$$
T F_{i, j}=\frac{f_{i, j}}{\max _{z} f_{z, j}}
$$
$$
I D F_{i}=\log \frac{N}{n_{i}}
$$
$$
w_{i, j}=T F_{i, j} \times I D F_{i}
$$
* 效用函数：
$$
u(c, s)=\text { score(ContentBasedProfile }(c), \text { Content }(s))
$$
* 常用余弦相似度计算内容间相关性（基于信息检索的启发式方法）：
$$
\begin{aligned}
u(c, s) &=\cos \left(\vec{w}_{c}, \vec{w}_{s}\right)=\frac{\vec{w}_{c} \cdot \vec{w}_{s}}{\left\|\vec{w}_{c}\right\|_{2} \times\left\|\vec{w}_{s}\right\|_{2}} \\
&=\frac{\sum_{i=1}^{K} w_{i, c} w_{i, s}}{\sqrt{\sum_{i=1}^{K} w_{i, c}^{2}} \sqrt{\sum_{i=1}^{K} w_{i, s}^{2}}}
\end{aligned}
$$

* 其他基于内容的方法：**statistical learning and machine learning**：clustering, decision trees, and artificial neural networks...
#### 2）缺点：
###### a. Limited Content Analysis
为了有足够的特征集，内容必须是可以由计算机自动解析的形式（例如，文本），或者应该手动地将特征分配给物品。但是一些领域难以自动提取特征，由于资源限制手动分配特征不切实际。
内容分析不合理，两个不相同的物品可能拥有同样的内容。因此，由于基于文本的文档通常由最重要的关键字表示，因此基于内容的系统无法区分写得好的文章和写得不好的文章，如果它们碰巧使用相同的术语。
###### b. Overspecialization
难以推荐到用户完全没接触过的物品。引入随机性，来推荐完全没接触过的物品。
好的推荐系统不仅需要过滤掉完全不相同的物品，也需要过滤掉基本一样的物品
###### c. New User Problem
评分太少的新用户无法分析其喜欢物品的内容

### 2. Collaborative Methods 基于协同过滤的推荐系统
#### 2) 要点记录：
* Collaborative：The user will be recommended items that people with similar tastes and preferences liked in the past
* algorithms for collaborative recom-mendations can be grouped into two general classes:memory-based(orheuristic-based) and model-base
##### memory-based algorithms 
* a、聚合函数 
$$
r_{c, s}=\operatorname{aggr} r_{c^{\prime}, s}
$$
where^CCdenotes the set of N users that are the most similar to user c and who have rated items( N can range anywhere from  1  to  the  number  of  all  users).  Some  examples  of  the aggregation function are:
$$
(a) r_{c, s}=\frac{1}{N} \sum_{c^{\prime} \in \hat{C}} r_{c^{\prime}, s}
$$
$$
(b) r_{c, s}=k \sum_{c^{\prime} \in C} \operatorname{sim}\left(c, c^{\prime}\right) \times r_{c^{\prime}, s}
$$
$$
(c) r_{c, s}=\bar{r}_{c}+k \sum_{c^{\prime} \in C} \operatorname{sim}\left(c, c^{\prime}\right) \times\left(r_{c^{\prime}, s}-\bar{r}_{c^{\prime}}\right)
$$

where  multiplier k serves  as  a  normalizing  factor  and  is usually  selected as 
$ k=1 / \sum_{c^{\prime} \in \hat{C}}\left|\operatorname{sim}\left(c, c^{\prime}\right)\right|$,  and  where  the average rating of user c ，$\bar{r}_{c}$, in (10c) is defined as：
$$
\bar{r}_{c}=\left(1 /\left|S_{c}\right|\right) \sum_{s \in S_{c}} r_{c, s}, \text { where } S_{c}=\left\{s \in S \mid r_{c, s} \neq \oslash\right\}
$$

* b. 计算x，y用户相似度
Inthe   correlation-based   approach,  the   Pearson   correlationcoefficient is used to measure the similarity ：
$$
\operatorname{sim}(x, y)=\frac{\sum_{s \in S_{x y}}\left(r_{x, s}-\bar{r}_{x}\right)\left(r_{y, s}-\bar{r}_{y}\right)}{\sqrt{\sum_{s \in S_{x y}}\left(r_{x, s}-\bar{r}_{x}\right)^{2} \sum_{s \in S_{x y}}\left(r_{y, s}-\bar{r}_{y}\right)^{2}}}
$$
cosine-based approach：
$$
\operatorname{sim}(x, y)=\cos (\vec{x}, \vec{y})=\frac{\vec{x} \cdot \vec{y}}{\|\vec{x}\|_{2} \times\|\vec{y}\|_{2}}=\frac{\sum_{s \in S_{x y}} r_{x, s} r_{y, s}}{\sqrt{\sum_{s \in S_{x y}} r_{x, s}^{2}} \sqrt{\sum_{s \in S_{x y}} r_{y, s}^{2}}},
$$
*  c. 一般提前计算好相似度矩阵，因为短期内不会变化。当用户请求推荐时，用预先计算好的相似度矩阵计算。

#####   model-basedalgorithms  
贝叶斯网络，SVD，隐语义模型等
$$
r_{c, s}=E\left(r_{c, s}\right)=\sum_{i=0}^{n} i \times \operatorname{Pr}\left(r_{c, s}=i \mid r_{c, s^{\prime}}, s^{\prime} \in S_{c}\right)
$$
#### 缺点：
* New User Problem用户冷启动
* New Item Problem物品冷启动
* Sparsity数据稀疏

### 3. hybrid recommendation 混合推荐系统
hybrid recommendation: These methods combine collaborative and content-based methods.

1.分别做协同过滤和基于内容的推荐，结合它们预测的结果
* 评分线性组合
* 推荐投票方案
* 在不同时刻使用不同方法的推荐结果

2.将一些基于内容的特征加入协同过滤的方法：使用内容计算用户相似性，有利于克服纯粹协同过滤的稀疏性问题

3.将一些协同过滤的特征加入基于内容的方法：主要做降维

4.基于内容和协同过滤构建一个统一的模型

5.通过基于知识的方法进行增强

### 4. 对不同推荐系统的分类

![1611222996(1)](\1611222996(1).png)

## 二、Extending Capabilities of Recommender Systems
1. 全面了解用户和物品

2. 多维性，不仅限于用户物品二维，考虑时间地点上下文等。

3. 多标准评分

4. 非侵入式
	* 隐式/显式
	* 主动学习

5. 灵活性

6. 设计新的实验和指标保证推荐系统等有效性

7. 其他，如可解释性，可信度，隐私等

## 三、GitHub链接
[我的GitHub主页](https://github.com/Neocw/NLP_notebook)