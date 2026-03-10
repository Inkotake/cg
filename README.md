# Taichi 粒子群交互实验

这是一个基于 `Taichi` 的二维粒子群交互项目。程序使用 GPU 并行计算大量粒子的运动状态，鼠标位置作为引力中心，粒子在阻力和边界反弹的共同作用下形成动态聚散效果。

## 项目结构

```text
.
├─ main.py
├─ pyproject.toml
├─ resource/
│  └─ work0.gif
└─ src/
   └─ Work0/
      ├─ __init__.py
      ├─ config.py
      ├─ main.py
      └─ physics.py
```

## 模块说明

- `src/Work0/main.py`：程序入口，初始化 Taichi，创建 GUI 窗口，循环读取鼠标位置并触发渲染。
- `src/Work0/physics.py`：核心物理逻辑，使用 Taichi `kernel` 并行更新粒子的坐标和速度。
- `src/Work0/config.py`：集中管理粒子数量、引力强度、阻力系数、反弹系数、窗口尺寸等参数。
- `resource/work0.gif`：项目运行演示录屏，可直接在仓库中展示效果。

## 核心实现逻辑

1. 初始化阶段在 GPU 显存中创建 `pos` 和 `vel` 两组向量场，分别存储粒子位置与速度。
2. `init_particles()` 为每个粒子生成随机初始坐标，并将速度初始化为零。
3. `update_particles(mouse_x, mouse_y)` 在每一帧中并行计算粒子到鼠标位置的方向向量和距离。
4. 当粒子与鼠标距离足够大时，为其施加朝向鼠标的引力；随后叠加阻力衰减，更新位移。
5. 若粒子越过窗口边界，则执行位置钳制和速度反向衰减，实现反弹效果。
6. GUI 每一帧读取粒子坐标并绘制圆点，从而形成实时交互动画。

## 已实现功能

- GPU 并行粒子更新
- 鼠标引力交互
- 粒子阻尼运动
- 窗口边界碰撞与反弹
- 实时图形渲染展示

## 运行方式

### 1. 安装依赖

推荐使用 `uv`：

```bash
uv sync
```

如果本地没有 `uv`，也可以使用 `pip`：

```bash
pip install taichi
```

### 2. 启动项目

```bash
uv run -m src.Work0.main
```

运行后会弹出图形窗口，移动鼠标即可观察粒子群向鼠标位置聚拢并发生动态反弹。

## 演示效果

下图为项目运行效果录屏：

![粒子群演示](resource/work0.gif)


