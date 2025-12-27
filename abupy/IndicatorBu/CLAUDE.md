[根目录](../../CLAUDE.md) > **IndicatorBu**

---

# IndicatorBu - 技术指标模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

IndicatorBu 提供常用技术指标的计算，包括 MA、MACD、RSI、BOLL、ATR 等。

## 入口与启动

### 主入口
```python
from abupy.IndicatorBu import nd

# 计算指标
ma20 = nd.calc_ma(kl_df, ma_period=20)
macd = nd.calc_macd(kl_df)
rsi = nd.calc_rsi(kl_df)
atr = nd.calc_atr(kl_df)
```

## 对外接口

### 核心函数

```python
# 均线
def calc_ma(kl_df, ma_period=20, ma_type=0):
    """
    计算均线
    :param kl_df: K线数据
    :param ma_period: 周期
    :param ma_type: 0=EMA, 1=SMA
    :return: Series
    """

# MACD
def calc_macd(kl_df, fast_period=12, slow_period=26, signal_period=9):
    """计算 MACD"""

# RSI
def calc_rsi(kl_df, rsi_period=14):
    """计算 RSI"""

# 布林带
def calc_boll(kl_df, boll_period=20, boll_dev=3):
    """计算布林带"""

# ATR
def calc_atr(kl_df, atr_period=14):
    """计算 ATR"""
```

### 内置指标

```python
class ABuNDMa:
    """均线指标"""

class ABuNDMacd:
    """MACD 指标"""

class ABuNDRsi:
    """RSI 指标"""

class ABuNDBoll:
    """布林带指标"""

class ABuNDAtr:
    """ATR 指标"""
```

## 关键依赖与配置

### 依赖模块
- `pandas` - 数据处理
- `numpy` - 数值计算

## 数据模型

### 指标返回格式

```python
# 单线指标（如 MA）
return pd.Series(...)

# 双线指标（如 MACD）
return {
    'macd': pd.Series(...),
    'signal': pd.Series(...),
    'hist': pd.Series(...)
}

# 三线指标（如 BOLL）
return {
    'upper': pd.Series(...),
    'middle': pd.Series(...),
    'lower': pd.Series(...)
}
```

## 测试与质量

### 使用示例

```python
# 教程 13: 量化技术分析应用
```

## 常见问题 (FAQ)

### Q: 如何使用技术指标？
```python
from abupy.IndicatorBu import nd

# 计算均线
ma20 = nd.calc_ma(kl_df, ma_period=20)

# 计算 MACD
macd = nd.calc_macd(kl_df)
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuND.py` | 指标入口 |
| `ABuNDMa.py` | 均线 |
| `ABuNDMacd.py` | MACD |
| `ABuNDRsi.py` | RSI |
| `ABuNDBoll.py` | 布林带 |
| `ABuNDAtr.py` | ATR |
| `ABuNDBase.py` | 基础类 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 IndicatorBu 模块文档
- 识别 7 个核心文件
- 文档覆盖率：100%
