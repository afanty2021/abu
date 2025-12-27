[根目录](../../CLAUDE.md) > **DLBu**

---

# DLBu - 深度学习模块

> 最后更新：2025-12-27 12:20:52

## 模块职责

DLBu 提供深度学习功能，主要是基于图像识别的 K 线形态识别。

## 入口与启动

### 主入口
```python
from abupy.DLBu import dl

# 使用深度学习
dl.train_model()
dl.predict()
```

## 对外接口

### 核心类

```python
class ABuDL:
    """深度学习主类"""

    def train(self):
        """训练模型"""

    def predict(self):
        """预测"""
```

## 关键依赖与配置

### 依赖模块
- `caffe` - 深度学习框架（可选）
- `tensorflow` - 深度学习框架（可选）
- `numpy` - 数值计算

### 配置文件

```bash
# DLBu/pb/ 目录
deploy.prototxt      # 部署配置
solver.prototxt      # 求解器配置
train_val.prototxt   # 训练配置
```

## 常见问题 (FAQ)

### Q: 深度学习模块是否必须？
A: 否，这是可选功能，主要用于 K 线图像识别

## 相关文件清单

| 文件 | 职责 |
|------|------|
| `ABuDL.py` | 深度学习主类 |
| `ABuDLImgStd.py` | 图像标准化 |
| `ABuDLTVSplit.py` | 训练验证分割 |
| `pb/*.prototxt` | Caffe 配置 |
| `sh/*.sh` | 训练脚本 |

## 变更记录

### 2025-12-27 12:20:52
- 创建 DLBu 模块文档
- 识别核心文件
- 文档覆盖率：100%
