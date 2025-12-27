[根目录](../../CLAUDE.md) > **BetaBu**

---

# BetaBu - 仓位管理模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

BetaBu 负责交易中的仓位管理，即决定每次交易买入多少数量的股票。系统提供多种仓位管理策略，包括基于 ATR、凯利公式、固定比例等。

## 入口与启动

### 主入口
```python
from abupy.BetaBu import (
    AbuPositionBase,
    AbuAtrPosition,      # ATR 仓位管理（默认）
    AbuKellyPosition,    # 凯利公式仓位管理
    AbuPtPosition        # 固定比例仓位管理
)
```

## 对外接口

### 基类

```python
class AbuPositionBase:
    """仓位管理基类"""

    def __init__(self, capital, kl_pd_manager):
        """
        :param capital: 资金实例
        :param kl_pd_manager: K线管理器
        """
        self.capital = capital
        self.kl_pd_manager = kl_pd_manager

    def _init_position(self, factor, kl_pd):
        """
        计算买入数量
        :param factor: 买入因子实例
        :param kl_pd: K线数据
        :return: 买入数量
        """
        pass
```

### 内置仓位管理策略

```python
# 1. ATR 仓位管理（默认）
class AbuAtrPosition(AbuPositionBase):
    """
    基于 ATR 的动态仓位管理
    - 市场波动大时减少仓位
    - 市场波动小时增加仓位
    - 默认策略
    """

    def _init_position(self, factor, kl_pd):
        """
        买入数量 = (账户资金 * position_rate) / (atr * 3)
        """
        # position_rate 默认 0.8
        # atr * 3 是 3 倍 ATR 作为风险单位


# 2. 凯利公式仓位管理
class AbuKellyPosition(AbuPositionBase):
    """
    基于凯利公式的仓位管理
    - 需要历史胜率数据
    - 适合有统计数据的策略
    """
    def _init_position(self, factor, kl_pd):
        """
        f = (bp - q) / b
        f: 投资比例
        b: 盈亏比
        p: 胜率
        q: 败率 (1-p)
        """


# 3. 固定比例仓位管理
class AbuPtPosition(AbuPositionBase):
    """
    固定比例仓位管理
    - 每次使用固定比例的资金
    - 简单稳定
    """
    def _init_position(self, factor, kl_pd):
        """
        买入数量 = (账户资金 * position_rate) / 买入价格
        """
```

## 关键依赖与配置

### 依赖模块
- `CoreBu` - 基础类
- `TradeBu` - 资金管理
- `IndicatorBu` - ATR 指标

### 配置参数

```python
# 在买入因子中配置
buy_factors = [
    {
        'xd': 60,
        'class': AbuFactorBuyBreak,
        'position': AbuAtrPosition  # 指定仓位管理策略
    }
]

# 或者修改全局默认
from abupy.BetaBu import AbuAtrPosition
AbuAtrPosition.position_rate = 0.9  # 修改仓位比例
```

## 数据模型

### 仓位计算流程

```python
# 1. 获取当前可用资金
cash = capital.current_cash

# 2. 根据策略计算风险单位
if isinstance(position, AbuAtrPosition):
    # 使用 ATR 计算风险
    atr = calculate_atr(kl_pd)
    risk_unit = atr * 3
elif isinstance(position, AbuKellyPosition):
    # 使用凯利公式
    risk_unit = kelly_formula(win_rate, profit_loss_ratio)
else:
    # 使用固定比例
    risk_unit = price

# 3. 计算买入数量
buy_cnt = (cash * position_rate) / risk_unit
```

## 测试与质量

### 使用示例

```python
# 教程 4: 多支股票择时回测与仓位管理
# 教程 31: 资金仓位管理与买入策略的搭配
```

### 策略对比

| 策略 | 适用场景 | 优点 | 缺点 |
|------|---------|------|------|
| AtrPosition | 通用 | 动态调整风险 | 需要计算 ATR |
| KellyPosition | 已知胜率 | 数学最优 | 依赖历史数据 |
| PtPosition | 简单策略 | 稳定可控 | 不能动态调整 |

## 常见问题 (FAQ)

### Q: 如何选择仓位管理策略？
- **默认使用 AbuAtrPosition**: 动态调整，适合大多数情况
- **有胜率数据**: 使用 AbuKellyPosition
- **简单策略**: 使用 AbuPtPosition

### Q: 如何调整仓位比例？
```python
from abupy.BetaBu import AbuAtrPosition
AbuAtrPosition.position_rate = 0.9  # 使用 90% 资金
```

### Q: 什么是凯利公式？
A: 凯利公式计算最优投资比例：
```
f = (bp - q) / b
- f: 投资比例
- b: 盈亏比（平均盈利/平均亏损）
- p: 胜率
- q: 败率 = 1 - p
```

### Q: 如何自定义仓位管理？
```python
from abupy.BetaBu import AbuPositionBase

class MyPosition(AbuPositionBase):
    def _init_position(self, factor, kl_pd):
        # 自定义仓位计算逻辑
        cash = self.capital.current_cash
        price = kl_pd.close[-1]
        return int(cash * 0.5 / price)
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuPositionBase.py` | 仓位管理基类 |
| `ABuAtrPosition.py` | ATR 仓位管理 |
| `ABuKellyPosition.py` | 凯利公式仓位管理 |
| `ABuPtPosition.py` | 固定比例仓位管理 |
| `ABuBeta.py` | Beta 模块封装 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 BetaBu 模块文档
- 识别 5 个核心文件
- 文档覆盖率：100%
