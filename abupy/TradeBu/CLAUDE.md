[根目录](../../CLAUDE.md) > **TradeBu**

---

# TradeBu - 交易执行模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

TradeBu 是交易执行的核心模块，负责资金管理、订单管理、K线数据管理、交易基准以及交易执行。它连接策略层和市场数据层，将买入/卖出信号转化为实际的交易订单。

## 入口与启动

### 主入口
```python
from abupy.TradeBu import AbuCapital, AbuBenchmark, AbuKLManager

# 初始化资金和基准
capital = AbuCapital(read_cash=1000000, benchmark)
kl_manager = AbuKLManager(benchmark, capital)
```

## 对外接口

### 核心类

```python
class AbuCapital:
    """资金管理类"""
    def __init__(self, read_cash, benchmark, user_commission_dict=None):
        """
        :param read_cash: 初始资金
        :param benchmark: 交易基准
        :param user_commission_dict: 自定义手续费字典
        """
        self.current_cash = read_cash  # 当前可用资金
        self.snapshot = None           # 资金快照

    def reset_cash(self):
        """重置资金到初始状态"""

    def current_benefit(self):
        """当前收益"""


class AbuBenchmark:
    """交易基准类"""
    def __init__(self, n_folds=2, start=None, end=None):
        """
        :param n_folds: 回测年数
        :param start: 开始日期 '2013-07-10'
        :param end: 结束日期 '2016-07-26'
        """
        self.kl_pd = None  # 基准K线数据


class AbuKLManager:
    """K线数据管理类"""
    def __init__(self, benchmark, capital):
        self.benchmark = benchmark
        self.capital = capital

    def batch_get_pick_time_kl_pd(self, choice_symbols, n_process=None):
        """批量获取择时K线数据"""


class AbuOrder:
    """交易订单类"""
    def __init__(self, symbol_id, buy_date, buy_price, buy_cnt,
                 buy_factor, expect_direction=1.0):
        """
        :param symbol_id: 股票代码
        :param buy_date: 买入日期
        :param buy_price: 买入价格
        :param buy_cnt: 买入数量
        :param buy_factor: 买入因子
        :param expect_direction: 期望方向 1.0(买涨) / -1.0(买跌)
        """
```

### 交易执行

```python
# 交易执行核心函数
def do_symbols_with_same_factors_process(
    choice_symbols, benchmark,
    buy_factors, sell_factors,
    capital, kl_pd_manager,
    n_process_kl=None, n_process_pick_time=None
):
    """
    对选定的股票序列使用相同因子进行择时
    :return: (orders_pd, action_pd, all_fit_symbols_cnt)
    """

# 订单转换为 DataFrame
def make_orders_pd(orders, kl_pd):
    """
    AbuOrder 序列转换为 pd.DataFrame
    :return: 交易订单时间序列
    """

# 计算简单收益
def calc_simple_profit(orders, kl_pd):
    """计算交易收益（不含手续费）"""
```

## 关键依赖与配置

### 依赖模块
- `pandas` - 数据处理
- `numpy` - 数值计算
- `CoreBu` - 基础类和环境
- `MarketBu` - 市场数据
- `BetaBu` - 仓位管理
- `FactorBuyBu` - 买入因子
- `FactorSellBu` - 卖出因子

### 手续费配置

```python
# 默认手续费配置
def commission_trader(trade_cnt, price):
    """默认交易手续费计算"""

# 自定义手续费
commission_dict = {
    'buy_commission_func': lambda cnt, price: cnt * price * 0.0003,
    'sell_commission_func': lambda cnt, price: cnt * price * 0.0013
}
```

## 数据模型

### 交易订单 DataFrame
```python
# columns:
# - buy_date: 买入日期
# - buy_price: 买入价格
# - buy_cnt: 买入数量
# - buy_factor: 买入因子
# - symbol: 股票代码
# - buy_pos: 买入位置
# - buy_type_str: 买入类型 (call/put)
# - expect_direction: 期望方向 (1/-1)
# - sell_date: 卖出日期
# - sell_price: 卖出价格
# - sell_type: 卖出类型
# - profit: 收益
# - result: 结果 (1/-1/0)
```

### 交易行为 DataFrame
```python
# 记录每个交易日的资金变化、持仓情况等
```

### 机器学习特征

```python
class AbuMlFeature:
    """机器学习特征提取"""

    def make_buy_order_ml_feature(self, kl_pd, order):
        """为买入订单构建机器学习特征"""

    def unzip_ml_feature(self, orders_pd):
        """解压机器学习特征到订单中"""
```

## 测试与质量

### 测试方式
- 通过 Jupyter Notebook 教程验证
- 使用沙盒股票池测试

### 质量保证
- 详细的交易日志
- 资金一致性检查
- 订单完整性验证

## 常见问题 (FAQ)

### Q: 如何自定义手续费？
```python
def free_commission(trade_cnt, price):
    return 0  # 免手续费

commission_dict = {
    'buy_commission_func': free_commission,
    'sell_commission_func': free_commission
}
capital = AbuCapital(read_cash, benchmark, user_commission_dict=commission_dict)
```

### Q: 如何获取交易订单？
```python
# 回测后获取
orders_pd = abu_result.orders_pd
action_pd = abu_result.action_pd
```

### Q: 支持哪些订单类型？
- 买涨 (call): 期望价格上涨
- 买跌 (put): 期望价格下跌（期货/期权）

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuCapital.py` | 资金管理 |
| `ABuBenchmark.py` | 交易基准 |
| `ABuKLManager.py` | K线数据管理 |
| `ABuOrder.py` | 交易订单 |
| `ABuTradeExecute.py` | 交易执行核心 |
| `ABuTradeProxy.py` | 交易代理 |
| `ABuTradeDrawer.py` | 交易可视化 |
| `ABuMLFeature.py` | 机器学习特征 |
| `ABuPosition.py` | 持仓管理 |
| `ABuRisk.py` | 风险控制 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 TradeBu 模块文档
- 识别核心文件
- 文档覆盖率：100%
