[根目录](../../CLAUDE.md) > **TLineBu**

---

# TLineBu - 趋势线分析模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

TLineBu 提供趋势线分析和绘制功能，包括支撑线、阻力线等。

## 入口与启动

### 主入口
```python
from abupy.TLineBu import tl

# 使用趋势线
tl.draw_support(kl_df)
tl.draw_resistance(kl_df)
```

## 对外接口

### 核心函数

```python
class ABuTL:
    """趋势线工具"""

    @staticmethod
    def draw_support(kl_df):
        """绘制支撑线"""

    @staticmethod
    def draw_resistance(kl_df):
        """绘制阻力线"""
```

## 关键依赖与配置

### 依赖模块
- `matplotlib` - 可视化
- `pandas` - 数据处理

## 常见问题 (FAQ)

### Q: 如何绘制趋势线？
```python
from abupy.TLineBu import tl

tl.draw_support(kl_df)  # 支撑线
tl.draw_resistance(kl_df)  # 阻力线
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuTL.py` | 趋势线工具 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 TLineBu 模块文档
- 识别 1 个核心文件
- 文档覆盖率：100%
