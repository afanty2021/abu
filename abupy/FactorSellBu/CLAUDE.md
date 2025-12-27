[根目录](../../CLAUDE.md) > **FactorSellBu**

---

# FactorSellBu - 卖出因子模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

FactorSellBu 定义所有卖出择时因子策略，包括止盈、止损、风险控制等策略。卖出因子与买入因子配合使用，控制交易的风险和收益。

## 入口与启动

### 主入口
```python
from abupy.FactorSellBu import (
    AbuFactorAtrNStop,      # ATR 止盈止损
    AbuFactorPreAtrNStop,   # 预判 ATR 止损
    AbuFactorCloseAtrNStop, # 收盘 ATR 止损
    AbuFactorSellBreak,     # 突破卖出
    AbuFactorSellNDay,      # N 日卖出
    AbuDoubleMaSell         # 双均线卖出
)

# 使用卖出因子
sell_factors = [
    {'stop_loss_n': 0.5, 'stop_win_n': 3.0, 'class': AbuFactorAtrNStop},
    {'pre_atr_n': 1.0, 'class': AbuFactorPreAtrNStop}
]
```

## 对外接口

### 基类

```python
class AbuFactorSellBase(AbuParamBase):
    """卖出因子基类"""

    def __init__(self, kl_pd, capital, **kwargs):
        """
        :param kl_pd: 买入后至今的金融时间序列
        :param capital: 资金类实例
        """
        self.kl_pd = kl_pd
        self.capital = capital

    # 抽象方法，子类必须实现
    def fit_sell_factor(self, today, orders):
        """
        择时卖出因子每天都触发
        :param today: 今天的数据行
        :param orders: 持仓订单列表
        :return: 如果卖出返回 order，否则返回 None
        """
        pass
```

### 内置卖出因子

```python
# 1. ATR 止盈止损（最常用）
class AbuFactorAtrNStop(AbuFactorSellBase):
    """
    基于 ATR (Average True Range) 的止盈止损
    :param stop_loss_n: 止损倍数 (默认 1.0)
    :param stop_win_n: 止盈倍数 (默认 2.0)
    """
    def __init__(self, kl_pd, capital, stop_loss_n=1.0, stop_win_n=2.0):
        self.stop_loss_n = stop_loss_n
        self.stop_win_n = stop_win_n

# 2. 预判 ATR 止损
class AbuFactorPreAtrNStop(AbuFactorSellBase):
    """
    预判 ATR 止损，在买入前判断
    :param pre_atr_n: 预判 ATR 倍数
    """

# 3. 收盘 ATR 止损
class AbuFactorCloseAtrNStop(AbuFactorSellBase):
    """基于收盘价的 ATR 止损"""

# 4. 突破卖出
class AbuFactorSellBreak(AbuFactorSellBase):
    """价格向下突破 xd 日均线卖出"""

# 5. N 日卖出
class AbuFactorSellNDay(AbuFactorSellBase):
    """持有 N 天后强制卖出"""

# 6. 双均线卖出
class AbuDoubleMaSell(AbuFactorSellBase):
    """短均线向下跌破长均线卖出"""
```

## 关键依赖与配置

### 依赖模块
- `CoreBu` - 基础类
- `TradeBu` - 订单、资金
- `IndicatorBu` - 技术指标 (ATR)

### 因子参数配置

```python
sell_factors = [
    {
        'stop_loss_n': 0.5,      # 止损 0.5 倍 ATR
        'stop_win_n': 3.0,       # 止盈 3.0 倍 ATR
        'class': AbuFactorAtrNStop
    },
    {
        'pre_atr_n': 1.0,        # 预判 1.0 倍 ATR
        'class': AbuFactorPreAtrNStop
    },
    {
        'close_atr_n': 1.5,      # 收盘 1.5 倍 ATR
        'class': AbuFactorCloseAtrNStop
    }
]
```

## 数据模型

### 卖出类型

```python
# 卖出类型字符串
'stop_loss'    # 止损卖出
'stop_win'     # 止盈卖出
'stop_nday'    # N日卖出
'stop_normal'  # 正常卖出
'keep'         # 继续持有
```

### 支持方向枚举

```python
class ESupportDirection(Enum):
    E_SUPPORT_DOWN = 'down'      # 向下支撑
    E_SUPPORT_UP = 'up'          # 向上阻力
```

### 卖出特征混入

```python
class SellFeatureMixin:
    """卖出特征混入，为机器学习提供特征"""
```

## 测试与质量

### 使用示例

```python
# 教程 2: 择时策略的优化
# 教程 7: 寻找策略最优参数和评分
```

### 止盈止损策略对比

| 策略 | 适用场景 | 优点 | 缺点 |
|------|---------|------|------|
| AtrNStop | 通用 | 动态调整 | 可能过早止盈 |
| PreAtrNStop | 买入前判断 | 避免不良交易 | 可能错过机会 |
| CloseAtrNStop | 尾盘控制 | 减少日内波动 | 反应较慢 |
| NDay | 时间控制 | 强制平仓 | 可能错过趋势 |

## 常见问题 (FAQ)

### Q: 如何设置止盈止损？
```python
# 设置 1.0 倍 ATR 止损，3.0 倍 ATR 止盈
sell_factors = [
    {'stop_loss_n': 1.0, 'stop_win_n': 3.0, 'class': AbuFactorAtrNStop}
]
```

### Q: 什么是 ATR？
A: ATR (Average True Range) 是平均真实波幅，反映市场波动性

### Q: 如何同时使用多个卖出因子？
```python
# 多个卖出因子是"或"关系，任一触发即卖出
sell_factors = [
    {'stop_loss_n': 0.5, 'stop_win_n': 3.0, 'class': AbuFactorAtrNStop},
    {'n_day': 20, 'class': AbuFactorSellNDay}
]
```

### Q: 如何自定义卖出因子？
```python
from abupy.FactorSellBu import AbuFactorSellBase

class MySellFactor(AbuFactorSellBase):
    def fit_sell_factor(self, today, orders):
        for order in orders:
            if condition:
                return self.make_sell_order(order, today)
        return None
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuFactorSellBase.py` | 卖出因子基类 |
| `ABuFactorAtrNStop.py` | ATR 止盈止损 |
| `ABuFactorPreAtrNStop.py` | 预判 ATR 止损 |
| `ABuFactorCloseAtrNStop.py` | 收盘 ATR 止损 |
| `ABuFactorSellBreak.py` | 突破卖出 |
| `ABuFactorSellNDay.py` | N 日卖出 |
| `ABuFactorSellDM.py` | 双均线卖出 |
| `ABuFS.py` | 卖出因子辅助函数 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 FactorSellBu 模块文档
- 识别 8 个核心文件
- 文档覆盖率：100%
