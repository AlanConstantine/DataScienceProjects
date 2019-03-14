# Data Science Projects

<p>持续更新...</p>

***One week, One project!***

从网上搜集各种数据科学项目并进行复现。
项目主要包括：
* 简单的数据科学入门
* 数据分析、数据挖掘和数据可视化分析
* Kaggle经典项目
项目里面会写上自己的学习心得，项目的READMED会给出参考和转载的原网址。

### List
1. [Second Hand Housing Analysis](https://github.com/AlanConstantine/DataScienceProjects/tree/master/1_SecondHandHousing)(1st Week)
    1. [Feature Analysis](https://github.com/AlanConstantine/DataScienceProjects/blob/master/1_SecondHandHousing/SecondHandHousingFeaturesAnalysis.ipynb)
    2. [Feature Engineering & Data Prediction](https://github.com/AlanConstantine/DataScienceProjects/blob/master/1_SecondHandHousing/FeatureEngineeringandDataPrediction.ipynb)
2. [Basic Data Preprocessing](https://github.com/AlanConstantine/DataScienceProjects/tree/master/2_BasicDataPreprocessing)(2nd Week)

### Notes
* Seaborn是基于matplotlib的Python可视化库。 它提供了一个高级界面来绘制有吸引力的统计图形。Seaborn其实是在matplotlib的基础上进行了更高级的API封装，从而使得作图更加容易，不需要经过大量的调整就能使你的图变得精致。但应强调的是，**应该把Seaborn视为matplotlib的补充，而不是替代物。**
* 特征工程
    1. 特征选择
        1. 行业知识，领域专家
        2. 统计学方法：方差筛选，方差越大，认为特征越有用；越小，那特征对算法可能无用。sklearn中的```VarianceThreshold```类可以很方便的完成这个工作。
        3. 特征选择方法：
            1. 过滤法：
                * 方差筛选；
                * 相关系数；
                * 假设检验
                    1. 卡方检验：可以检验某个特征分布和输出值分布之间的相关性，sklearn中，可以使用```chi2```这个类来做卡方检验得到所有特征的卡方值与显著性水平P临界值，我们可以给定卡方值阈值， 选择卡方值较大的部分特征。
                    2. F检验：在sklearn中，有F检验的函数```f_classif```和```f_regression```，分别在分类和回归特征选择时使用。
                    3. t检验
                * 互信息：从信息熵的角度分析各个特征和输出值之间的关系评分。
                    1. 互信息值越大，说明该特征和输出值之间的相关性越大，越需要保留。
                    2. 在sklearn中，可以使用```mutual_info_classif```(分类)和```mutual_info_regression```(回归)来计算各个输入特征和输出值之间的互信息。
                * **没有什么思路的时候，可以优先使用卡方检验和互信息来做特征选择**
            2. 包装法：根据目标函数，通常是**预测效果评分，每次选择部分特征，或者排除部分特征**
                * 递归消除特征法(recursive feature elimination，RFE)：递归消除特征法使用一个机器学习模型来进行多轮训练，每轮训练后，消除若干权值系数的对应的特征，再基于新的特征集进行下一轮训练。在sklearn中，可以使用```RFE```函数来选择特征。
            3. 嵌入法：
                1. 先使用某些机器学习的算法和模型进行训练，得到各个特征的权值系数；
                2. 根据权值系数从大到小来选择特征。
                3. 类似于过滤法，但是它是通过机器学习训练来确定特征的优劣，而不是直接从特征的一些统计学指标来确定特征的优劣。
                4. 它和RFE的区别是它不是通过不停的筛掉特征来进行训练，而是使用的都是特征全集。在sklearn中，使用SelectFromModel函数来选择特征。
        4. 寻找高级特征
            1. 若干项特征加和： 我们假设你希望根据每日销售额得到一周销售额的特征。你可以将最近的7天的销售额相加得到。
            2. 若干项特征之差： 假设你已经拥有每周销售额以及每月销售额两项特征，可以求一周前一月内的销售额。
            3. 若干项特征乘积： 假设你有商品价格和商品销量的特征，那么就可以得到销售额的特征。
            4. 若干项特征除商： 假设你有每个用户的销售额和购买的商品件数，那么就是得到该用户平均每件商品的销售额。
    2. 特征表达
        1. 缺失值处理
            1. 连续值
                * 选择所有有该特征值的样本，然后取**平均值**，来填充缺失值
                * 取**中位数**来填充缺失值
            2. 离散值
                * 选择所有有该特征值的样本中**最频繁**出现的类别值，来填充缺失值。
                3. 在sklearn中，可以使用```preprocessing.Imputer```来选择这三种不同的处理逻辑做预处理。
            3. 特殊的特征处理
                * 时间
                    1. 连续的时间差值法：即计算出所有样本的时间到某一个未来时间之间的数值差距，这样这个差距是UTC的时间差，从而将时间特征转化为连续值
                    2. 根据时间所在的年，月，日，星期几，小时数，将一个时间特征转化为若干个离散特征，这种方法在分析具有**明显时间趋势**的问题比较好用
                    3. 权重法：根据时间的新旧得到一个权重值。比如对于商品，三个月前购买的设置一个较低的权重，最近三天购买的设置一个中等的权重，在三个月内但是三天前的设置一个较大的权重
                * 地理
                    1. 转化为多个离散特征，比如城市名特征，区县特征，街道特征等。
                    2. 如果需要判断**用户分布区域**，则一般处理成连续值会比较好，这时可以将地址处理成**经度**和**纬度**的连续特征。



### Reference
* [特征工程之特征选择](https://www.cnblogs.com/pinard/p/9032759.html)
* [特征工程之特征表达](https://www.cnblogs.com/pinard/p/9061549.html)
* [特征工程之特征预处理](https://www.cnblogs.com/pinard/p/9093890.html)

#### [个人博客](https://blog.csdn.net/AlanConstantineLau)
