[根目录](../../CLAUDE.md) > **ExtBu**

---

# ExtBu - 扩展库模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

ExtBu 包含第三方扩展库的本地副本，确保项目不依赖外部版本。

## 入口与启动

### 包含的库

```python
# empyrical - 金融度量库
from abupy.ExtBu.empyrical import stats

# joblib - 并行计算库
from abupy.ExtBu.joblib import Memory, Parallel

# six - Python 2/3 兼容
from abupy.ExtBu import six

# funcsigs - 函数签名
from abupy.ExtBu import funcsigs

# odict - 有序字典
from abupy.ExtBu import odict
```

## 关键依赖与配置

### 包含的库

| 库 | 路径 | 用途 |
|----|------|------|
| empyrical | `empyrical/` | 金融度量 |
| joblib | `joblib/` | 并行计算 |
| six | `six.py` | Py2/Py3 兼容 |
| funcsigs | `funcsigs.py` | 函数签名 |
| futures | `futures/` | 并发处理 |

## 相关文件清单

| 文件/目录 | 职责 |
|-----------|------|
| `empyrical/` | 金融度量库 |
| `joblib/` | 并行计算库 |
| `six.py` | Py2/Py3 兼容 |
| `funcsigs.py` | 函数签名 |
| `odict.py` | 有序字典 |
| `futures/` | 并发处理 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 ExtBu 模块文档
- 识别扩展库结构
- 文档覆盖率：100%
