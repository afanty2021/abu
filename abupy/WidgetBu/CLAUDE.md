[根目录](../../CLAUDE.md) > **WidgetBu**

---

# WidgetBu - IPython 界面组件模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

WidgetBu 提供 IPython Notebook 中的交互式界面组件，实现图形化操作。

## 入口与启动

### 主入口
```python
from abupy.WidgetBu import (
    WidgetRunLoopBack,    # 回测界面
    WidgetQuantTool,      # 量化工具
    WidgetGridSearch,     # 网格搜索
    WidgetCrossVal,       # 交叉验证
    WidgetUpdate,         # 数据更新
    WidgetSymbolChoice,   # 股票选择
    WidgetStockInfo,      # 股票信息
    WidgetVerifyTool      # 验证工具
)

# 使用界面
widget = WidgetRunLoopBack()
widget.show()
```

## 对外接口

### 核心组件

```python
class WidgetRunLoopBack:
    """回测界面"""

class WidgetQuantTool:
    """量化工具"""

class WidgetGridSearch:
    """网格搜索界面"""

class WidgetCrossVal:
    """交叉验证界面"""
```

## 关键依赖与配置

### 依赖模块
- `ipywidgets` - IPython 组件
- `matplotlib` - 可视化
- `pandas` - 数据处理

## 常见问题 (FAQ)

### Q: 如何使用界面组件？
```python
from abupy.WidgetBu import WidgetRunLoopBack

widget = WidgetRunLoopBack()
widget.show()
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuWGBRun.py` | 回测界面 |
| `ABuWGQuantTool.py` | 量化工具 |
| `ABuWGGridSearch.py` | 网格搜索 |
| `ABuWGCrossVal.py` | 交叉验证 |
| `ABuWGUpdate.py` | 数据更新 |
| `ABuWGBSymbol.py` | 股票选择 |
| `ABuWGStockInfo.py` | 股票信息 |
| `ABuWGVerifyTool.py` | 验证工具 |
| `ABuWGBRunBase.py` | 基础类 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 WidgetBu 模块文档
- 识别 9 个核心文件
- 文档覆盖率：100%
