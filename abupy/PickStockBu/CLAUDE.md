[根目录](../../CLAUDE.md) > **PickStockBu**

---

# PickStockBu - 选股策略模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

PickStockBu 负责选股策略的实现，从全市场或备选股票池中筛选出符合条件的股票。

## 入口与启动

### 主入口
```python
from abupy.PickStockBu import (
    AbuPickStockBase,           # 选股基类
    AbuPickStockPriceMinMax,    # 价格范围选股
    AbuPickRegressAngMinMax,    # 回归角度选股
    AbuPickStockNTop,           # N 支股票选股
    AbuPickSimilarNTop,         # 相似度选股
    AbuPickStockShiftDistance,  # 偏移距离选股
    ps                          # 模块封装
)

# 使用选股
stock_picks = [
    {'class': AbuPickStockPriceMinMax,
     'threshold_price_min': 50.0, 'reversed': False}
]
```

## 对外接口

### 基类

```python
class AbuPickStockBase(AbuParamBase):
    """选股因子基类"""

    def fit_pick(self, kl_pd, target_symbol):
        """
        :param kl_pd: K 线数据
        :param target_symbol: 目标股票
        :return: True(选中) / False(不选)
        """
        pass
```

### 内置选股因子

```python
# 1. 价格范围选股
class AbuPickStockPriceMinMax(AbuPickStockBase):
    """
    根据价格范围选股
    :param threshold_price_min: 最低价格
    :param threshold_price_max: 最高价格
    :param reversed: 是否反向选择
    """


# 2. 回归角度选股
class AbuPickRegressAngMinMax(AbuPickStockBase):
    """
    根据线性回归角度选股
    :param threshold_ang_min: 最小角度
    :param threshold_ang_max: 最大角度
    :param reversed: 是否反向选择
    """


# 3. N 支股票选股
class AbuPickStockNTop(AbuPickStockBase):
    """
    选择排名前 N 的股票
    :param top_n: 前 N 支
    """


# 4. 相似度选股
class AbuPickSimilarNTop(AbuPickStockBase):
    """
    根据与目标股票的相似度选股
    """


# 5. 偏移距离选股
class AbuPickStockShiftDistance(AbuPickStockBase):
    """
    根据偏移距离选股
    """
```

## 关键依赖与配置

### 依赖模块
- `CoreBu` - 基础类
- `MarketBu` - 市场数据
- `SimilarBu` - 相似度计算

### 选股配置

```python
# 单个选股因子
stock_picks = [
    {'class': AbuPickStockPriceMinMax,
     'threshold_price_min': 50.0}
]

# 多个选股因子（与关系）
stock_picks = [
    {'class': AbuPickStockPriceMinMax,
     'threshold_price_min': 50.0},
    {'class': AbuPickRegressAngMinMax,
     'threshold_ang_min': 0.0}
]
```

## 数据模型

### 选股流程

```python
# 1. 获取全市场股票或备选股票池
# 2. 对每只股票执行选股因子
# 3. 所有选股因子都返回 True 才选中
# 4. 返回选中的股票列表
```

## 测试与质量

### 使用示例

```python
# 教程 5: 选股策略的开发
# 教程 27: 狗股选股策略与择时策略的配合
```

## 常见问题 (FAQ)

### Q: 如何使用选股？
```python
stock_picks = [
    {'class': AbuPickStockPriceMinMax,
     'threshold_price_min': 50.0, 'reversed': False}
]

abu_result, _ = abu.run_loop_back(
    read_cash=1000000,
    buy_factors=buy_factors,
    sell_factors=sell_factors,
    stock_picks=stock_picks
)
```

### Q: 选股和择时的关系？
- 选股：筛选股票池
- 择时：对选中的股票进行买卖时机判断

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuPickStockBase.py` | 选股基类 |
| `ABuPickStockPriceMinMax.py` | 价格范围选股 |
| `ABuPickRegressAngMinMax.py` | 回归角度选股 |
| `ABuPickStockNTop.py` | N 支股票选股 |
| `ABuPickSimilarNTop.py` | 相似度选股 |
| `ABuPickStockShiftDistance.py` | 偏移距离选股 |
| `ABuPickStockDemo.py` | 演示选股因子 |
| `ABuPickStock.py` | 模块封装 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 PickStockBu 模块文档
- 识别 8 个核心文件
- 文档覆盖率：100%
