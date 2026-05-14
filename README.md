# Light Trace - 光线追踪演示

基于 Taichi 框架实现的交互式光线追踪演示程序。

## 功能特性

- **Whitted-Style 光线追踪**：支持漫反射和镜面反射材质
- **实时交互**：可调整光源位置和最大弹射次数
- **硬阴影**：实现精确的阴影检测
- **棋盘格纹理**：地板采用经典棋盘格图案
- **Phong 光照模型**：包含环境光和漫反射光照

## 场景内容

- 红色漫反射球（左侧）
- 银色镜面反射球（右侧）
- 棋盘格地板

## 技术栈

- Python 3.x
- Taichi GPU 编程框架

## 安装依赖

```bash
pip install taichi
```

## 运行

```bash
python tracing.py
```

## 使用说明

运行程序后，可以通过右侧控制面板调整参数：
- **Light X/Y/Z**：调整光源位置
- **Max Bounces**：设置光线最大弹射次数（1-5）

## 项目结构

```
light-trace/
├── tracing.py    # 主程序文件
└── README.md     # 项目说明文档
```

## 许可证

MIT License
