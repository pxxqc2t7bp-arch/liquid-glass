# Apple Liquid Glass UI 设计规范

这是一个面向 Stitch、AI UI 生成工具和产品设计协作的中文设计规范仓库。它把 Apple 式的克制产品语言，与 Liquid Glass 的半透明玻璃、折射、高光、悬浮层级和物理动效结合在一起，方便快速生成更接近 iOS / visionOS / macOS 新一代系统气质的界面。

## 项目内容

仓库目前只保留一份核心设计文档：

- `DESIGN-liquid-glass.md`：完整的 Apple Liquid Glass UI 设计说明，包含视觉原则、材质层级、颜色 token、组件规范、动效、可访问性、移动端性能约束，以及可直接粘贴给 Stitch 的英文 Prompt。

## 适合谁使用

- 想用 Stitch 生成 Apple Liquid Glass 风格 UI 的设计师
- 想给 AI UI 工具提供稳定设计约束的产品同学
- 想复用 Apple-like 视觉语言的前端开发者
- 想避免普通 glassmorphism 套路感的界面设计者

## 设计方向

这份规范强调：

- Apple-like 的产品优先、系统字体、单一 Action Blue 交互色
- Liquid Glass 的真实 `backdrop-filter`、透明玻璃、边缘高光、环境色折射
- 清晰的材质层级：薄玻璃导航、中等玻璃卡片、厚玻璃弹窗
- 可用的第一屏产品 UI，而不是概念海报或说明页
- 高可读性、移动端性能和无障碍约束

## 使用方法

1. 打开 `DESIGN-liquid-glass.md`
2. 阅读顶部的设计目标和硬性要求
3. 将文档末尾的 Stitch Prompt 粘贴到 Stitch
4. 根据你的产品类型补充具体页面需求，例如 dashboard、媒体播放器、地图、日历、购物页或设计工具

## 推荐生成要求

给 Stitch 或其他 UI 工具补充需求时，可以保留这些约束：

- 第一屏必须是可用 UI，不要做介绍页
- 多使用 Liquid Glass：导航、浮动条、卡片、搜索、tab、弹窗、toast
- 背景必须有真实内容支撑玻璃效果
- 只使用 Apple Action Blue 作为主交互色
- 不要使用紫蓝渐变、发光圆球、bokeh 和空洞玻璃卡片
- 玻璃上的文字必须始终清楚

## 文件

- [DESIGN-liquid-glass.md](./DESIGN-liquid-glass.md)

