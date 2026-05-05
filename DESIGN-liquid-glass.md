# DESIGN: Apple Liquid Glass UI

这是一份面向 Stitch / UI 生成工具的设计说明。它结合了 Apple 官网式的克制产品语言与 Apple Liquid Glass 的半透明、折射、悬浮材质。目标是让 Stitch 生成真实可用的 UI，而不是一张介绍玻璃效果的概念海报。

## 一句话设计指令

设计一个 Apple Liquid Glass 风格界面：以 Apple 的产品优先、系统字体、单一蓝色交互、精确留白和 pill 控件为基础，大量使用半透明玻璃浮层、真实 backdrop blur、饱和增强、细腻高光边缘、环境色折射和轻微物理动效。界面要像 iOS / visionOS / macOS 新一代系统 UI：清澈、漂浮、精致、安静、可读。

## 设计气质

- **Apple-like**：产品或核心内容优先，UI chrome 收敛，不堆装饰。
- **Liquid Glass**：导航、浮动条、工具栏、卡片、tab、弹窗、toast 都应像半透明玻璃。
- **Photography / Content First**：背景必须有可被玻璃折射的真实内容，如产品图、仪表盘、地图、图表、媒体画面或应用画布。
- **Single Accent**：所有可点击主动作使用 Apple Action Blue，不引入第二品牌色。
- **Highly Legible**：玻璃效果不能牺牲文字对比度，文字永远清楚。

## Apple 基础规范

### 色彩

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
  --apple-dark-tile-3: #252527;

  --apple-muted-dark: #333333;
  --apple-muted-light: #cccccc;
  --apple-hairline: #e0e0e0;
}
```

使用方式：

- 主 CTA、链接、focus ring：`#0066cc`
- 深色玻璃或深色 tile 上的链接：`#2997ff`
- 正文：`#1d1d1f`
- 亮色画布：`#ffffff` / `#f5f5f7`
- 深色画布：`#272729` / `#000000`
- 不要使用第二强调色，不要使用紫蓝渐变作为主视觉。

### 字体

- Display：`SF Pro Display, system-ui, -apple-system, BlinkMacSystemFont, sans-serif`
- Body / UI：`SF Pro Text, system-ui, -apple-system, BlinkMacSystemFont, sans-serif`
- 标题使用 `600`，正文使用 `400`，强调正文使用 `600`
- body copy 使用 `17px / 1.47`
- 导航和工具文字可使用 `12px - 14px`
- 主要标题建议 `40px - 56px`，移动端降到 `28px - 34px`
- 保持 Apple 式紧凑排版，但 Stitch 生成 CSS 时 letter-spacing 保持 `0`

### Apple 形状语法

- 全屏 product tile：`0px` 圆角，边到边铺满
- 小工具按钮：`8px`
- 玻璃卡片：`16px - 18px`
- 大玻璃浮层 / modal：`24px`
- 主 CTA、搜索框、选项 chip：`9999px` pill
- 圆形图标按钮：`44px × 44px`，圆形

## Liquid Glass 材质规范

Liquid Glass 不是普通毛玻璃。它需要同时具备：透光、折射、边缘高光、环境色、悬浮阴影和物理反馈。

### 材质层级

| 层级 | 用途 | 透明度 | Blur | 阴影 | 感觉 |
|---|---|---:|---:|---|---|
| Glass Chrome | 顶部导航、底部栏、工具栏 | 55% - 68% | 20px - 32px | 极轻 | 薄、贴近画布 |
| Glass Surface | 内容卡片、面板、侧边栏 | 62% - 76% | 16px - 28px | 中等 | 悬浮、可读 |
| Glass Control | 按钮、chip、tab 选中态 | 38% - 58% | 8px - 16px | 轻 | 小玻璃片 |
| Glass Elevated | modal、popover、toast | 72% - 86% | 32px - 48px | 明显 | 厚玻璃、最高层 |
| Glass Scrim | 弹窗遮罩 | 25% - 50% | 40px - 64px | 无 | 背景被抽离 |

### Liquid Glass Tokens

```css
:root {
  color-scheme: light dark;

  --lg-blur-xs: 6px;
  --lg-blur-sm: 10px;
  --lg-blur-md: 20px;
  --lg-blur-lg: 32px;
  --lg-blur-xl: 48px;

  --lg-saturate: 1.8;
  --lg-brightness: 1.03;

  --lg-radius-sm: 8px;
  --lg-radius-md: 12px;
  --lg-radius-lg: 18px;
  --lg-radius-xl: 24px;
  --lg-radius-pill: 9999px;

  --lg-bg-chrome: rgba(255, 255, 255, 0.58);
  --lg-bg-surface: rgba(255, 255, 255, 0.72);
  --lg-bg-elevated: rgba(255, 255, 255, 0.84);
  --lg-bg-control: rgba(255, 255, 255, 0.44);
  --lg-bg-control-hover: rgba(255, 255, 255, 0.58);
  --lg-bg-control-pressed: rgba(255, 255, 255, 0.34);
  --lg-bg-scrim: rgba(0, 0, 0, 0.28);

  --lg-edge-highlight: rgba(255, 255, 255, 0.72);
  --lg-edge-shadow: rgba(0, 0, 0, 0.08);
  --lg-inner-highlight: rgba(255, 255, 255, 0.34);
  --lg-inner-shadow: rgba(0, 0, 0, 0.08);

  --lg-shadow-chrome: 0 1px 0 rgba(255, 255, 255, 0.35);
  --lg-shadow-surface: 0 10px 30px rgba(0, 0, 0, 0.12);
  --lg-shadow-elevated: 0 18px 60px rgba(0, 0, 0, 0.20);

  --lg-text-primary: rgba(29, 29, 31, 0.94);
  --lg-text-secondary: rgba(60, 60, 67, 0.72);
  --lg-accent: #0066cc;
  --lg-accent-focus: #0071e3;

  --lg-duration-fast: 150ms;
  --lg-duration-normal: 300ms;
  --lg-duration-slow: 500ms;
  --lg-easing-spring: cubic-bezier(0.22, 1, 0.36, 1);
}

@media (prefers-color-scheme: dark) {
  :root {
    --lg-bg-chrome: rgba(18, 18, 20, 0.58);
    --lg-bg-surface: rgba(32, 32, 34, 0.66);
    --lg-bg-elevated: rgba(44, 44, 46, 0.78);
    --lg-bg-control: rgba(255, 255, 255, 0.10);
    --lg-bg-control-hover: rgba(255, 255, 255, 0.16);
    --lg-bg-control-pressed: rgba(255, 255, 255, 0.06);
    --lg-bg-scrim: rgba(0, 0, 0, 0.52);

    --lg-edge-highlight: rgba(255, 255, 255, 0.18);
    --lg-edge-shadow: rgba(0, 0, 0, 0.42);
    --lg-inner-highlight: rgba(255, 255, 255, 0.10);
    --lg-inner-shadow: rgba(0, 0, 0, 0.30);

    --lg-shadow-surface: 0 12px 34px rgba(0, 0, 0, 0.42);
    --lg-shadow-elevated: 0 22px 70px rgba(0, 0, 0, 0.58);

    --lg-text-primary: rgba(255, 255, 255, 0.94);
    --lg-text-secondary: rgba(235, 235, 245, 0.68);
    --lg-accent: #2997ff;
    --lg-accent-focus: #0071e3;
  }
}
```

### 通用玻璃基类

Stitch 生成时应尽量让主要浮层具备以下材质逻辑：

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

.liquid-glass::after {
  content: "";
  position: absolute;
  inset: 1px;
  pointer-events: none;
  border-radius: inherit;
  box-shadow:
    inset 0 0 0 1px rgba(255, 255, 255, 0.12),
    inset 0 -10px 24px rgba(255, 255, 255, 0.06);
}
```

这里的 gradient 只用于模拟玻璃高光和边缘折射，不用于做装饰背景。不要生成发光圆球、bokeh 或紫蓝渐变背景。

## 布局与页面结构

### 推荐结构

1. 顶部 **Liquid Glass Global Nav**，高度约 `44px`
2. 下方 **Liquid Glass Sub Nav / Context Bar**，高度约 `52px`
3. 主内容为真实 app / 产品 / 仪表盘 / 媒体画布
4. 关键区域使用 1 - 3 个玻璃浮层：卡片、工具栏、侧边栏、tab、搜索、toast
5. 页面底部或滚动时可出现 **Floating Liquid Glass Action Bar**

### Apple 节奏

- 可使用全屏 light / parchment / dark tile 交替形成节奏
- 色块切换本身就是分隔线，不要额外加粗分割线
- 第一屏要直接呈现产品或可用界面，不要先做解释性 landing
- 背景图或内容画布要边到边，但玻璃浮层可以悬浮在其上
- UI 留白要大，控件间距稳定，避免卡片套卡片

## 组件规范

### Liquid Glass Global Nav

Apple 式 44px 顶部导航，但使用 Liquid Glass 而非纯黑实色。适合 app、dashboard、product page。

```css
.lg-global-nav {
  height: 44px;
  position: sticky;
  top: 0;
  z-index: 200;
  background: var(--lg-bg-chrome);
  backdrop-filter: blur(var(--lg-blur-lg)) saturate(1.8);
  -webkit-backdrop-filter: blur(var(--lg-blur-lg)) saturate(1.8);
  border-bottom: 1px solid var(--lg-edge-shadow);
  color: var(--lg-text-primary);
}
```

设计要求：

- 左侧可放 Apple-like logo 或产品名
- 中间导航文字 `12px`
- 右侧使用搜索、用户、购物袋或设置图标
- 移动端折叠为 logo + menu + primary action

### Liquid Glass Sub Nav

适合产品分类、当前页面标题、右侧 CTA。

```css
.lg-sub-nav {
  height: 52px;
  background: rgba(245, 245, 247, 0.72);
  backdrop-filter: blur(24px) saturate(180%);
  -webkit-backdrop-filter: blur(24px) saturate(180%);
  border-bottom: 1px solid rgba(0, 0, 0, 0.06);
}
```

右侧 CTA 使用 Action Blue pill，不要换色。

### Liquid Glass Card

用于主内容卡片。比普通 Apple utility card 更通透，有明显玻璃边缘。

```css
.lg-card {
  border-radius: var(--lg-radius-lg);
  padding: 24px;
  background: var(--lg-bg-surface);
  backdrop-filter: blur(var(--lg-blur-md)) saturate(var(--lg-saturate));
  -webkit-backdrop-filter: blur(var(--lg-blur-md)) saturate(var(--lg-saturate));
  border: 1px solid var(--lg-edge-highlight);
  box-shadow:
    inset 0 1px 0 var(--lg-inner-highlight),
    var(--lg-shadow-surface);
  transition: transform var(--lg-duration-normal) var(--lg-easing-spring),
    box-shadow var(--lg-duration-normal) var(--lg-easing-spring);
}

.lg-card:hover {
  transform: translateY(-2px);
  box-shadow:
    inset 0 1px 0 var(--lg-inner-highlight),
    var(--lg-shadow-elevated);
}
```

### Liquid Glass Primary Button

Apple CTA 仍然是蓝色 pill，但可以带一层玻璃高光。

```css
.lg-button-primary {
  min-height: 44px;
  padding: 11px 22px;
  border-radius: var(--lg-radius-pill);
  background:
    linear-gradient(180deg, rgba(255, 255, 255, 0.24), transparent 42%),
    var(--apple-blue);
  color: #fff;
  border: 1px solid rgba(255, 255, 255, 0.20);
  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.30),
    0 8px 20px rgba(0, 102, 204, 0.24);
  transition: transform var(--lg-duration-fast) var(--lg-easing-spring);
}

.lg-button-primary:active {
  transform: scale(0.95);
}
```

### Liquid Glass Secondary Button / Icon Button

```css
.lg-button-glass {
  min-height: 44px;
  padding: 10px 16px;
  border-radius: var(--lg-radius-pill);
  background: var(--lg-bg-control);
  backdrop-filter: blur(var(--lg-blur-sm)) saturate(var(--lg-saturate));
  -webkit-backdrop-filter: blur(var(--lg-blur-sm)) saturate(var(--lg-saturate));
  border: 1px solid var(--lg-edge-highlight);
  color: var(--lg-text-primary);
}

.lg-icon-button {
  width: 44px;
  height: 44px;
  border-radius: 9999px;
  display: grid;
  place-items: center;
  background: var(--lg-bg-control);
  backdrop-filter: blur(var(--lg-blur-sm)) saturate(var(--lg-saturate));
  -webkit-backdrop-filter: blur(var(--lg-blur-sm)) saturate(var(--lg-saturate));
  border: 1px solid var(--lg-edge-highlight);
}
```

图标按钮优先使用符号或图标，不要用文字矩形替代常见动作。

### Liquid Glass Segmented Control

选中项像一片更亮的小玻璃浮在玻璃底座上。

```css
.lg-segmented {
  display: flex;
  padding: 4px;
  gap: 0;
  border-radius: var(--lg-radius-pill);
  background: var(--lg-bg-control);
  backdrop-filter: blur(var(--lg-blur-md)) saturate(var(--lg-saturate));
  -webkit-backdrop-filter: blur(var(--lg-blur-md)) saturate(var(--lg-saturate));
  border: 1px solid var(--lg-edge-highlight);
}

.lg-segment[aria-selected="true"] {
  background: var(--lg-bg-elevated);
  border-radius: var(--lg-radius-pill);
  box-shadow:
    inset 0 1px 0 var(--lg-inner-highlight),
    0 6px 18px rgba(0, 0, 0, 0.10);
}
```

### Floating Liquid Glass Action Bar

适合底部购买栏、播放控制、编辑工具栏、AI 输入栏。

```css
.lg-floating-bar {
  position: fixed;
  left: 50%;
  bottom: 24px;
  transform: translateX(-50%);
  min-height: 64px;
  padding: 10px 16px;
  border-radius: var(--lg-radius-pill);
  background: var(--lg-bg-chrome);
  backdrop-filter: blur(var(--lg-blur-lg)) saturate(var(--lg-saturate));
  -webkit-backdrop-filter: blur(var(--lg-blur-lg)) saturate(var(--lg-saturate));
  border: 1px solid var(--lg-edge-highlight);
  box-shadow:
    inset 0 1px 0 var(--lg-inner-highlight),
    var(--lg-shadow-elevated);
}
```

### Liquid Glass Modal / Popover

最高层玻璃，更厚、更清晰。

```css
.lg-overlay {
  background: var(--lg-bg-scrim);
  backdrop-filter: blur(var(--lg-blur-xl)) saturate(1.4);
  -webkit-backdrop-filter: blur(var(--lg-blur-xl)) saturate(1.4);
}

.lg-modal {
  border-radius: var(--lg-radius-xl);
  background: var(--lg-bg-elevated);
  backdrop-filter: blur(var(--lg-blur-xl)) saturate(var(--lg-saturate));
  -webkit-backdrop-filter: blur(var(--lg-blur-xl)) saturate(var(--lg-saturate));
  border: 1px solid var(--lg-edge-highlight);
  box-shadow:
    inset 0 1px 0 var(--lg-inner-highlight),
    var(--lg-shadow-elevated);
}
```

### Liquid Glass Search

```css
.lg-search {
  height: 44px;
  border-radius: var(--lg-radius-pill);
  padding: 0 18px;
  background: var(--lg-bg-control);
  backdrop-filter: blur(var(--lg-blur-md)) saturate(var(--lg-saturate));
  -webkit-backdrop-filter: blur(var(--lg-blur-md)) saturate(var(--lg-saturate));
  border: 1px solid var(--lg-edge-highlight);
  color: var(--lg-text-primary);
}
```

### Liquid Glass Toast

```css
.lg-toast {
  padding: 12px 20px;
  border-radius: var(--lg-radius-lg);
  background: var(--lg-bg-elevated);
  backdrop-filter: blur(var(--lg-blur-lg)) saturate(var(--lg-saturate));
  -webkit-backdrop-filter: blur(var(--lg-blur-lg)) saturate(var(--lg-saturate));
  border: 1px solid var(--lg-edge-highlight);
  box-shadow: var(--lg-shadow-elevated);
}
```

## Product / App 背景规则

为了让 Liquid Glass 有真实效果，背景不能是空白。优先使用：

- 产品摄影或产品渲染图
- dashboard 图表与数据面板
- 地图、媒体播放器、日历、设计画布
- 轻量内容网格
- light / parchment / dark tile 交替背景

避免：

- 纯装饰渐变
- 紫蓝发光圆球
- 无意义 bokeh
- 只有一张空白卡片的页面
- 大量互相嵌套的玻璃卡片

## 动效

Liquid Glass 动效应像轻质玻璃片：

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
  animation: lg-enter var(--lg-duration-normal) var(--lg-easing-spring) both;
}
```

交互状态：

- hover：玻璃稍亮，阴影稍深，上浮 `1px - 2px`
- active：按钮缩放到 `0.95 - 0.97`
- focus：使用 `2px solid #0071e3`
- modal enter：淡入 + 轻微上移 + 缩放
- reduced motion：取消弹簧和位移

## 可访问性

```css
@media (prefers-reduced-motion: reduce) {
  .lg-animate-in,
  .lg-card,
  .lg-button-primary,
  .lg-button-glass {
    animation: none;
    transition: none;
  }
}

@media (prefers-contrast: more) {
  .liquid-glass,
  .lg-card,
  .lg-modal,
  .lg-floating-bar {
    backdrop-filter: none;
    -webkit-backdrop-filter: none;
    background: rgba(255, 255, 255, 0.96);
    border: 2px solid var(--apple-ink);
  }
}
```

要求：

- 玻璃面文字对比度目标 `4.5:1`
- 复杂背景上的文字要有局部垫层或更厚玻璃
- 所有按钮和输入都有 focus state
- 移动端 touch target 不小于 `44px × 44px`

## 移动端与性能

- 同屏大面积 blur 控制在 3 个以内
- 滚动列表里不要每个 item 都使用强 blur
- 移动端可把 `blur(32px)` 降为 `blur(16px)`
- 低端设备可用半透明实色替代 blur
- `will-change: transform` 只在动画期间使用
- 主要性能成本来自 `blur()`，`saturate()` 可以保留

## Stitch 生成硬性要求

- 第一屏必须是可用产品 UI，不是说明页。
- 必须明显使用 Liquid Glass：导航、浮动条、卡片、tab、搜索、modal、toast 至少选择 4 类。
- 必须出现 Apple Action Blue 作为唯一主交互色。
- 必须有真实背景内容支撑玻璃折射，不要空白背景。
- 必须使用 Apple-like 系统字体、pill CTA、44px touch target。
- 必须保留清晰层级：薄玻璃 chrome、中等玻璃 surface、厚玻璃 elevated。
- 不要让每个 section 都变成卡片，不要卡片套卡片。
- 不要使用装饰性紫蓝渐变、发光圆球、bokeh。
- 不要牺牲可读性，玻璃上的文字必须清楚。

## 可直接粘贴给 Stitch 的 Prompt

Create a polished Apple Liquid Glass UI. The design should combine Apple's quiet product-first language with modern Liquid Glass surfaces: translucent floating chrome, realistic backdrop blur, saturation boost, edge highlights, subtle inner refraction, soft environmental tint, and restrained spring interactions.

Use Apple-like system typography, 17px body text, 600-weight headings, generous spacing, pill-shaped blue CTAs, 44px touch targets, and a single interaction color: Apple Action Blue (#0066cc, or #2997ff on dark glass). The first screen must be a usable app/product interface, not a documentation or marketing explanation page.

Use Liquid Glass heavily but intelligently: top navigation, contextual sub-nav, floating action bar, content cards, segmented controls, search input, modal/popover, icon buttons, and toast notifications should feel like layered glass. Show clear material hierarchy: thin glass chrome, medium glass surfaces, thick elevated glass. Include a rich background canvas such as product imagery, dashboard content, map, media, chart, or app workspace so the glass can blur and refract real content.

Avoid decorative purple-blue gradients, glowing orbs, bokeh blobs, generic glassmorphism cards, card nesting, low-contrast text, and empty single-card layouts. The result should feel like iOS / visionOS / macOS Liquid Glass: clear, tactile, premium, calm, and highly legible.

