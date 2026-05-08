# DESIGN: Prismatic Liquid Glass UI

这是一份更大胆的 Apple Liquid Glass 设计分支。它保留 Apple-like 的产品优先、系统字体、单一 Action Blue、清晰层级和高可读性，同时让产品内容、图表、地图、媒体画面和品牌摄影拥有更鲜明的色彩。视觉关键词是：清澈、折射、克制、鲜明、可用。

## 一句话设计指令

设计一个 Prismatic Liquid Glass 风格界面：UI chrome 保持 Apple 式安静和克制，交互色只使用 Apple Action Blue；内容层可以更大胆，允许高饱和产品画面、数据可视化、地图、媒体封面、创作画布或品牌摄影穿过半透明玻璃，被 `backdrop-filter`、饱和增强、边缘高光和环境染色自然折射。结果应像 iOS / visionOS / macOS 的 Liquid Glass，而不是普通彩色 glassmorphism。

## 核心原则

- **Content Leads**：第一屏必须是可用产品 UI、真实 dashboard、地图、媒体、日历、编辑器或产品画布，不做说明页。
- **Single Interaction Accent**：主 CTA、链接、选中态和 focus ring 只使用 Apple Action Blue。
- **Prismatic Content Color**：大胆色彩来自内容本体，而不是来自一堆彩色按钮、彩色导航或装饰渐变。
- **Glass As Function**：Liquid Glass 主要用于导航、工具栏、悬浮面板、segmented control、搜索、modal、toast 和临时反馈。
- **Legibility First**：玻璃上的文字必须清楚；鲜艳背景上要使用更厚玻璃、局部 scrim 或更稳定的 surface。
- **No Decorative Noise**：避免紫蓝发光圆球、bokeh、空洞渐变、大量嵌套玻璃卡片和彩虹 UI chrome。

## 色彩系统

```css
:root {
  --apple-blue: #0066cc;
  --apple-blue-focus: #0071e3;
  --apple-blue-on-dark: #2997ff;

  --apple-ink: #1d1d1f;
  --apple-white: #ffffff;
  --apple-parchment: #f5f5f7;
  --apple-pearl: #fafafc;
  --apple-black: #000000;

  --apple-dark-tile-1: #272729;
  --apple-dark-tile-2: #2a2a2c;
  --apple-hairline: #e0e0e0;

  /* Content-only color. Do not use these as primary CTA colors. */
  --prism-red: #ff3b30;
  --prism-orange: #ff9500;
  --prism-yellow: #ffcc00;
  --prism-green: #34c759;
  --prism-mint: #00c7be;
  --prism-cyan: #32ade6;
  --prism-indigo: #5856d6;
  --prism-pink: #ff2d55;
}
```

使用规则：

- CTA / link / focus：`#0066cc`，深色玻璃上使用 `#2997ff`。
- 正文：`#1d1d1f` 或暗色模式下接近白色。
- 页面基底：`#ffffff`、`#f5f5f7`、`#fafafc`、`#272729`、`#000000`。
- 内容色：单屏选择 `2 - 4` 个即可，优先出现在图表、地图、媒体封面、产品材质、状态点和背景画布里。
- 语义色可以出现，但面积要小，不能抢走主动作。

## Liquid Glass Tokens

```css
:root {
  color-scheme: light dark;

  --lg-blur-sm: 10px;
  --lg-blur-md: 20px;
  --lg-blur-lg: 32px;
  --lg-blur-xl: 48px;

  --lg-saturate: 1.8;
  --lg-brightness: 1.03;

  --lg-radius-sm: 8px;
  --lg-radius-lg: 18px;
  --lg-radius-xl: 24px;
  --lg-radius-pill: 9999px;

  --lg-bg-chrome: rgba(255, 255, 255, 0.58);
  --lg-bg-surface: rgba(255, 255, 255, 0.72);
  --lg-bg-elevated: rgba(255, 255, 255, 0.84);
  --lg-bg-control: rgba(255, 255, 255, 0.44);

  --lg-bg-chrome-on-vivid: rgba(255, 255, 255, 0.64);
  --lg-bg-surface-on-vivid: rgba(255, 255, 255, 0.78);
  --lg-bg-elevated-on-vivid: rgba(255, 255, 255, 0.88);
  --lg-local-scrim-on-vivid: rgba(255, 255, 255, 0.18);
  --lg-environment-tint: rgba(10, 132, 255, 0.10);

  --lg-edge-highlight: rgba(255, 255, 255, 0.72);
  --lg-edge-shadow: rgba(0, 0, 0, 0.08);
  --lg-inner-highlight: rgba(255, 255, 255, 0.34);
  --lg-inner-shadow: rgba(0, 0, 0, 0.08);

  --lg-shadow-surface: 0 10px 30px rgba(0, 0, 0, 0.12);
  --lg-shadow-elevated: 0 18px 60px rgba(0, 0, 0, 0.20);
}

@media (prefers-color-scheme: dark) {
  :root {
    --lg-bg-chrome: rgba(18, 18, 20, 0.58);
    --lg-bg-surface: rgba(32, 32, 34, 0.66);
    --lg-bg-elevated: rgba(44, 44, 46, 0.78);
    --lg-bg-control: rgba(255, 255, 255, 0.10);

    --lg-bg-chrome-on-vivid: rgba(18, 18, 20, 0.66);
    --lg-bg-surface-on-vivid: rgba(32, 32, 34, 0.74);
    --lg-bg-elevated-on-vivid: rgba(44, 44, 46, 0.84);
    --lg-local-scrim-on-vivid: rgba(0, 0, 0, 0.22);
    --lg-environment-tint: rgba(41, 151, 255, 0.12);

    --lg-edge-highlight: rgba(255, 255, 255, 0.18);
    --lg-edge-shadow: rgba(0, 0, 0, 0.42);
    --lg-inner-highlight: rgba(255, 255, 255, 0.10);
    --lg-inner-shadow: rgba(0, 0, 0, 0.30);

    --lg-shadow-surface: 0 12px 34px rgba(0, 0, 0, 0.42);
    --lg-shadow-elevated: 0 22px 70px rgba(0, 0, 0, 0.58);
  }
}
```

## 玻璃基类

```css
.liquid-glass {
  position: relative;
  overflow: hidden;
  background:
    linear-gradient(135deg, rgba(255, 255, 255, 0.34), rgba(255, 255, 255, 0.06) 42%, rgba(255, 255, 255, 0.18)),
    var(--lg-bg-surface);
  backdrop-filter: blur(var(--lg-blur-md)) saturate(var(--lg-saturate)) brightness(var(--lg-brightness));
  -webkit-backdrop-filter: blur(var(--lg-blur-md)) saturate(var(--lg-saturate)) brightness(var(--lg-brightness));
  border: 1px solid var(--lg-edge-highlight);
  box-shadow:
    inset 0 1px 0 var(--lg-inner-highlight),
    inset 0 -1px 0 var(--lg-inner-shadow),
    var(--lg-shadow-surface);
}

.liquid-glass::before {
  content: "";
  position: absolute;
  inset: 0;
  pointer-events: none;
  background:
    radial-gradient(circle at 20% 0%, rgba(255, 255, 255, 0.42), transparent 32%),
    linear-gradient(180deg, rgba(255, 255, 255, 0.26), transparent 38%);
  opacity: 0.75;
  mix-blend-mode: screen;
}

.liquid-glass.on-vivid {
  background:
    linear-gradient(135deg, rgba(255, 255, 255, 0.42), rgba(255, 255, 255, 0.10) 44%, var(--lg-environment-tint)),
    var(--lg-bg-surface-on-vivid);
  backdrop-filter: blur(var(--lg-blur-lg)) saturate(1.9) brightness(1.04);
  -webkit-backdrop-filter: blur(var(--lg-blur-lg)) saturate(1.9) brightness(1.04);
}

.liquid-glass.on-vivid::before {
  background:
    linear-gradient(180deg, var(--lg-local-scrim-on-vivid), transparent 48%),
    radial-gradient(circle at 18% 0%, rgba(255, 255, 255, 0.46), transparent 34%);
}
```

`.on-vivid` 用于高饱和地图、图表、媒体和摄影背景上。不要为了可读性把背景全部洗白；应该让玻璃变厚、边缘更亮、文字区域更稳定。

## 组件方向

- **Global Nav**：44px 左右，薄玻璃 chrome，文字 `12px - 14px`，只保留必要导航。
- **Sub Nav / Context Bar**：52px 左右，适合当前页面标题、分类和右侧 Action Blue pill CTA。
- **Floating Panel**：用于检查器、摘要、筛选器、播放信息、日历详情或 AI 输入上下文。
- **Segmented Control**：底座透明，选中项像更亮的小玻璃片漂浮在上方。
- **Search**：44px pill，半透明控制层，focus 使用 Apple Blue。
- **Modal / Popover**：最高层厚玻璃，背景 scrim + blur，内容高对比。
- **Toast**：短暂反馈，厚玻璃，避免彩色大面积背景。

普通内容卡片、文章列表、商品 tile、表格行和大面积正文区不应全部做成玻璃。让真实内容层先成立，再让玻璃功能层漂浮其上。

## 背景与内容色

优先使用：

- 产品摄影、设备渲染图、材质特写
- dashboard 图表与数据面板
- 地图、天气雷达、交通热力、旅行路线
- 音乐封面、视频画面、媒体播放器
- 日历、设计画布、代码编辑器、创作工具
- light / parchment / dark tile 与鲜明内容交替

大胆背景的推荐做法：

- 强色来自真实内容，例如图表峰值、地图区域、封面光影、产品材质或画布元素。
- 用大面积中性色或深色 tile 托住高饱和内容。
- 单屏只保留一个主色温方向，例如冷蓝绿、暖橙红或黑白加一组鲜色。
- 玻璃上的文字需要 `4.5:1` 对比度；复杂背景上使用 `.on-vivid`、局部 scrim 或更厚 surface。

避免：

- 纯装饰渐变
- 紫蓝发光圆球
- 无意义 bokeh
- 彩虹按钮体系
- 卡片套卡片
- 用玻璃效果遮蔽核心内容

## 动效

```css
@keyframes lg-enter {
  from {
    opacity: 0;
    transform: scale(0.96) translateY(10px);
    filter: blur(2px);
  }
  to {
    opacity: 1;
    transform: scale(1) translateY(0);
    filter: blur(0);
  }
}

.lg-animate-in {
  animation: lg-enter 300ms cubic-bezier(0.22, 1, 0.36, 1) both;
}
```

交互状态：

- hover：玻璃稍亮，阴影稍深，上浮 `1px - 2px`。
- active：按钮缩放到 `0.95 - 0.97`。
- focus：使用 `2px solid #0071e3`。
- modal enter：淡入 + 轻微上移 + 缩放。
- reduced motion：取消弹簧和位移。

## 生成硬性要求

- 第一屏必须是可用产品 UI，不是说明页或概念海报。
- 必须明显使用 Liquid Glass：导航、浮动条、悬浮面板、tab、搜索、modal、toast 至少选择 4 类。
- 必须出现 Apple Action Blue 作为唯一主交互色。
- 可以使用更大胆的内容配色，但强色必须来自真实内容，并被玻璃材质柔化。
- 必须有真实背景内容支撑玻璃折射，不要空白背景。
- 必须使用 Apple-like 系统字体、pill CTA、44px touch target。
- 不要把大胆配色扩散到多个竞争按钮、导航选中态或链接颜色。
- 不要牺牲可读性，玻璃上的文字必须清楚。

## 可直接粘贴给 Stitch 的 Prompt

Create a polished Prismatic Liquid Glass UI. Keep the interface Apple-like: product-first, calm, precise, highly legible, with system typography, generous spacing, pill-shaped controls, 44px touch targets, and a single interaction color: Apple Action Blue (#0066cc, or #2997ff on dark glass).

The color palette may be bolder than a traditional Apple product page, but vivid color must come from the product canvas, photography, charts, maps, media, status signals, or environmental tint behind the glass. Do not create multiple competing CTA colors. The first screen must be a usable app/product interface, not a documentation page or marketing explanation.

Use Liquid Glass as functional layered material: top navigation, contextual sub-nav, floating action bar, floating panels, segmented controls, search input, modal/popover, icon buttons, and toast notifications. Ordinary content should remain in the content layer rather than turning every list, article, product tile, or data card into glass.

Include a rich and potentially high-saturation background canvas such as product imagery, dashboard content, map, media, chart, or app workspace so the glass can blur and refract real content. When the background is vivid, make glass surfaces slightly thicker, add subtle local scrims behind text, and preserve contrast.

Avoid decorative purple-blue gradients, glowing orbs, bokeh blobs, generic glassmorphism cards, card nesting, low-contrast text, empty single-card layouts, rainbow UI chrome, and colorful CTA systems. The result should feel like iOS / visionOS / macOS Liquid Glass: clear, tactile, premium, calm, color-rich where content deserves it, and highly legible.
