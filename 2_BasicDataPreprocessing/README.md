### Basic Data Preprocessing with Python
#### Reference
1. [basic_data_preprocessing](https://github.com/bonn0062/basic_data_preprocessing)
2. [The complete beginner’s guide to data cleaning and preprocessing](https://towardsdatascience.com/the-complete-beginners-guide-to-data-cleaning-and-preprocessing-2070b7d4c6d)
3. [数据清洗&预处理入门完整指南](https://mp.weixin.qq.com/s/axQc08WIEjSOzRo3difjlg)

#### Notes
1. 数据集划分方法```iloc[:,:]```，逗号前表示行范围，后为列范围
2. 缺失值处理，利用 scikit-learn 预处理模型中的 imputer 类实现
3. 非数值型数据编码，有LabelEncoder和OneHotEncoder两种方式。
4. 测试集划分```train_test_split```
5. 特征缩放，先拟合x_train，再transform x_test，y_train测试集上进行拟合，只进行变换。