[根目录](../../CLAUDE.md) > **MetricsBu**

---

# MetricsBu - 度量评估模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

MetricsBu 负责回测结果的度量评估，包括收益率、夏普比率、最大回撤、胜率等多种指标的计算和可视化。

## 入口与启动

### 主入口
```python
from abupy.MetricsBu import (
    AbuMetricsBase,         # 基础度量类
    AbuMetricsFutures,      # 期货度量
    AbuMetricsTC,           # 数字货币度量
    metrics,                # 模块封装
    make_scorer             # 自定义评分
)

# 快速度量
metrics = AbuMetricsBase(
    orders_pd, action_pd, capital, benchmark
)
metrics.fit_metrics()
metrics.plot_returns_cmp()
```

## 对外接口

### 核心类

```python
class AbuMetricsBase:
    """基础度量类（股票）"""

    def __init__(self, orders_pd, action_pd, capital, benchmark):
        """
        :param orders_pd: 交易订单 DataFrame
        :param action_pd: 交易行为 DataFrame
        :param capital: 资金实例
        :param benchmark: 基准实例
        """

    def fit_metrics(self):
        """计算度量指标"""

    def plot_returns_cmp(self, only_info=False):
        """收益对比图"""

    def plot_sharp_volatility_cmp(self, only_info=False):
        """夏普比率与波动率对比"""


class AbuMetricsFutures(AbuMetricsBase):
    """期货度量类"""


class AbuMetricsTC(AbuMetricsBase):
    """数字货币度量类"""
```

### 核心指标

```python
# 收益指标
return_cum       # 累计收益率
return_year      # 年化收益率

# 风险指标
max_drawdown     # 最大回撤
volatility       # 波动率
sharpe_ratio     # 夏普比率

# 交易指标
win_rate         # 胜率
profit_loss_ratio  # 盈亏比
trade_cnt        # 交易次数
avg_hold_days    # 平均持有天数
```

### 网格搜索

```python
class GridSearch:
    """网格参数搜索"""

    def __init__(self, buy_factors, sell_factors):
        """
        :param buy_factors: 买入因子列表
        :param sell_factors: 卖出因子列表
        """

    def fit(self, n_folds=2, n_jobs=16):
        """
        执行网格搜索
        :param n_folds: 回测年数
        :param n_jobs: 并行任务数
        """

    def best_score(self):
        """最佳评分"""

    def best_params(self):
        """最佳参数"""
```

### 交叉验证

```python
class AbuCrossVal:
    """交叉验证"""

    @staticmethod
    def cross_val(orders_pd, action_pd, capital, benchmark, n_folds=5):
        """
        K-Fold 交叉验证
        :param n_folds: 折数
        :return: 度量结果列表
        """
```

## 关键依赖与配置

### 依赖模块
- `pandas` - 数据处理
- `numpy` - 数值计算
- `matplotlib` - 可视化
- `seaborn` - 高级可视化
- `empyrical` - 金融度量（通过 ExtBu）

### 度量配置

```python
# 评分器
class WrsmScorer(AbuBaseScorer):
    """
    加权风险收益评分器
    - 结合收益、风险、胜率
    """
```

## 数据模型

### 度量结果

```python
class MetricsDemo:
    """度量结果示例"""
    return_cum       # 累计收益率
    return_year      # 年化收益率
    max_drawdown     # 最大回撤
    sharpe_ratio     # 夏普比率
    win_rate         # 胜率
    profit_loss_ratio  # 盈亏比
```

### 评分元组

```python
class AbuScoreTuple(namedtuple):
    """评分元组"""
    score           # 综合评分
    return_cum      # 累计收益
    return_year     # 年化收益
    max_drawdown    # 最大回撤
    sharpe_ratio    # 夏普比率
```

## 测试与质量

### 使用示例

```python
# 教程 6: 回测结果的度量
# 教程 7: 寻找策略最优参数和评分
```

## 常见问题 (FAQ)

### Q: 如何快速度量？
```python
from abupy.MetricsBu import AbuMetricsBase

AbuMetricsBase.show_general(
    orders_pd, action_pd, capital, benchmark
)
```

### Q: 如何自定义评分？
```python
from abupy.MetricsBu import make_scorer

def my_scorer(metrics):
    return metrics.return_year / metrics.max_drawdown

scorer = make_scorer(my_scorer)
```

### Q: 如何进行参数搜索？
```python
from abupy.MetricsBu import GridSearch

buy_factors = [{'xd': 60, 'class': AbuFactorBuyBreak}]
gs = GridSearch(buy_factors, sell_factors)
gs.fit(n_folds=2, n_jobs=8)
print(gs.best_score(), gs.best_params())
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuMetricsBase.py` | 基础度量类 |
| `ABuMetricsFutures.py` | 期货度量 |
| `ABuMetricsTC.py` | 数字货币度量 |
| `ABuMetricsScore.py` | 评分器 |
| `ABuMetrics.py` | 度量辅助函数 |
| `ABuGridSearch.py` | 网格搜索 |
| `ABuCrossVal.py` | 交叉验证 |
| `ABuGridHelper.py` | 网格搜索辅助 |
| `metrics` | 模块封装 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 MetricsBu 模块文档
- 识别 9 个核心文件
- 文档覆盖率：100%
