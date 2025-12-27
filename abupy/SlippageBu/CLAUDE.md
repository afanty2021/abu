[根目录](../../CLAUDE.md) > **SlippageBu**

---

# SlippageBu - 滑点模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

SlippageBu 模拟真实交易中的滑点，即订单价格与实际成交价格之间的差异。

## 入口与启动

### 主入口
```python
from abupy.SlippageBu import (
    AbuSlippageBuyMean,     # 买入滑点
    AbuSlippageSellMean,    # 卖出滑点
    slippage                # 模块封装
)
```

## 对外接口

### 买入滑点

```python
class AbuSlippageBuyMean:
    """均值买入滑点"""

    def fit_price(self, kl_pd, factor):
        """
        计算买入价格
        :param kl_df: K线数据
        :param factor: 买入因子
        :return: 买入价格
        """
        # 默认使用当天收盘价
        return kl_pd.close[-1]
```

### 卖出滑点

```python
class AbuSlippageSellMean:
    """均值卖出滑点"""

    def fit_price(self, kl_pd, factor):
        """计算卖出价格"""
        # 默认使用当天收盘价
        return kl_pd.close[-1]
```

## 关键依赖与配置

### 依赖模块
- `CoreBu` - 基础类
- `MarketBu` - 市场数据

## 常见问题 (FAQ)

### Q: 如何自定义滑点？
```python
from abupy.SlippageBu import AbuSlippageBuyMean

class MySlippageBuy(AbuSlippageBuyMean):
    def fit_price(self, kl_pd, factor):
        # 自定义滑点计算
        return kl_pd.close[-1] * 1.001  # 加 0.1% 滑点
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuSlippageBuyBase.py` | 买入滑点基类 |
| `ABuSlippageBuyMean.py` | 均值买入滑点 |
| `ABuSlippageSellBase.py` | 卖出滑点基类 |
| `ABuSlippageSellMean.py` | 均值卖出滑点 |
| `ABuSlippage.py` | 模块封装 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 SlippageBu 模块文档
- 识别 5 个核心文件
- 文档覆盖率：100%
