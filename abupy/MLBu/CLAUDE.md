[根目录](../../CLAUDE.md) > **MLBu**

---

# MLBu - 机器学习模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

MLBu 提供机器学习功能的封装，支持分类、回归、无监督学习等多种学习器，以及特征工程、交叉验证、参数搜索等功能。

## 入口与启动

### 主入口
```python
from abupy.MLBu import AbuML, ml

# 快速使用
ml.fit(x, y, classifier='svm')
ml.predict(x)
```

## 对外接口

### 核心类

```python
class AbuML:
    """机器学习主类"""

    def fit(self, x, y, classifier=None, **kwargs):
        """
        训练模型
        :param x: 特征数据
        :param y: 标签数据
        :param classifier: 学习器类型
            - 'auto': 自动选择（默认）
            - 'reg': 回归
            - 'clf': 分类
            - 'hmm': 隐马尔可夫
            - 'pca': 主成分分析
            - 'kmean': K均值聚类
        :return: self
        """

    def predict(self, x):
        """预测"""

    def score(self, x, y):
        """评分"""
```

### 学习器类型

```python
class EMLFitType(Enum):
    E_FIT_AUTO = 'auto'      # 自动选择
    E_FIT_REG = 'reg'        # 回归
    E_FIT_CLF = 'clf'        # 分类
    E_FIT_HMM = 'hmm'        # 隐马尔可夫
    E_FIT_PCA = 'pca'        # 主成分分析
    E_FIT_KMEAN = 'kmean'    # K均值聚类
```

### 评分类型

```python
class _EMLScoreType(Enum):
    E_SCORE_ACCURACY = 'accuracy'    # 准确率
    E_SCORE_MSE = 'mse'              # 均方误差
    E_SCORE_ROC_AUC = 'roc_auc'      # ROC AUC
```

## 关键依赖与配置

### 依赖模块
- `scikit-learn` - 机器学习库
- `numpy` - 数值计算
- `pandas` - 数据处理

### 支持的学习器

```python
# 分类
'svm'       # 支持向量机
'decision'  # 决策树
'forest'    # 随机森林
'adaboost'  # AdaBoost
'gbdt'      # 梯度提升
'xgboost'   # XGBoost（需安装）
'lightgbm'  # LightGBM（需安装）
'knn'       # K近邻
'nb'        # 朴素贝叶斯
'logistic'  # 逻辑回归

# 回归
'svr'       # 支持向量回归
'decision'  # 决策树回归
'forest'    # 随机森林回归
'lasso'     # Lasso
'ridge'     # Ridge
'linear'    # 线性回归
'xgboost'   # XGBoost 回归
```

## 数据模型

### 特征工程

```python
class AbuMLPd:
    """Pandas 数据处理"""

    def fit(self, df, y_col=None):
        """
        对 DataFrame 进行特征工程
        :param df: 输入数据
        :param y_col: 目标列名
        """
```

### 网格搜索

```python
class AbuMLGrid:
    """网格参数搜索"""

    def fit(self, x, y, params):
        """
        :param params: 参数网格
        """
```

## 测试与质量

### 使用示例

```python
# 教程 12: 机器学习与比特币示例
# 教程 15: 量化交易和搜索引擎
```

### 测试数据

```python
ML_TEST_FILE = '/abupy/RomDataBu/ml_test.csv'
```

## 常见问题 (FAQ)

### Q: 如何快速使用？
```python
from abupy.MLBu import ml

# 分类
ml.fit(x_train, y_train, classifier='svm')
y_pred = ml.predict(x_test)

# 回归
ml.fit(x_train, y_train, classifier='svr')
```

### Q: 如何使用网格搜索？
```python
from abupy.MLBu import AbuMLGrid

grid = AbuMLGrid()
params = {'C': [0.1, 1, 10], 'gamma': [0.001, 0.01]}
grid.fit(x, y, params)
```

### Q: 如何进行交叉验证？
```python
from abupy.MLBu import AbuML

ml = AbuML()
ml.fit(x, y)
ml.cross_val_score(n_folds=5)
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuML.py` | 机器学习主类 |
| `ABuMLApi.py` | API 封装 |
| `ABuMLPd.py` | Pandas 处理 |
| `ABuMLCreater.py` | 学习器创建 |
| `ABuMLExecute.py` | 执行器 |
| `ABuMLGrid.py` | 网格搜索 |
| `ABuMLBinsCs.py` | 分箱 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 MLBu 模块文档
- 识别 7 个核心文件
- 文档覆盖率：100%
