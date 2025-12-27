[根目录](../../CLAUDE.md) > **UmpBu**

---

# UmpBu - UMP 裁判决策系统

> 最后更新：2025-12-27 12:20:52

## 模块职责

UmpBu 实现 UMP (UMP Main Referee & Ump Edge Referee) 裁判决策系统，通过机器学习训练裁判模型来拦截不良交易，提高策略实盘效果。

## 入口与启动

### 主入口
```python
from abupy.UmpBu import (
    AbuUmpMainBase,      # 主裁基类
    AbuUmpEdgeBase,      # 边裁基类
    AbuUmpMainDeg,       # 角度主裁
    AbuUmpMainFull,      # 完整主裁
    AbuUmpEdgeDeg,       # 角度边裁
    AbuUmpEdgeFull,      # 完整边裁
    ump                  # 模块封装
)
```

## 对外接口

### 基类

```python
class AbuUmpBase:
    """裁判基类"""

    def predict(self, x, need_hit_cnt=1):
        """
        预测是否拦截交易
        :param x: 特征
        :param need_hit_cnt: 需要命中的裁判数量
        :return: 1(通过) / 0(拦截)
        """

    def train_ump(self, orders_pd, **kwargs):
        """
        训练裁判
        :param orders_pd: 订单数据
        """


class BuyUmpMixin:
    """买入裁判混入"""
    _ump_type_prefix = 'buy_'


class SellUmpMixin:
    """卖出裁判混入"""
    _ump_type_prefix = 'sell_'
```

### 主裁 (Main Referee)

```python
class AbuUmpMainBase(AbuUmpBase):
    """
    主裁：基于交易整体特征判断
    - 训练数据：所有历史交易
    - 特征：买入价格、日期、角度等
    """


class AbuUmpMainDeg(AbuUmpMainBase, BuyUmpMixin):
    """角度主裁：基于价格变化角度"""


class AbuUmpMainFull(AbuUmpMainBase, BuyUmpMixin):
    """完整主裁：使用所有特征"""


class AbuUmpMainPrice(AbuUmpMainBase, BuyUmpMixin):
    """价格主裁：基于价格位置"""


class AbuUmpMainJump(AbuUmpMainBase, BuyUmpMixin):
    """跳空主裁：基于跳空缺口"""


class AbuUmpMainWave(AbuUmpMainBase, BuyUmpMixin):
    """波浪主裁：基于波浪理论"""


class AbuUmpMainMul(AbuUmpMainBase, BuyUmpMixin):
    """多元主裁：综合多种特征"""
```

### 边裁 (Edge Referee)

```python
class AbuUmpEdgeBase(AbuUmpBase):
    """
    边裁：基于局部特征判断
    - 训练数据：特定类型的交易
    - 特征：K线形态、技术指标等
    """


class AbuUmpEdgeDeg(AbuUmpEdgeBase, BuyUmpMixin):
    """角度边裁"""


class AbuUmpEdgeFull(AbuUmpEdgeBase, BuyUmpMixin):
    """完整边裁"""


class AbuUmpEdgePrice(AbuUmpEdgeBase, BuyUmpMixin):
    """价格边裁"""


class AbuUmpEdgeMul(AbuUmpEdgeBase, BuyUmpMixin):
    """多元边裁"""


class AbuUmpEdgeWave(AbuUmpEdgeBase, BuyUmpMixin):
    """波浪边裁"""
```

### 裁判管理器

```python
class CachedUmpManager:
    """裁判缓存管理器"""

    def get_ump(self, ump):
        """获取训练好的裁判"""


class AbuUmpManager:
    """裁判管理器"""

    def __init__(self, factor):
        """
        :param factor: 择时因子实例
        """
```

## 关键依赖与配置

### 依赖模块
- `sklearn` - 机器学习
- `numpy` - 数值计算
- `CoreBu` - 基础类

### 训练配置

```python
# 训练裁判（教程 16-18）
ump = AbuUmpMainDeg()
ump.train_ump(orders_pd)
```

## 数据模型

### 裁判类型

```python
# 主裁类型
- buy_main_deg       # 买入角度主裁
- buy_main_full      # 买入完整主裁
- sell_main_deg      # 卖出角度主裁

# 边裁类型
- buy_edge_deg       # 买入角度边裁
- buy_edge_full      # 买入完整边裁
- sell_edge_deg      # 卖出角度边裁
```

### 缓存路径

```python
# 裁判模型缓存
dump_file_fn = f'/path/to/cache/{ump_type}_{factor_name}_{market}.p'
```

## 测试与质量

### 使用示例

```python
# 教程 16: UMP 主裁交易决策
# 教程 17: UMP 边裁交易决策
# 教程 18: 自定义裁判决策交易
```

### 工作流程

```python
# 1. 运行回测，获取 orders_pd
# 2. 使用 orders_pd 训练裁判
# 3. 在买入因子中应用裁判
# 4. 重新回测，验证拦截效果
```

## 常见问题 (FAQ)

### Q: 主裁和边裁有什么区别？
- **主裁**: 基于整体交易特征，判断是否应该交易
- **边裁**: 基于局部形态特征，判断特定形态是否可靠

### Q: 如何使用裁判？
```python
from abupy.UmpBu import AbuUmpMainDeg

# 1. 训练裁判
ump = AbuUmpMainDeg()
ump.train_ump(orders_pd)

# 2. 在买入因子中应用
buy_factors = [{
    'xd': 60,
    'class': AbuFactorBuyBreak,
    'ump': ump  # 应用裁判
}]
```

### Q: 如何自定义裁判？
```python
from abupy.UmpBu import AbuUmpBase, BuyUmpMixin

class MyUmp(AbuUmpBase, BuyUmpMixin):
    def predict(self, x, need_hit_cnt=1):
        # 自定义预测逻辑
        return 1  # 通过
        return 0  # 拦截
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuUmpBase.py` | 裁判基类、缓存管理 |
| `ABuUmpMainBase.py` | 主裁基类 |
| `ABuUmpMainDeg.py` | 角度主裁 |
| `ABuUmpMainFull.py` | 完整主裁 |
| `ABuUmpMainPrice.py` | 价格主裁 |
| `ABuUmpMainJump.py` | 跳空主裁 |
| `ABuUmpMainWave.py` | 波浪主裁 |
| `ABuUmpMainMul.py` | 多元主裁 |
| `ABuUmpEdgeBase.py` | 边裁基类 |
| `ABuUmpEdgeDeg.py` | 角度边裁 |
| `ABuUmpEdgeFull.py` | 完整边裁 |
| `ABuUmpEdgeMul.py` | 多元边裁 |
| `ABuUmpEdgeWave.py` | 波浪边裁 |
| `ABuUmpEdgePrice.py` | 价格边裁 |
| `ABuUmp.py` | 模块封装 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 UmpBu 模块文档
- 识别 17 个核心文件
- 文档覆盖率：100%
