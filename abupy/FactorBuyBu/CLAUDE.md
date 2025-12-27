[根目录](../../CLAUDE.md) > **FactorBuyBu**

---

# FactorBuyBu - 买入因子模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

FactorBuyBu 定义所有买入择时因子策略的基类和具体实现。每个买入因子必须继承 `AbuFactorBuyBase` 并混入一个方向类（BuyCallMixin 或 BuyPutMixin），明确表示买入方向。

## 入口与启动

### 主入口
```python
from abupy.FactorBuyBu import AbuFactorBuyBreak, AbuFactorBuyWD
from abupy.FactorBuyBu import AbuDoubleMaBuy, AbuUpDownTrend

# 使用买入因子
buy_factors = [
    {'xd': 60, 'class': AbuFactorBuyBreak},
    {'xd': 42, 'class': AbuFactorBuyBreak}
]
```

## 对外接口

### 基类

```python
class AbuFactorBuyBase(AbuParamBase):
    """买入因子基类"""

    def __init__(self, capital, kl_pd, combine_kl_pd, benchmark, **kwargs):
        """
        :param capital: 资金类 AbuCapital 实例
        :param kl_pd: 择时时段金融时间序列
        :param combine_kl_pd: 合并历史数据的时间序列
        :param benchmark: 交易基准 AbuBenchmark 实例
        """
        self.kl_pd = kl_pd
        self.capital = capital
        self.benchmark = benchmark

    # 抽象方法，子类必须实现
    def fit_day(self, today):
        """
        择时买入因子每天都触发一次
        :param today: 今天的数据行
        :return: 如果买入返回 order，否则返回 None
        """
        pass

    def fit_first_buy(self, kl_pd):
        """
        只在择时周期内触发一次
        :param kl_pd: 择时时段金融时间序列
        :return: 如果买入返回 order，否则返回 None
        """
        pass
```

### 方向混入类

```python
class BuyCallMixin:
    """买涨混入类，期望价格上涨"""
    buy_type_str = 'call'
    expect_direction = 1.0

class BuyPutMixin:
    """买跌混入类，期望价格下跌"""
    buy_type_str = 'put'
    expect_direction = -1.0
```

### 内置买入因子

```python
# 1. 突破策略
class AbuFactorBuyBreak(AbuFactorBuyBase, BuyCallMixin):
    """价格向上突破 xd 日均线买入"""
    def __init__(self, capital, kl_pd, combine_kl_pd, benchmark, xd=60):
        self.xd = xd  # 均线周期

# 2. 横盘突破
class AbuFactorBuyWD(AbuFactorBuyBase, BuyCallMixin):
    """一定周期内价格波动小于阈值后买入"""

# 3. 双均线
class AbuDoubleMaBuy(AbuFactorBuyBase, BuyCallMixin):
    """短均线向上突破长均线买入"""

# 4. 趋势策略
class AbuUpDownTrend(AbuFactorBuyBase, BuyCallMixin):
    """向上趋势买入"""

class AbuDownUpTrend(AbuFactorBuyBase, BuyCallMixin):
    """下跌转折向上买入"""

class AbuUpDownGolden(AbuFactorBuyBase, BuyCallMixin):
    """金叉买入"""

# 5. 演示因子
class AbuSDBreak(AbuFactorBuyBase, BuyCallMixin):
    """标准差突破"""
```

## 关键依赖与配置

### 依赖模块
- `CoreBu` - 基础类
- `TradeBu` - 订单、资金
- `BetaBu` - 仓位管理
- `SlippageBu` - 滑点
- `MarketBu` - 市场数据

### 因子参数配置

```python
# 在 run_loop_back 中配置
buy_factors = [
    {
        'xd': 60,                    # 均线周期
        'class': AbuFactorBuyBreak   # 因子类
    },
    {
        'xd': 42,
        'class': AbuFactorBuyBreak
    }
]
```

## 数据模型

### 订单创建流程

```python
# 1. fit_day() 或 fit_first_buy() 被调用
# 2. 判断买入条件
# 3. 计算买入数量（通过仓位管理）
# 4. 计算买入价格（通过滑点）
# 5. 创建 AbuOrder 对象
order = AbuOrder(
    symbol_id=symbol,
    buy_date=today.date,
    buy_price=buy_price,
    buy_cnt=buy_cnt,
    buy_factor=self.__class__.__name__
)
```

### 买入特征混入

```python
class BuyFeatureMixin:
    """买入特征混入，为机器学习提供特征"""
```

## 测试与质量

### 使用示例

```python
# 教程 1: 择时策略的开发
# 教程 4: 多支股票择时回测与仓位管理
```

## 常见问题 (FAQ)

### Q: 如何自定义买入因子？
```python
from abupy.FactorBuyBu import AbuFactorBuyBase, BuyCallMixin

class MyBuyFactor(AbuFactorBuyBase, BuyCallMixin):
    def fit_day(self, today):
        # 自定义买入逻辑
        if self.kl_pd.close[-1] > self.kl_pd.close[-2]:
            return self.make_buy_order(today)
        return None
```

### Q: 什么是 xd 参数？
A: xd 表示均线周期，如 xd=60 表示 60 日均线

### Q: 如何同时使用多个买入因子？
```python
buy_factors = [
    {'xd': 60, 'class': AbuFactorBuyBreak},
    {'xd': 42, 'class': AbuFactorBuyBreak},
    {'class': AbuDoubleMaBuy}
]
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuFactorBuyBase.py` | 买入因子基类 |
| `ABuFactorBuyBreak.py` | 突破策略 |
| `ABuFactorBuyWD.py` | 横盘突破 |
| `ABuFactorBuyDM.py` | 双均线策略 |
| `ABuFactorBuyTrend.py` | 趋势策略 |
| `ABuFactorBuyDemo.py` | 演示因子 |
| `ABuBuyFactorWrap.py` | 因子封装 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 FactorBuyBu 模块文档
- 识别 7 个核心文件
- 文档覆盖率：100%
