# PU learning 

## article 
- https://zhuanlan.zhihu.com/p/42435966

### 1、直接利用标准分类方法
- 最经常使用的PU learning方法或许是这样的：将正样本和未标记样本分别看作是positive samples和negative samples, 然后利用这些数据训练一个标准分类器。分类器将为每个物品打一个分数（概率值）。通常正样本分数高于负样本的分数。因此对于那些未标记的物品，分数较高的最有可能为positive。
这种朴素的方法在文献Learning classifiers from only positive and unlabeled data 中有介绍。该论文的核心结果是，在某些基本假设下（虽然对于现实生活目的而言可能稍微不合理),合理利用正例和未贴标签数据进行训练得到的标准分类器应该能够给出与实际正确分数成正比的分数。
正如作者们所说, “This means that if the [hypothetically properly trained] classifier is only used to rank examples according to the chance that they belong to [the positives], then the classifier [trained on positive and unlabeled data] can be used directly instead.”

### 2、PU bagging
- 一个更加复杂的方法来解决PU问题是采用bagging的变种：
a)、通过将所有正样本和未标记样本进行随机组合来创建训练集。
b)、利用这个“bootstrap”样本来构建分类器，分别将正样本和未标记样本视为positive和negative。
c)、将分类器应用于不在训练集中的未标记样本 - OOB（“out of bag”）- 并记录其分数。
d)、重复上述三个步骤，最后为每个样本的分数为OOB分数的平均值。
描述这种方法的一篇论文是 A bagging SVM to learn from positive and unlabeled examples 。 这篇文章的作者说, “the method can match and even outperform the performance of state-of-the-art methods for PU learning, particularly when the number of positive examples is limited and the fraction of negatives among the unlabeled examples is small. The proposed method can also run considerably faster than state-of-the-art methods, particularly when the set of unlabeled examples is large.”

### 3、Two-step approaches
- 大部分的PU learning策略属于 “two-step approaches”。最近的一篇介绍这些方法的论文是 An Evaluation of Two-Step Techniques for Positive-Unlabeled Learning in Text Classification 。
以下是这种方法的“两个步骤”：
a)、识别可以百分之百标记为negative的未标记样本子集（上述论文的作者称这些样本为“reliable negatives”。）
b)、使用正负样本来训练标准分类器并将其应用于剩余的未标记样本。
通常，会将第二步的结果返回到第一步并重复上述步骤。 正如以上作者所述,“If the [reliable negative] set contains mostly negative documents and is sufficiently large, a learning algorithm… works very well and will be able to build a good classifier. But often a very small set of negative documents identified in step 1… then a learning algorithm iteratively runs till it converges or some stopping criterion is met.”
一个相似的方法在博客Introduction to Pseudo-Labelling : A Semi-Supervised learning technique中给出，但是它具体不是针对PU问题来设计的。

### 4、Positive unlabeled random forest
- 这里值得一提的关于PU learning的最新一个发展是文献 Towards Positive Unlabeled Learning for Parallel Data Mining: A Random Forest Framework 中提出的一种算法。
根据作者所述，“The proposed framework, termed PURF (Positive Unlabeled Random Forest), is able to learn from positive and unlabeled instances and achieve comparable classification performance with RF trained by fully labeled data through parallel computing according to experiments on both synthetic and real-world UCI datasets… This framework combines PU learning techniques including widely used PU information gain (PURF-IG) and newly developed PU Gini index (PURF-GI) with an extendable parallel computing algorithm (i.e. RF).”
更重要地是，作者提到他们已经“implemented PURF with Python based on the scikit-learn package,” 因此，如果代码能够开源的话，这将会是一个令人期待的工具。