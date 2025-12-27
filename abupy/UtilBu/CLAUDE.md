[根目录](../../CLAUDE.md) > **UtilBu**

---

# UtilBu - 工具类模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

UtilBu 提供各种工具函数，包括日期处理、文件操作、数学计算、进度条等。

## 入口与启动

### 主入口
```python
from abupy.UtilBu import (
    ABuDateUtil,      # 日期工具
    ABuFileUtil,      # 文件工具
    ABuStatsUtil,     # 统计工具
    ABuScalerUtil,    # 数据缩放
    ABuStrUtil,       # 字符串工具
    ABuProgress,      # 进度条
    ABuKLUtil,        # K线工具
    AbuProgress       # 进度条类
)
```

## 对外接口

### 日期工具

```python
class ABuDateUtil:
    """日期处理"""

    @staticmethod
    def date_str_to_int(date_str):
        """日期字符串转整数"""

    @staticmethod
    def fmt_date(date_int):
        """整数转日期字符串"""
```

### 文件工具

```python
class ABuFileUtil:
    """文件操作"""

    @staticmethod
    def ensure_dir(dir_path):
        """确保目录存在"""

    @staticmethod
    def load_pickle(file_path):
        """加载 pickle"""

    @staticmethod
    def dump_pickle(obj, file_path):
        """保存 pickle"""
```

### 统计工具

```python
class ABuStatsUtil:
    """统计计算"""

    @staticmethod
    def std(returns):
        """标准差"""

    @staticmethod
    def sharpe_ratio(returns):
        """夏普比率"""
```

### 进度条

```python
class AbuProgress:
    """进度条"""

    @staticmethod
    def do_clear_output(wait=False):
        """清除输出"""
```

## 关键依赖与配置

### 依赖模块
- `pandas` - 数据处理
- `numpy` - 数值计算
- `datetime` - 日期处理
- `os` - 文件操作

## 常见问题 (FAQ)

### Q: 如何使用日期工具？
```python
from abupy.UtilBu import ABuDateUtil

date_int = ABuDateUtil.date_str_to_int('2023-01-01')  # 20230101
date_str = ABuDateUtil.fmt_date(20230101)  # '2023-01-01'
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuDateUtil.py` | 日期工具 |
| `ABuFileUtil.py` | 文件工具 |
| `ABuStatsUtil.py` | 统计工具 |
| `ABuScalerUtil.py` | 数据缩放 |
| `ABuStrUtil.py` | 字符串工具 |
| `ABuProgress.py` | 进度条 |
| `ABuKLUtil.py` | K线工具 |
| `ABuDTUtil.py` | 日期时间工具 |
| `ABuDelegateUtil.py` | 委托工具 |
| `ABuRegUtil.py` | 正则工具 |
| `ABuMd5.py` | MD5 工具 |
| `ABuPlatform.py` | 平台工具 |
| `ABuOsUtil.py` | 系统工具 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 UtilBu 模块文档
- 识别 13 个核心文件
- 文档覆盖率：100%
