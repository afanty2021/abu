[根目录](../../CLAUDE.md) > **MarketBu**

---

# MarketBu - 市场数据模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

MarketBu 负责所有市场数据的获取、解析、缓存和管理。支持 A股、美股、港股、期货（国内外）、数字货币等多种金融市场的行情数据和股票代码管理。

## 入口与启动

### 主入口
```python
from abupy.MarketBu import ABuSymbolPd
from abupy.MarketBu import ABuSymbol

# 获取价格数据
kl_df = ABuSymbolPd.get_price('usTSLA', start='2020-01-01', end='2023-12-31')

# 代码转换
symbol = ABuSymbol.code_to_symbol('usTSLA')
```

### 数据更新
```python
from abupy import abu
# 更新全市场数据
abu.run_kl_update(n_folds=6, market=EMarketTargetType.E_MARKET_TARGET_US)
```

## 对外接口

### 核心接口

```python
# 获取价格数据（核心）
def get_price(symbol, start=None, end=None, n_folds=2):
    """
    获取金融时间序列数据
    :param symbol: 股票代码
    :param start: 开始日期
    :param end: 结束日期
    :return: DataFrame with columns: date, open, high, low, close, volume, etc.
    """

# 代码转换
def code_to_symbol(code, rs=True):
    """
    解析代码为 Symbol 对象
    支持格式: 'usTSLA', 'TSLA', 'sz000001', '600000', 'hk00700'
    :return: Symbol(market, sub_market, stock_code)
    """

# 获取所有股票代码
def all_symbol(market=None):
    """
    获取指定市场的所有股票代码
    :param market: EMarketTargetType 枚举
    :return: list of symbol strings
    """
```

### Symbol 类

```python
class Symbol:
    """股票代码统一表示类"""
    def __init__(self, market, sub_market, stock_code):
        self.market = market          # EMarketTargetType
        self.sub_market = sub_market  # EMarketSubType (交易所)
        self.stock_code = stock_code  # 股票代码

    @property
    def symbol_code(self):
        """返回完整代码，如 'usTSLA'"""

    @property
    def symbol_market(self):
        """返回市场标识"""
```

### 数据基类

```python
class BaseMarket:
    """市场数据基类"""

class StockBaseMarket(BaseMarket):
    """股票市场基类"""

class FuturesBaseMarket(BaseMarket):
    """期货市场基类"""

class TCBaseMarket(BaseMarket):
    """数字货币市场基类"""
```

## 关键依赖与配置

### 依赖模块
- `pandas` - 数据处理
- `numpy` - 数值计算
- `requests` - 网络请求（通过 ABuNetWork）
- `h5py` - HDF5 存储
- `sqlite3` - 本地缓存

### 数据源配置

```python
# 默认数据源：雪球网
# 可扩展数据源：ABuDataSource.py

# 本地缓存配置
K_LINE_CSV_NET = '/abupy/RomDataBu/csv_net'
K_LINE_HDF5_NET = '/abupy/RomDataBu/hdf5_net'
```

## 数据模型

### K线数据结构
```python
# DataFrame columns:
# - date: 交易日期 (int, 格式 YYYYMMDD)
# - open: 开盘价
# - high: 最高价
# - low: 最低价
# - close: 收盘价
# - volume: 成交量
# - fat: 涨跌额（可选）
# - fat_rad: 涨跌幅（可选）
# - key: 唯一标识
```

### 市场枚举
```python
class EMarketSubType(Enum):
    # A股
    SH = 'sh'      # 上海证券交易所
    SZ = 'sz'      # 深圳证券交易所
    # 港股
    HK = 'hk'      # 香港交易所
    # 数字货币
    COIN = 'coin'  # 币类市场
    # 期货
    CN_FUTURES = 'cn_futures'
    GB_FUTURES = 'gb_futures'
```

## 测试与质量

### 沙盒测试股票池
```python
# 美股沙盒
K_SAND_BOX_US = ['usTSLA', 'usNOAH', 'usSFUN', 'usBIDU',
                 'usAAPL', 'usGOOG', 'usWUBA', 'usVIPS']

# A股沙盒
K_SAND_BOX_CN = ['sz002230', 'sz300104', 'sz300059', 'sh601766',
                 'sh600085', 'sh600036', 'sh600809', 'sz000002',
                 'sz002594', 'sz002739']

# 港股沙盒
K_SAND_BOX_HK = ['hk03333', 'hk00700', 'hk02333', 'hk01359',
                 'hk00656', 'hk03888', 'hk02318']
```

### 数据验证
- 数据完整性检查：`ABuDataCheck.py`
- 数据解析器：`ABuDataParser.py`
- 数据缓存管理：`ABuDataCache.py`

## 常见问题 (FAQ)

### Q: 支持哪些市场？
A: 美股、A股、港股、国内期货、国际期货、比特币、莱特币

### Q: 如何自定义数据源？
A: 继承 `BaseMarket` 类，实现 `fetch_kline_data()` 方法

### Q: 数据存储在哪里？
A: 默认在 `abupy/RomDataBu/` 目录，支持 CSV 和 HDF5 格式

### Q: 如何更新全市场数据？
```python
from abupy import abu, EMarketTargetType
abupy.env.g_market_target = EMarketTargetType.E_MARKET_TARGET_US
abu.run_kl_update(n_folds=6)
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuSymbol.py` | Symbol 类定义，代码转换逻辑 |
| `ABuSymbolPd.py` | 获取 K 线数据的核心接口 |
| `ABuSymbolStock.py` | A股/美股/港股股票代码查询 |
| `ABuSymbolFutures.py` | 期货代码查询 |
| `ABuMarket.py` | 市场分割、选股、K-Fold 切分 |
| `ABuDataBase.py` | 市场数据基类 |
| `ABuDataParser.py` | 数据解析器 |
| `ABuDataSource.py` | 数据源管理 |
| `ABuDataCache.py` | 数据缓存管理 |
| `ABuDataCheck.py` | 数据验证 |
| `ABuDataFeed.py` | 数据馈送 |
| `ABuIndustries.py` | 行业分类 |
| `ABuMarketDrawing.py` | 市场数据可视化 |
| `ABuNetWork.py` | 网络请求封装 |
| `ABuHkUnit.py` | 港股单位转换 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 MarketBu 模块文档
- 识别 15 个核心文件
- 文档覆盖率：100%
