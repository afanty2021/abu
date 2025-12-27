[根目录](../../CLAUDE.md) > **CoreBu**

---

# CoreBu - 核心基础模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

CoreBu 是阿布量化系统的核心基础模块，提供全局环境配置、基础类定义、并行处理框架、向后兼容性处理等基础设施。它是整个系统的"道"层，为其他所有业务模块提供底层支撑。

## 入口与启动

### 主入口
```python
from abupy import abu
from abupy.CoreBu import ABuEnv
```

### 核心函数
- `abu.run_loop_back()` - 主回测入口函数
- `abu.run_kl_update()` - 数据更新入口函数

## 对外接口

### 环境配置类 (ABuEnv)

```python
class ABuEnv:
    """全局环境配置类"""

    # 市场类型
    g_market_target = EMarketTargetType.E_MARKET_TARGET_US

    # 数据获取模式
    g_data_fetch_mode = EMarketDataFetchMode.E_DATA_FETCH_FETCH_LOCAL

    # 项目缓存目录
    g_project_cache_dir = None

    # CPU 核心数
    g_cpu_cnt = None

    # 操作系统标识
    g_is_mac_os = False
    g_is_linux = False
    g_is_windows = False
```

### 枚举类型

```python
# 市场目标类型
class EMarketTargetType(Enum):
    E_MARKET_TARGET_US = 'us'      # 美股
    E_MARKET_TARGET_CN = 'cn'      # A股
    E_MARKET_TARGET_HK = 'hk'      # 港股
    E_MARKET_TARGET_FUTURES_CN = 'cn_futures'    # 国内期货
    E_MARKET_TARGET_FUTURES_GLOBAL = 'gb_futures' # 国际期货
    E_MARKET_TARGET_TC = 'tc'      # 数字货币

# 数据获取模式
class EMarketDataFetchMode(Enum):
    E_DATA_FETCH_FETCH_LOCAL = 'local'        # 本地缓存
    E_DATA_FETCH_FETCH_NET = 'net'            # 网络获取
    E_DATA_FETCH_FETCH_AUTO = 'auto'          # 自动选择

# 数据存储类型
class EDataCacheType(Enum):
    E_DATA_CACHE_CSV = 'csv'
    E_DATA_CACHE_HDF5 = 'hdf5'
    E_DATA_FETCH_HDF5_NETWORK_DAY = 'hdf5_network_day'
```

### 基础类

```python
class AbuParamBase:
    """参数基类，所有因子类的基础"""

class FreezeAttrMixin:
    """冻结属性混入类，防止属性被意外修改"""

class PickleStateMixin:
    """序列化状态混入类，支持对象序列化"""

class AbuResultTuple(namedtuple):
    """回测结果元组: (orders_pd, action_pd, capital, benchmark)"""
```

## 关键依赖与配置

### 依赖模块
- `six` - Python 2/3 兼容库
- `multiprocessing` - 多进程并行
- `joblib` - 并行计算（通过 ExtBu）
- `pandas` - 数据处理

### 配置文件
项目无独立配置文件，环境通过 `ABuEnv` 类动态配置

## 数据模型

### 回测结果
```python
AbuResultTuple(
    orders_pd,      # 交易订单 DataFrame
    action_pd,      # 交易行为 DataFrame
    capital,        # AbuCapital 资金对象
    benchmark       # AbuBenchmark 基准对象
)
```

### 存储枚举
```python
class EStoreAbu(Enum):
    E_STORE_ABU_RESULT = 'abu_result'
    E_STORE_ABU_CPTL = 'abu_cptl'
    E_STORE_ABU_KL_manager = 'abu_kl_manager'
```

## 测试与质量

### 测试覆盖
- **无传统单元测试**
- 通过教程 Notebook 验证功能

### 代码质量
- 完整的 Python 2/3 兼容处理
- 使用 `from __future__ import` 确保未来兼容
- 详细的文档字符串

## 常见问题 (FAQ)

### Q: 如何设置回测市场？
```python
from abupy.CoreBu import EMarketTargetType
abupy.env.g_market_target = EMarketTargetType.E_MARKET_TARGET_CN
```

### Q: 如何切换数据源？
```python
from abupy.CoreBu import EMarketDataFetchMode
abupy.env.g_data_fetch_mode = EMarketDataFetchMode.E_DATA_FETCH_FETCH_NET
```

### Q: 如何设置缓存目录？
```python
abupy.env.g_project_cache_dir = '/path/to/cache'
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABu.py` | 主回测入口函数 |
| `ABuEnv.py` | 全局环境配置和枚举定义 |
| `ABuBase.py` | 基础类定义 |
| `ABuFixes.py` | Python 2/3 兼容处理 |
| `ABuParallel.py` | 并行处理封装 |
| `ABuPdHelper.py` | Pandas 辅助函数 |
| `ABuStore.py` | 序列化存储工具 |
| `ABuDeprecated.py` | 废弃 API 处理 |
| `ABuEnvProcess.py` | 环境处理函数 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 CoreBu 模块文档
- 识别 9 个核心文件
- 文档覆盖率：100%
