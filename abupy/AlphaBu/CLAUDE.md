[根目录](../../CLAUDE.md) > **AlphaBu**

---

# AlphaBu - 择时与选股主控模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

AlphaBu 是择时（PickTime）和选股（PickStock）的主控模块，负责多进程并行执行择时和选股策略，协调 Worker 和 Master 之间的任务分配。

## 入口与启动

### 主入口
```python
from abupy.AlphaBu import AbuPickStockMaster, AbuPickTimeMaster

# 选股（由 run_loop_back 自动调用）
AbuPickStockMaster.do_pick_stock_with_process(
    capital, benchmark, stock_picks,
    choice_symbols=None, n_process_pick_stock=None
)

# 择时（由 run_loop_back 自动调用）
AbuPickTimeMaster.do_symbols_with_same_factors_process(
    choice_symbols, benchmark,
    buy_factors, sell_factors, capital
)
```

## 对外接口

### 选股主控

```python
class AbuPickStockMaster:
    """选股主控"""

    @staticmethod
    def do_pick_stock_with_process(
        capital, benchmark, stock_picks,
        choice_symbols=None,
        n_process_pick_stock=None
    ):
        """
        多进程执行选股策略
        :param capital: 资金实例
        :param benchmark: 基准实例
        :param stock_picks: 选股因子列表
        :param choice_symbols: 备选股票池
        :param n_process_pick_stock: 进程数
        :return: 选中的股票列表
        """
```

### 择时主控

```python
class AbuPickTimeMaster:
    """择时主控"""

    @staticmethod
    def do_symbols_with_same_factors_process(
        choice_symbols, benchmark,
        buy_factors, sell_factors, capital,
        kl_pd_manager=None,
        n_process_kl=None,
        n_process_pick_time=None
    ):
        """
        多进程执行择时策略
        :param choice_symbols: 股票列表
        :param benchmark: 基准实例
        :param buy_factors: 买入因子列表
        :param sell_factors: 卖出因子列表
        :param capital: 资金实例
        :param kl_pd_manager: K线管理器
        :param n_process_kl: K线获取进程数
        :param n_process_pick_time: 择时进程数
        :return: (orders_pd, action_pd, all_fit_symbols_cnt)
        """
```

### Worker 类

```python
class AbuPickStockWorker(AbuPickStockWorkBase):
    """选股工作进程"""
    def work(self):
        """执行选股任务"""

class AbuPickTimeWorker(AbuPickTimeWorkBase):
    """择时工作进程"""
    def work(self):
        """执行择时任务"""
```

## 关键依赖与配置

### 依赖模块
- `CoreBu` - 并行处理
- `MarketBu` - 市场数据
- `TradeBu` - 交易执行
- `FactorBuyBu` - 买入因子
- `FactorSellBu` - 卖出因子
- `PickStockBu` - 选股因子

### 并行配置

```python
# 默认进程数
n_process_pick = ABuEnv.g_cpu_cnt          # 择时/选股进程数
n_process_kl = ABuEnv.g_cpu_cnt * 2        # K线获取进程数（Mac 下加倍）

# Windows 下限制
if not ABuEnv.g_is_mac_os and symbol_count < 20:
    n_process_pick = 1
    n_process_kl = 1
```

## 数据模型

### 选股因子

```python
class AbuPickStockBase(AbuParamBase):
    """选股因子基类"""

    def fit_pick(self, kl_pd, target_symbol):
        """
        :param kl_pd: 金融时间序列
        :param target_symbol: 目标股票
        :return: True/False
        """
        pass
```

### 择时执行流程

```python
# 1. 初始化 Capital 和 Benchmark
# 2. 执行选股（可选）
# 3. 批量获取 K 线数据
# 4. 多进程执行择时
# 5. 返回交易结果
```

## 测试与质量

### 使用示例

```python
# 教程 5: 选股策略的开发
# 教程 29: 多因子策略并行执行配合
```

## 常见问题 (FAQ)

### Q: 选股和择时有什么区别？
- **选股**: 从全市场筛选出符合条件的股票池
- **择时**: 对选定的股票决定何时买入/卖出

### Q: 如何使用选股？
```python
stock_picks = [
    {'class': AbuPickStockPriceMinMax,
     'threshold_price_min': 50.0, 'reversed': False}
]
abu_result, _ = abu.run_loop_back(
    read_cash=1000000,
    buy_factors=buy_factors,
    sell_factors=sell_factors,
    stock_picks=stock_picks
)
```

### Q: 如何调整并行进程数？
```python
abu_result, _ = abu.run_loop_back(
    read_cash=1000000,
    buy_factors=buy_factors,
    sell_factors=sell_factors,
    n_process_kl=8,      # K 线获取进程
    n_process_pick=4     # 择时进程
)
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuPickBase.py` | 基础类 |
| `ABuPickStockMaster.py` | 选股主控 |
| `ABuPickStockWorker.py` | 选股工作进程 |
| `ABuPickStockExecute.py` | 选股执行 |
| `ABuPickTimeMaster.py` | 择时主控 |
| `ABuPickTimeWorker.py` | 择时工作进程 |
| `ABuPickTimeExecute.py` | 择时执行 |
| `ABuAlpha.py` | Alpha 模块封装 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 AlphaBu 模块文档
- 识别 8 个核心文件
- 文档覆盖率：100%
