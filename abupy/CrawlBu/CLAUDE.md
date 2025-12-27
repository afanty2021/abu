[根目录](../../CLAUDE.md) > **CrawlBu**

---

# CrawlBu - 数据爬虫模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

CrawlBu 负责从雪球网爬取股票数据。

## 入口与启动

### 主入口
```python
from abupy.CrawlBu import ABuXqCrawl

# 爬取数据
data = ABuXqCrawl.crawl_kline('usTSLA')
```

## 对外接口

### 核心类

```python
class ABuXqCrawl:
    """雪球数据爬虫"""

    @staticmethod
    def crawl_kline(symbol):
        """爬取 K 线数据"""


class ABuXqApi:
    """雪球 API 封装"""
```

## 关键依赖与配置

### 依赖模块
- `requests` - HTTP 请求
- `pandas` - 数据处理

## 常见问题 (FAQ)

### Q: 如何使用爬虫？
```python
from abupy.CrawlBu import ABuXqCrawl

data = ABuXqCrawl.crawl_kline('usTSLA')
```

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuXqCrawl.py` | 爬虫主类 |
| `ABuXqCrawlImp.py` | 爬虫实现 |
| `ABuXqApi.py` | API 封装 |
| `ABuXqConsts.py` | 常量定义 |
| `ABuXqFile.py` | 文件处理 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 CrawlBu 模块文档
- 识别 5 个核心文件
- 文档覆盖率：100%
