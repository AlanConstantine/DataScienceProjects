### Second Hand Housing Analysis with Python
#### Reference
* [入门Python数据分析最好的实战项目（一）](https://segmentfault.com/a/1190000015440560)
* [入门Python数据分析最好的实战项目（二）](https://segmentfault.com/a/1190000015613967)

#### Notes

##### Feature Analysis
1. 处理缺失值时，可以使用**中位数/平均数**代替，或者可以**根据其他特征建立模型进行预测**
2. ```seaborn.regplot()```
    * 可以直接用来可视化出线性回归关系
3. ```seaborn.displot()```
    * [displot()](https://zhuanlan.zhihu.com/p/34354510)集合了matplotlib的hist()与核函数估计kdeplot的功能，增加了rugplot分布观测条显示与利用scipy库fit拟合参数分布的新颖用途。
4. ```seaborn.kdeplot()```
    * [核密度估计(kernel density estimation)](https://zhuanlan.zhihu.com/p/34354510)是在概率论中用来估计未知的密度函数，属于非参数检验方法之一。通过核密度估计图可以比较直观的看出数据样本本身的分布特征。具体用法如下
