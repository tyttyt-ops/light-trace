##光线追踪 
姓名：唐悦婷 学号：202311061051 专业：计算机科学与技术（公费师范）

基于 Taichi 的交互式光线追踪渲染器，实现了经典的 Whitted-Style 光线追踪算法。

## 预览效果

<!-- [![Light Trace Demo](docs/demo.gif)](docs/demo.gif) -->

**1.phong模型** 

<img width="480" height="360" alt="converted" src="https://github.com/user-attachments/assets/aff858e9-6e02-4dc9-bad5-8733cf7cd9bc" />

**2.phong模型（透明材质）** 

<img width="480" height="360" alt="透明光线" src="https://github.com/user-attachments/assets/c65053f5-b5e2-4bdc-a1b1-7161fe8c9178" />

## 功能特性

- 光线投射与光线追踪的完整实现
- 硬阴影检测（Shadow Ray）
- 理想镜面反射（Perfect Reflection）
- 玻璃材质与折射效果（基于斯涅尔定律 Snell's Law）
- 全内反射处理（Total Internal Reflection）
- MSAA 抗锯齿（Multi-Sample Anti-Aliasing）
- 交互式 GUI 控制面板

## 项目架构

```
light-trace/
├── tracing.py          # 主渲染器实现
│   ├── scene_intersect # 场景求交函数
│   ├── render          # 渲染内核（迭代式光线追踪）
│   ├── reflect         # 反射计算
│   ├── refract         # 折射计算（斯涅尔定律）
│   └── main            # 主函数（窗口与交互）
├── test_glass.py       # 玻璃材质测试版本
└── README.md           # 项目说明文档
```

## 场景元素

| 元素 | 材质 | 位置 | 描述 |
|------|------|------|------|
| 地板 | 漫反射 | y = -1.0 | 无限大平面，黑白棋盘格纹理 |
| 红球 | 玻璃 | (-1.5, 0.0, 0) | 折射材质，红色着色 |
| 银球 | 镜面 | (1.5, 0.0, 0) | 反射材质，银色着色 |

## 交互控制

- **Light X / Light Y / Light Z**：动态调整点光源三维坐标
- **Max Bounces**：设置光线最大弹射次数（范围 1-5）

> **[📷 插入交互控制面板截图：docs/controls.png]** - 显示 GUI 滑动条控件

## 技术实现

### 迭代式光线追踪

由于 GPU 不擅长处理递归，采用迭代循环模式：

```
for bounce in range(max_bounces):
    1. 求交场景
    2. 材质分支处理：
       - 镜面反射 → 更新光线方向，继续循环
       - 玻璃折射 → 计算透射方向（含全内反射处理）
       - 漫反射 → 计算光照，终止循环
```

### 斯涅尔定律折射

```
R = η * I + (η * cos_i - cos_t) * N
```

- 外部进入玻璃：η = 1.0/1.5
- 玻璃射出外部：η = 1.5/1.0
- 全内反射：当 sin²(θ_t) > 1 时回退到反射

### 浮点数精度处理

通过法线偏移解决 Shadow Acne：

```
ro = p + N * ε    (ε = 1e-4)
```

## 安装与运行

```bash
# 安装依赖
pip install taichi

# 运行主程序
python tracing.py
```

## 要求

- Python 3.10+
- Taichi 1.7+

## 实验说明

本项目用于光线追踪实验教学，包含以下核心概念：

1. **光线投射与光线追踪的本质区别**：光线投射仅计算首次交点，光线追踪递归追踪次级射线
2. **次级射线**：通过发射暗影射线实现硬阴影，通过反射射线实现镜面反射
3. **GPU 并行计算**：使用迭代循环代替递归，适应并行计算架构
4. **斯涅尔定律**：实现折射效果和全内反射处理
5. **MSAA 抗锯齿**：每个像素多次采样，平滑边缘过渡

## 参考资料

- Taichi 官方文档：https://docs.taichi-lang.org/
- Whitted-Style 光线追踪：https://en.wikipedia.org/wiki/Ray_tracing_(graphics)
- 斯涅尔定律：https://en.wikipedia.org/wiki/Snell%27s_law
