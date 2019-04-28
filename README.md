# Data Science Projects

<p>持续更新...</p>

从网上搜集各种数据科学项目并进行复现以及参加的比赛的项目。
项目主要包括：
* 简单的数据科学入门
* 数据分析、数据挖掘和数据可视化分析
* Kaggle经典项目
* 各种数据科学比赛（ISI, KDD, 天池）
项目里面会写上自己的学习心得，项目或者比赛的READMED会给出参考和转载的原网址。

## List
1. [Second Hand Housing Analysis](https://github.com/AlanConstantine/DataScienceProjects/tree/master/1_SecondHandHousing)(1st Week)
    1. [Feature Analysis](https://github.com/AlanConstantine/DataScienceProjects/blob/master/1_SecondHandHousing/SecondHandHousingFeaturesAnalysis.ipynb)
    2. [Feature Engineering & Data Prediction](https://github.com/AlanConstantine/DataScienceProjects/blob/master/1_SecondHandHousing/FeatureEngineeringandDataPrediction.ipynb)
2. [Basic Data Preprocessing](https://github.com/AlanConstantine/DataScienceProjects/tree/master/2_BasicDataPreprocessing)(2nd Week)
3. [TitanicMachineLearningfromDisaster](https://github.com/AlanConstantine/DataScienceProjects/tree/master/3_TitanicMachineLearningfromDisaster)(3~4th Week)
4. [ISI-WorldCup2019 (Mission 1, 第一次参加正式比赛：25/308 top8%)](https://github.com/AlanConstantine/ISI-WorldCup2019)

## Notes
### 小记
* Seaborn是基于matplotlib的Python可视化库。 它提供了一个高级界面来绘制有吸引力的统计图形。Seaborn其实是在matplotlib的基础上进行了更高级的API封装，从而使得作图更加容易，不需要经过大量的调整就能使你的图变得精致。但应强调的是，**应该把Seaborn视为matplotlib的补充，而不是替代物。**

### 特征工程
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
    4. 离散特征的连续化处理
        * one-hot encoding
        * 特征嵌入embedding：数据量过大情况下，如果要使用独热编码，则维度会爆炸，如果使用特征嵌入就维度低很多了。
        * word2vec将词转化为词向量
    5. 离散特征的离散化处理
        * one-hot encoding
        * 虚拟编码dummy coding：和独热编码类似，但是它的特点是，如果我们的特征有N个取值，它只需要N-1个新的0,1特征来代替，而独热编码会用N个新特征代替。比如一个特征的取值是高，中和低，那么我们只需要两位编码，比如只编码中和低，如果是1，0则是中，0,1则是低。0,0则是高了。目前虚拟编码使用的没有独热编码广，因此一般有需要的话还是使用独热编码比较好。
        * 原始特征是季节春夏秋冬。我们可以将其转化为淡季和旺季这样的二值特征，方便建模。当然有时候转化为三值特征或者四值特征也是可以的。
        * 分类问题的特征输出，我们一般需要用sklearn的```LabelEncoder```将其转化为0,1,2，...这样的类别标签值。
    6. 连续特征的离散化处理（对于连续特征，有时候我们也可以将其做离散化处理。这样特征变得高维稀疏，方便一些算法的处理）
        * 根据阈值进行分组，比如我们根据连续值特征的分位数，将该特征分为高，中和低三个特征。将分位数从0-0.3的设置为高，0.3-0.7的设置为中，0.7-1的设置为高。
        * 高级方法：使用GBDT。在LR+GBDT的经典模型中，就是使用GDBT来先将连续值转化为离散值。
3. 特征预处理
    1. 特征的标准化和归一化
        * z-score标准化
            1. 最常见的特征预处理方式，基本所有的线性模型在拟合的时候都会做 z-score标准化。
            2. 具体的方法是求出样本特征x的均值mean和标准差std；
            3. 然后用（x-mean)/std来代替原特征。这样特征就变成了均值为0，方差为1了。
            4. 在sklearn中，我们可以用StandardScaler来做z-score标准化。当然，如果是用pandas做数据预处理，可以在数据框里面减去均值，再除以方差，自己做z-score标准化。
        * max-min标准化
            1. 也称为离差标准化，预处理后使特征值映射到[0,1]之间。
            2. 具体的方法是求出样本特征x的最大值max和最小值min;
            3. 然后用(x-min)/(max-min)来代替原特征。
            4. 如果我们希望将数据映射到任意一个区间[a,b]，而不是[0,1]，那么也很简单。用(x-min)(b-a)/(max-min)+a来代替原特征即可。
            5. 在sklearn中，我们可以用MinMaxScaler来做max-min标准化。这种方法的问题就是如果测试集或者预测数据里的特征有小于min，或者大于max的数据，会导致max和min发生变化，需要重新计算。
            6. 实际算法中，除非**你对特征的取值区间有需求，否则max-min标准化没有z-score标准化好用。**
        * L1/L2范数标准化
        * 中心化
            * 主要是在PCA降维的时候，此时我们求出特征x的平均值mean后，用x-mean代替原特征，也就是特征的均值变成了0, 但是方差并不改变。这个很好理解，因为PCA就是依赖方差来降维的。
    2. 异常特征样本清洗
        1. **聚类**:比如我们可以用KMeans聚类将训练样本分成若干个簇，如果某一个簇里的样本数很少，而且簇质心和其他所有的簇都很远，那么这个簇里面的样本极有可能是异常特征样本了。我们可以将其从训练集过滤掉。
        2. **异常点检测方法**:主要是使用iForest或者one class SVM，使用异常点检测的机器学习算法来过滤所有的异常点。
        3. 某些筛选出来的异常样本是否真的是不需要的异常特征样本，最好找懂业务的再确认一下，防止将正常的样本过滤掉了。
    3. 处理不平衡数据：一个二分类问题，如果训练集里A类别样本占90%，B类别样本占10%。 而测试集里A类别样本占50%， B类别样本占50%， 如果不考虑类别不平衡问题，训练出来的模型对于类别B的预测准确率会很低，甚至低于50%。
        * 权重法
            * 比较简单的方法，我们可以对训练集里的每个类别加一个权重class weight。如果该类别的样本数多，那么它的权重就低，反之则权重就高。
            * 如果更细致点，我们还可以对每个样本加权重sample weight，思路和类别权重也是一样，即样本数多的类别样本权重低，反之样本权重高。sklearn中，绝大多数分类算法都有class weight和 sample weight可以使用。
        * 采样法
            * 采样法常用的也有两种思路。
                * 一种是对类别样本数多的样本做子采样, 比如训练集里A类别样本占90%，B类别样本占10%。那么我们可以对A类的样本子采样，直到子采样得到的A类样本数和B类别现有样本一致为止，这样我们就只用子采样得到的A类样本数和B类现有样本一起做训练集拟合模型。
                * 第二种思路是对类别样本数少的样本做过采样, 还是上面的例子，我们对B类别的样本做过采样，直到过采样得到的B类别样本数加上B类别原来样本一起和A类样本数一致，最后再去拟合模型。
        * 上述两种常用的采样法很简单，但是都有个问题，就是采样后改变了训练集的分布，可能导致泛化能力差。所以有的算法就通过其他方法来避免这个问题，比如SMOTE算法通过人工合成的方法来生成少类别的样本。

### 异常值检测与处理
* [Detect and exclude outliers in Pandas data frame](https://stackoverflow.com/questions/23199796/detect-and-exclude-outliers-in-pandas-data-frame)
* [【财通金工】“拾穗”多因子（五）：数据异常值处理：比较与实践](https://mp.weixin.qq.com/s/uQksjDJ3TCDWqZ-6eUR6Uw)

### 缺失值处理
![](https://pic2.zhimg.com/80/v2-33fd18869493bbe07dbf36755ee52d61_hd.jpg)


## Reference
* [特征工程之特征选择](https://www.cnblogs.com/pinard/p/9032759.html)
* [特征工程之特征表达](https://www.cnblogs.com/pinard/p/9061549.html)
* [特征工程之特征预处理](https://www.cnblogs.com/pinard/p/9093890.html)
* [【Python数据分析基础】: 数据缺失值处理](https://zhuanlan.zhihu.com/p/40775756)

#### [个人博客](https://blog.csdn.net/AlanConstantineLau)
