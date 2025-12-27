[根目录](../../CLAUDE.md) > **SimilarBu**

---

# SimilarBu - 相关性分析模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

SimilarBu 提供股票相关性分析和相似股票查找功能。

## 入口与启动

### 主入口
```python
from abupy.SimilarBu import (
    find_similar_with_se,      # 按标准差查找
    find_similar_with_folds,   # 按 K-Fold 查找
    find_similar_with_cnt,     # 按数量查找
    ABuCorrcoef,               # 相关系数
    ECoreCorrType              # 相关类型
)

# 查找相似股票
similar = find_similar_with_se('usTSLA', n_folds=10)
```

## 对外接口

### 核心函数

```python
def find_similar_with_se(target_symbol, n_folds=5, corr_type=ECoreCorrType.E_CORE_CORR_PEARSON):
    """
    按标准差查找相似股票
    :param target_symbol: 目标股票
    :param n_folds: 折数
    :param corr_type: 相关类型
    :return: 相似股票列表
    """

def find_similar_with_folds(target_symbol, n_folds=5):
    """按 K-Fold 查找"""

def find_similar_with_cnt(target_symbol, cnt=5):
    """按数量查找"""
```

### 相关类型

```python
class ECoreCorrType(Enum):
    E_CORE_CORR_PEARSON = 'pearson'    # 皮尔逊相关
    E_CORE_CORR_SPERMAN = 'spearman'   # 斯皮尔曼相关
    E_CORE_CORR_KENDALL = 'kendall'    # 肯德尔相关
```

## 关键依赖与配置

### 依赖模块
- `pandas` - 数据处理
- `numpy` - 数值计算
- `scipy` - 统计计算

## 测试与质量

### 使用示例

```python
# 教程 14: 量化相关性分析应用
```

## 常见问题 (FAQ)

### Q: 如何查找相似股票？
```python
from abupy.SimilarBu import find_similar_with_se

similar = find_similar_with_se('usTSLA', n_folds=10)
print(similar)
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuSimilar.py` | 相似性计算 |
| `ABuCorrcoef.py` | 相关系数 |
| `ABuSimilarDrawing.py` | 相似性可视化 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 SimilarBu 模块文档
- 识别 3 个核心文件
- 文档覆盖率：100%
