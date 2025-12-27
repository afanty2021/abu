[根目录](../../CLAUDE.md) > **CheckBu**

---

# CheckBu - 参数检查模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

CheckBu 提供函数参数检查和验证功能。

## 入口与启动

### 主入口
```python
from abupy.CheckBu import (
    arg_process,      # 参数处理装饰器
    return_process,   # 返回值处理装饰器
    FuncChecker,      # 函数检查器
    ArgChecker,       # 参数检查器
    ReturnChecker     # 返回值检查器
)

# 使用装饰器
@arg_process
def my_function(arg1, arg2):
    pass
```

## 对外接口

### 装饰器

```python
def arg_process(func):
    """参数处理装饰器"""

def return_process(func):
    """返回值处理装饰器"""
```

### 检查器

```python
class ArgChecker:
    """参数检查器"""

class ReturnChecker:
    """返回值检查器"""

class FuncChecker:
    """函数检查器"""
```

## 关键依赖与配置

### 依赖模块
- `functools` - 函数工具

## 常见问题 (FAQ)

### Q: 如何使用参数检查？
```python
from abupy.CheckBu import arg_process

@arg_process
def my_function(arg1, arg2):
    # 函数体
    pass
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuProcessor.py` | 处理器 |
| `ABuChecker.py` | 检查器 |
| `ABuChecks.py` | 检查集合 |
| `ABuFuncUtil.py` | 函数工具 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 CheckBu 模块文档
- 识别 4 个核心文件
- 文档覆盖率：100%
