# Stacking
- 简述
	- 主要的三类集成学习方法为Bagging、Boosting和Stacking。目前，大型的数据挖掘比赛（如Kaggle），排名靠前的基本上都是集成机器学习模型或者深度神经网络。
	- Stacking的经典图
		- ![](https://img-blog.csdnimg.cn/20190412163905109.png)
		- 将训练好的所有基模型对整个训练集进行预测，第j个基模型对第i个训练样本的预测值将作为新的训练集中第i个样本的第j个特征值，最后基于新的训练集进行训练。同理，预测的过程也要先经过所有基模型的预测形成新的测试集，最后再对测试集进行预测。
	- 具体原理讲解参考这位的博客
		- [博客地址](https://blog.csdn.net/wstcjf/article/details/77989963)
	- 简单来说，集成学习其实都是将基本模型组合形成更优秀的模型，Stacking也不例外。
	- stacking是得到各个算法训练全样本的结果再用一个元算法融合这些结果，它可以选择使用网格搜索和交叉验证。
- Mlxtend框架
	- 众所周知，如今传统机器学习领域的库基本上被sciket-learn（sklearn)占领，若你没有使用过sklearn库，那就不能称为使用过机器学习算法进行数据挖掘。但是，**自定义集成学习库依然没有什么太过主流的**，sklearn也只是实现了一些比较主流的集成学习方法如随机森林（RF）、AdaBoost等。当然，这也是因为bagging和boosting可以直接调用而stacking需要自己设计。
	- Mlxtend完美兼容sklearn，可以使用sklearn的模型进行组合生成新模型。它同时集成了stacking分类和回归模型以及它们的交叉验证的版本。
	- 由于已经有很多stacking的分类介绍，本例以回归为例讲讲stacking的回归实现。
- Mlxtend安装
	- 使用pip安装即可
		- `pip install mlxtend`
	- 官方文档
		- [地址](http://rasbt.github.io/mlxtend/)
- stacking回归
	- stacking回归是一种通过元回归器（meta-regressor）组合多个回归模型（lr，svr等）的集成学习技术。而且，每个基回归模型（就是上述的多个回归模型）在训练时都要**使用完整训练集**，集成学习过程中每个基回归模型的输出作为元特征成为元回归器的输入，元回归器通过拟合这些元特征来组合多个模型。
- 案例
	- 使用StackingRegressor
		- 简单使用stacking模型预测波士顿房价（使用经典波士顿房价数据集）
			- 由于大数据集需要调参，这里简单使用100条数据进行回归测试。
			- 代码
				- code
			- 运行结果
				- ![](https://img-blog.csdnimg.cn/20190412182324699.png)
			- 可以看到stacking模型的一般预测准确率是高于所有基模型的。
		- 对stacking模型网格搜索调参
			- 仍然使用上一个案例的模型
			- 代码及结果
				- ![](https://img-blog.csdnimg.cn/20190412184336245.png)
			- 显然mlxtend模型依然支持网格搜索调参
	- 使用StackingCVRegressor
		- 简介
			- mlxtend.regressor中的StackingCVRegressor是一种集成学习元回归器。
			- StackingCVRegressor扩展了标准Stacking算法（在mlxtend中的实现为StackingRegressor）。
			- 在标准Stacking算法中，拟合一级回归器的时候，我们如果使用与第二级回归器的输入的同一个训练集，这很可能会导致过拟合。 然而，StackingCVRegressor使用了"非折叠预测"的概念:数据集被分成k个折叠，并且在k个连续的循环中，使用k-1折来拟合第一级回归器，其实也就是k折交叉验证的StackingRegressor。
			- 在K轮中每一轮中，一级回归器被应用于在每次迭代中还未用于模型拟合的剩余1个子集。然后将得到的预测叠加起来并作为输入数据提供给二级回归器。
			- 在StackingCVRegressor的训练完成之后，一级回归器拟合整个数据集以获得最佳预测。
		- 波士顿房价预测
			- 代码
				- code
			- 运行结果
				- ![](https://img-blog.csdnimg.cn/20190412192135693.png))
			- 可以看到，对比第一次使用StackingRegressor模型的损失降低了。（尽管由于调参问题，评分没有基回归器高）
		- 网格搜索
			- 使用上面案例
			- 代码及结果
				- ![](https://img-blog.csdnimg.cn/20190412192249653.png)
- 补充说明
	- 主要介绍了框架Mlxtend的使用 ，具体的API函数见上面提到的官方文档。
	- 毫不夸张的说，Stacking可以说是大型数据挖掘比赛的利器。
	- 具体代码见我的Github，欢迎Star或者Fork（环境为Jupyter）