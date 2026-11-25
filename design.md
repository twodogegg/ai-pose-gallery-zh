---
version: alpha
name: POSE & CHARACTER Atlas
description: AI 角色参考图库与合成工作台的设计系统 —— 暖黑与和纸双主题、单一琥珀/陶土强调、衬线标题、发丝线与玻璃（对齐 plot18.com 的 Night Craft / Japanese Minimal）。
---

# POSE & CHARACTER Atlas Design System

本文件是这个站点的视觉语言单一真相源。它是给人和编码 Agent 共读的：散文部分解释为什么这样长，YAML 令牌是规范性数值。

品牌基准是 [plot18.com](https://www.plot18.com/)：暖黑底、琥珀金强调、衬线大标题、玻璃层次、18px 点阵。这个站点不复制它的消费端页面结构（hero 轮播、商品区、创作者区），只把它的**视觉语法**移植到「参考图库 + 合成工作台」：

- **暗色 = Night Craft**：`#101114` 暖黑底、`#E8A532` 琥珀、`#F3F0E8` 米白正文。
- **亮色 = Japanese Minimal**：`#F7F1E8` 和纸底、`#B34631` 陶土红、`#302821` 墨色正文。
- 两种模式跟随系统明暗切换；暗色是品牌主态。亮色不是暗色反相，而是另一套来自 plot18 的和纸配色。

代码落点：

- 颜色 / 圆角 / 字体 / 玻璃 / 纸纹：`src/index.css` 的 `:root`（亮色）与 `.dark`（暗色）
- 组件：`src/components/` 与 `src/routes/`（Tailwind 4 + shadcn/ui）
- 通知：`src/notifications.css`（Sonner）
- ComfyUI 插件弹窗：`comfyui_plugin/web/style.css`（独立表面，但也跟随本文件的品牌令牌）

运行时唯一真相源仍是 `src/index.css` 的 CSS 变量。本文是它的描述；改视觉时两边一起改。

## Overview

这个站点是「三库 + 一台」：姿势 / 服装 / 人像三个参考图库，加一个 AI 角色合成台。页面里最多的东西是图片，不是 UI。因此视觉策略是 **图片承担颜色，界面退到暖色单声道**：

- **图片是唯一的彩色来源**。姿势白模、深度图、服装、人像本身已经提供了全部色相；界面只保留一个强调色（暗琥珀 / 亮陶土）和一组状态色。不要给卡片、标签、图表再加装饰性色相。
- **暖色单声道**。页面、侧栏、面板、卡片全部是带暖调的灰阶。层次靠 `--surface` / `--surface-raised` / `--surface-sunken` 的明度差和 `1px` 暖色半透明边框建立，不靠大面积灰色块。
- **衬线只做标题**。页面标题、区块标题、精选卡标题、统计数字用 Georgia / 宋体（`--font-display`）；正文、按钮、筛选器、图片元信息一律 Inter 无衬线。衬线是 plot18 最明显的品牌特征，但它不进入控件和密集列表。
- **玻璃只给浮层**。合成台的配方栏、弹层、通知、侧栏边缘可以用玻璃；图库卡片和图片格不用，几十张图逐个 `backdrop-filter` 会掉帧。
- **18px 点阵**。页面背景、侧栏顶部、空状态铺 18px 点阵；暗色用琥珀 13% 透明度，亮色用墨色 10%。点阵是环境纹理，不能盖过图片。
- **紧凑**。控件 34px、图标按钮 34×34、紧凑按钮 30px、移动端点按 44px；正文 12–14px，标签 10–11px，图片角标 9–10px。这是参考工具，不是营销页。

情绪目标：暗房、接触印相、纸本目录。安静、精确、可以连续翻几百张图不累。

## Colors

两套主题都从 plot18 的 CSS 令牌取值，并做一处可访问性修正：

- **暗色 Night Craft**：页面 `#101114`，表面 `#1B1B1D`，抬起 `#222224`，正文 `#F3F0E8`，次要文字 `#9F988B`，琥珀 `#E8A532`，危险 `#E46C5B`。
- **亮色 Japanese Minimal**：页面 `#F7F1E8`，表面 `#FFFDF8`，抬起 `#FAF5ED`，正文 `#302821`，次要文字 `#78695D`，陶土红 `#B34631`，危险 `#B94A43`。
- **陶土的修正**：plot18 亮色的 `#C8553D` 在米白底上做正文只有 3.9:1，不达 WCAG AA。这里取同色系加深到 `#B34631`（正文 5.3:1、米白字按钮 5.3:1），暗色仍用原值 `#E8A532`。不要为了“更像参考站”改回 `#C8553D`。

语义令牌全部来自 `src/index.css` 的 `:root`，暗色在 `.dark` 覆盖同名变量。shadcn 组件继续用 `bg-card`、`text-muted-foreground`、`bg-primary` 这些工具类；**不要另建一套平行令牌**。

**暗色覆盖值**（`.dark`）：

| 令牌 | 暗色值 |
| --- | --- |
| `--background` | `#101114` |
| `--foreground` | `#F3F0E8` |
| `--card` | `#1B1B1D` |
| `--card-foreground` | `#F3F0E8` |
| `--popover` | `#222224` |
| `--popover-foreground` | `#F3F0E8` |
| `--primary` | `#E8A532` |
| `--primary-foreground` | `#17110A` |
| `--secondary` | `#2A2A2C` |
| `--secondary-foreground` | `#F3F0E8` |
| `--muted` | `#171719` |
| `--muted-foreground` | `#9F988B` |
| `--accent` | `#E8A532` |
| `--accent-foreground` | `#17110A` |
| `--destructive` | `#E46C5B` |
| `--border` | `rgb(243 240 232 / .15)` |
| `--input` | `rgb(243 240 232 / .18)` |
| `--ring` | `#E8A532` |
| `--sidebar` | `#171719` |
| `--sidebar-foreground` | `#F3F0E8` |
| `--sidebar-accent` | `#222224` |
| `--sidebar-accent-foreground` | `#F3F0E8` |
| `--chart-1…5` | `#F3F0E8` `#D0C9BA` `#9F988B` `#E8A532` `#746F66` |

**shadcn 令牌映射**（从现在的值迁到新值）：

| 令牌 | 亮色新值 | 暗色新值 |
| --- | --- | --- |
| `--background` | `#F7F1E8` | `#101114` |
| `--foreground` | `#302821` | `#F3F0E8` |
| `--card` | `#FFFDF8` | `#1B1B1D` |
| `--popover` | `#FFFDF8` | `#222224` |
| `--primary` | `#B34631` | `#E8A532` |
| `--primary-foreground` | `#FFFAF2` | `#17110A` |
| `--secondary` | `#F2E8DA` | `#2A2A2C` |
| `--muted` | `#F2E8DA` | `#171719` |
| `--muted-foreground` | `#78695D` | `#9F988B` |
| `--accent` | `#B34631` | `#E8A532` |
| `--accent-foreground` | `#FFFAF2` | `#17110A` |
| `--destructive` | `#B94A43` | `#E46C5B` |
| `--border` | `rgb(48 40 33 / .17)` | `rgb(243 240 232 / .15)` |
| `--input` | `rgb(48 40 33 / .22)` | `rgb(243 240 232 / .18)` |
| `--ring` | `#B34631` | `#E8A532` |
| `--sidebar` | `#FAF5ED` | `#171719` |
| `--sidebar-accent` | `#F2E8DA` | `#222224` |

颜色角色：

- **Background / Foreground**：页面底色与正文。亮色不是纯白，暗色不是纯黑；都带暖调。
- **Card / Popover**：图库卡、面板、弹层、侧栏。亮色 `#FFFDF8`，暗色 `#1B1B1D`；抬起面（`--surface-raised`）亮 `#FAF5ED`、暗 `#222224`，用于玻璃和精选卡。
- **Muted / Muted Foreground**：下沉块、输入框底、禁用面与次要文字。**不要再用大面积 `bg-muted` 当面板底**——面板改为 `card` + `1px border`，muted 只做内嵌井和悬停。
- **Primary / Accent**：主按钮、激活筛选、选中端口、强调文字、焦点环。亮色 `#B34631`，暗色 `#E8A532`；两者同值，语义不同。
- **Border / Input**：所有发丝线。半透明暖墨/暖白，叠在不同表面上会自然变深或变浅；不要用不透明灰。
- **Destructive**：删除、错误、18+ 解锁提示。实心危险按钮用 `#B94A43`（亮）/`#C14B3D`（暗）保证米白字对比度。
- **状态色**：成功 `#3F714D`（亮，plot18 `#4B855B` 的 AA 加深档）/`#96C87A`（暗）、警告 `#99651D`（亮）/`#F0BC5D`（暗）、信息 `#416F69`（亮，plot18 `#477B74` 的 AA 加深档）/`#82B7C9`（暗）。只用于“安全模式 / 已连接 / 18+ / 热门 / 未配置”这类状态，不做装饰。状态徽标用 10% 底色 + 状态文字色；实心圆点只放图标，不放 10px 白字。
- **图片角标**：图片上的 ID / 角度 / 排名用 `rgb(0 0 0 / .70)` 底 + 米白字，或 `rgb(255 255 255 / .90)` 底 + 墨字；这是图片内部的反差，不属于主题色。
- **图表 1–5**：暖灰阶梯 + 一个强调色，不引入第二个色相。

```yaml
colors:
  background: "#f7f1e8"
  foreground: "#302821"
  card: "#fffdf8"
  card-foreground: "#302821"
  popover: "#fffdf8"
  popover-foreground: "#302821"
  primary: "#b34631"
  primary-foreground: "#fffaf2"
  secondary: "#f2e8da"
  secondary-foreground: "#302821"
  muted: "#f2e8da"
  muted-foreground: "#78695d"
  accent: "#b34631"
  accent-foreground: "#fffaf2"
  destructive: "#b94a43"
  border: "rgb(48 40 33 / .17)"
  ring: "#b34631"
  sidebar: "#faf5ed"
  sidebar-foreground: "#302821"
  sidebar-accent: "#f2e8da"
  sidebar-accent-foreground: "#302821"
  success: "#3f714d"
  success-soft: "#eaf1ea"
  warning: "#99651d"
  warning-soft: "#faf6ec"
  info: "#416f69"
  info-soft: "#e9f0ee"
  overlay-ink: "rgb(0 0 0 / .70)"
  overlay-paper: "#fffaf2"
  overlay-light: "rgb(255 250 242 / .90)"
  overlay-light-foreground: "#17110a"
  chart-1: "#302821"
  chart-2: "#78695d"
  chart-3: "#99887a"
  chart-4: "#b34631"
  chart-5: "#dfcdb8"
```

**玻璃配方**（暗色；亮色退化成实心和纸面）：

```css
--glass-bg: rgb(16 17 20 / .52);
--glass-bg-strong: rgb(16 17 20 / .66);
--glass-border: rgb(243 240 232 / .24);
--glass-highlight: rgb(243 240 232 / .20);
--glass-filter: blur(22px) saturate(132%);
--glass-shadow: 0 18px 48px rgb(16 17 20 / .32);
--glass-shadow-strong: 0 24px 68px rgb(16 17 20 / .44);

/* 亮色 */
--glass-bg: var(--card);
--glass-filter: none;
--glass-border: var(--border);
```

**阴影与微光**：

```css
/* 暗色 */
--shadow: inset 1px 1px 0 var(--glass-highlight),
          inset -1px -1px 0 rgb(16 17 20 / .44),
          0 18px 48px rgb(16 17 20 / .24);
--shadow-strong: 0 24px 68px rgb(16 17 20 / .44);
--glow: 0 0 12px rgb(232 165 50 / .20);

/* 亮色 */
--shadow: 0 1px 0 rgb(255 249 232 / .44),
          0 2px 1px rgb(105 93 72 / .08),
          0 11px 25px rgb(48 40 33 / .11);
--shadow-strong: 0 2px 0 rgb(105 93 72 / .10),
                 0 15px 32px rgb(48 40 33 / .14);
--glow: 0 0 12px rgb(179 70 49 / .16);
```

**和纸纸纹**（亮色页面背景、空状态、精选卡；不要铺在图片和表单上）：

```css
--paper-texture:
  repeating-linear-gradient(7deg, transparent 0 12px, rgb(129 109 78 / .08) 12px 12.7px, transparent 13px 29px),
  repeating-linear-gradient(174deg, transparent 0 18px, rgb(255 244 217 / .20) 18px 18.8px, transparent 19px 41px),
  repeating-linear-gradient(21deg, transparent 0 37px, rgb(139 116 81 / .05) 37px 37.6px, transparent 38px 71px);
--paper-size: 89px 53px, 113px 67px, 149px 83px;
```

**点阵**：

```css
/* 暗色 */
background-image: radial-gradient(circle, rgb(232 165 50 / .13) 1px, transparent 1.1px);
background-size: 18px 18px;
/* 亮色 */
background-image: radial-gradient(circle, rgb(48 40 33 / .10) 1px, transparent 1.1px);
background-size: 18px 18px;
```

强调色纪律：

- 每个模式只有一个强调色相。状态色（成功/警告/信息/危险）只表达状态，不参与品牌装饰。
- 强调色不要铺大面积背景。它出现在按钮、激活筛选、1px 描边、图标、短横线、微光里；大面积用 surface 和纸纹。
- 发光是强调色的低透明度光晕，不是彩色阴影。只有“激活 / 选中 / 悬停抬升”三种状态允许发光。
- 图片上的角标保持中性黑/白，不染成琥珀；图片本身已经有颜色，角标再染色会脏。

## Typography

两套字族：**Inter Variable** 做 UI 与正文（泰文按 unicode-range 加载 `Noto Sans Thai Variable`，中文回退 `PingFang SC` → `Hiragino Sans GB` → `Microsoft YaHei` → `Noto Sans SC`），**Georgia → Songti SC** 做标题、统计数字和字标。

- **Display（38.4 / 33.28px，700，衬线）**：只用于首页/指南的 hero 级标题。字距 `-1.344px`，行高 1.05；中文必须负字距。
- **Headline（24 / 22 / 18px，700，衬线）**：页面标题（桌面 24、手机 18）、区块标题（22）、弹层标题（18）。衬线低于 18px 会糊，所以 18px 是衬线下限。
- **Title Serif（17.28px，700，衬线）**：精选卡、图库卡标题。这是密集卡片里能接受的最小衬线尺寸；再小退回无衬线。
- **Title Sans（14px，600，无衬线）**：合成台面板标题、设置项标题、需要和正文同族的卡片标题。
- **Body（14 / 12px，400，无衬线）**：14px 是说明文字和表单正文；12px 是图库元信息、筛选器、灯箱侧栏、画布式密集 UI 的默认值。
- **Label（11 / 10px，500 / 800，无衬线）**：11px 是按钮小字、标签、元信息；10px 是 eyebrow（大写、字距 1.2px）和图片角标。
- **Micro（9px，500）**：只用于姿势五角度缩略条。不要用在任何可读正文里。
- **Mono（12 / 10px）**：图片 ID、提示词、模型名、JSON。用 `ui-monospace` 栈，不加第二套等宽字体。

字重只使用 `400 / 500 / 600 / 700 / 800`。衬线只用 700（Georgia 没有更细的可用档，写 800 会触发伪粗体）。泰文行高统一不低于 1.6；中文标题用负字距，正文不要负字距。

```yaml
typography:
  display-lg:
    fontFamily: 'Georgia, "Songti SC", "Noto Serif SC", serif'
    fontSize: 38.4px
    fontWeight: 700
    lineHeight: 1.05
    letterSpacing: -1.344px
  display-md:
    fontFamily: 'Georgia, "Songti SC", "Noto Serif SC", serif'
    fontSize: 33.28px
    fontWeight: 700
    lineHeight: 1.05
    letterSpacing: -1.331px
  headline-lg:
    fontFamily: 'Georgia, "Songti SC", "Noto Serif SC", serif'
    fontSize: 24px
    fontWeight: 700
    lineHeight: 1.15
    letterSpacing: -0.5px
  headline-md:
    fontFamily: 'Georgia, "Songti SC", "Noto Serif SC", serif'
    fontSize: 22px
    fontWeight: 700
    lineHeight: 1.2
    letterSpacing: -0.4px
  headline-sm:
    fontFamily: 'Georgia, "Songti SC", "Noto Serif SC", serif'
    fontSize: 18px
    fontWeight: 700
    lineHeight: 1.25
    letterSpacing: -0.3px
  title-serif:
    fontFamily: 'Georgia, "Songti SC", "Noto Serif SC", serif'
    fontSize: 17.28px
    fontWeight: 700
    lineHeight: 1.2
    letterSpacing: 0
  title-sans:
    fontFamily: Inter Variable
    fontSize: 14px
    fontWeight: 600
    lineHeight: 1.35
    letterSpacing: -0.1px
  body-lg:
    fontFamily: Inter Variable
    fontSize: 14px
    fontWeight: 400
    lineHeight: 1.7
  body-md:
    fontFamily: Inter Variable
    fontSize: 12px
    fontWeight: 400
    lineHeight: 1.7
  label-md:
    fontFamily: Inter Variable
    fontSize: 11px
    fontWeight: 500
    lineHeight: 1.5
    letterSpacing: 0.2px
  label-sm:
    fontFamily: Inter Variable
    fontSize: 10px
    fontWeight: 800
    lineHeight: 1.4
    letterSpacing: 1.2px
  micro:
    fontFamily: Inter Variable
    fontSize: 9px
    fontWeight: 500
    lineHeight: 1.2
  mono-md:
    fontFamily: ui-monospace
    fontSize: 12px
    fontWeight: 400
    lineHeight: 1.6
  mono-sm:
    fontFamily: ui-monospace
    fontSize: 10px
    fontWeight: 400
    lineHeight: 1.4
  stat-value:
    fontFamily: 'Georgia, "Songti SC", "Noto Serif SC", serif'
    fontSize: 28px
    fontWeight: 700
    lineHeight: 1
    letterSpacing: -0.7px
  brand:
    fontFamily: 'Georgia, "Songti SC", "Noto Serif SC", serif'
    fontSize: 15px
    fontWeight: 700
    lineHeight: 1
    letterSpacing: -0.3px
```

字号阶梯窄而密：最小 9px（只限角度条），常规最小 10px，最大 38.4px，主体集中在 12–14px。不要新增 13px 这种半档；用 12 或 14。

## Layout

桌面使用侧栏 + 主内容，最小宽度 960px；手机（≤767px）侧栏收成抽屉，图片网格降为 2 列。

外壳结构：

- **侧栏** 256px（`16rem`），收起 48px（`3rem`），手机抽屉 288px（`18rem`）。无右边框，底色 `--sidebar`，靠表面明度差和主内容分开。导航项 34px 高、4px 圆角、14px 字；激活项是药丸：`--accent` 10% 底 + `--accent` 字 + 1px `--accent` 52% 描边 + `var(--glow)`。
- **顶栏** 56px 高，左右 16px，底部 `1px solid var(--border)`，底色 `--background`（暗色可半透明玻璃）。左侧面包屑 12px；右侧状态按钮、随机按钮、合成台主按钮、语言、主题。
- **主内容** 最大宽 1352px，左右 16 / 24 / 32px（sm / md），上 16px、下 40px，区块 gap 16–24px。
- **二级栏 / 筛选栏** 44px 高；筛选药丸 `padding: 6px 14px`、全圆、14px / 500；激活用实心 `--primary` + `--primary-foreground`，未激活用 `--muted` + `--muted-foreground`。
- **合成台** 12 栏：左 7 栏（参考图、模型参数、提示词），右 5 栏（生成结果）；间距 16–24px。
- **配方栏** 停在底部 12px，最大宽 1024px，玻璃 + 8px 圆角 + `var(--shadow-strong)`；在合成台、设置、指南页隐藏。
- **图库网格**：姿势 / 服装 / 人像 2 → 3 → 4 → 5 列（sm / md / lg）；单张模式 2 → 3 → 4 → 6 列。卡片间距 16px。姿势 3:4、服装 2:3、人像 4:5、库入口 3:4、生成历史 3:4，保持素材原始裁切。

间距节奏：2 / 4 / 6 / 8 / 12 / 18 / 24 / 40 / 64。**用 `gap`，不用 `space-x-*` / `space-y-*`**。

控件高度是硬约定：`--control-height` 34px（输入、按钮、图标按钮），紧凑按钮 30px，移动端点按高度 44px，顶栏 56px，筛选栏 44px，侧栏项 34px。不要为了“更精致”把控件压到 28px 以下。

```yaml
spacing:
  unit: 2px
  xxs: 4px
  xs: 6px
  sm: 8px
  md: 12px
  lg: 18px
  xl: 24px
  xxl: 40px
  section: 64px
  page-gutter: 46px
  content-max: 1352px
  header-height: 56px
  subnav-height: 44px
  control-height: 34px
  control-sm: 30px
  touch-target: 44px
  sidebar-width: 256px
  sidebar-icon-width: 48px
  dock-bottom: 12px
  gallery-gap: 16px
```

## Elevation & Depth

深度用 **边框 + 明度 + 一层玻璃** 表达，阴影是例外而非常规。

- **普通面板、图库卡、统计卡、侧栏**：`--card` 底 + `1px solid var(--border)`，无阴影。悬停时边框转 `--accent` 40%，并加 `var(--glow)`；不要靠放大阴影做反馈。
- **浮层（弹层 / 下拉 / 灯箱 / 通知 / 配方栏）**：`--popover` 或 `--surface-raised` 底 + 1px `--glass-border` + `var(--glass-filter)` + `var(--shadow-strong)`。暗色是真正的玻璃；亮色退化成实心和纸面。
- **侧栏**：暗色可用 `--glass-bg` + `backdrop-filter: blur(14px)`；亮色用实心 `--sidebar` + 纸纹。不要给图库网格加玻璃。
- **选中态**：`box-shadow: inset 0 0 0 1px var(--accent), var(--glow)`。图片被选中时用 2px `--accent` 描边 + 右下角实心状态圆点，不要彩色发光铺满整张图。
- **层级顺序**：图库卡 < 配方栏 < 弹层 / 灯箱 < 通知。不要在浮层上手动写 `z-index`，交给 Radix Portal 的顺序。
- **过渡**：暗色 `--motion-fast: .14s`、`--motion-base: .20s`；亮色 `.13s` / `.19s`。悬停只动 `border-color` / `color` / `box-shadow` / `transform`；图片缩放 300ms；按下反馈 `scale(.98)` 或 `translate-y-px`。`prefers-reduced-motion` 和 `[data-gallery-motion=off]` 下全部关掉。

阴影只有四档，不要给单个组件另造第五套：

```css
--shadow:        图库卡悬停、下拉、配方栏
--shadow-strong: 弹层、灯箱、通知
--glow:          激活、选中、悬停抬升
--shadow-inset:  inset 0 0 0 1px var(--accent)（选中描边）
```

## Shapes

形状语言是 **小圆角 + 药丸强调**。两种模式共用同一套阶梯，避免切换明暗时布局跳动：

- 图片井 / 缩略图：4px。
- 控件级（按钮、输入、图标按钮、下拉项）：4px。
- 卡片级（图库卡、统计卡、面板、合成台区块）：6px。
- 大块级（弹层、灯箱、配方栏、精选卡）：8px。
- 全圆：筛选药丸、状态徽标、主按钮、配方槽、图片上的“加入配方”按钮。这是 plot18 最明显的形状特征；普通卡片不要用全圆。
- 图片角标：2px（ID / 角度 / 排名），不要跟着卡片用 6px。
- 国旗：2px，保持硬边。
- 头像 / 状态圆点 / 选中勾：全圆。

```yaml
rounded:
  none: 0px
  xs: 2px
  sm: 4px
  md: 6px
  lg: 8px
  xl: 12px
  2xl: 16px
  full: 999px
```

## Components

### App Shell & Sidebar

- 侧栏 256px，`--sidebar` 底，`--sidebar-foreground` 字；无右边框，靠明度差分开。暗色可加 `backdrop-filter: blur(14px)` 和 18px 点阵。
- 品牌区：32×32 圆角 6px 的 `--primary` 方块 + 琥珀/陶土图标；标题 `brand` 衬线 15px，副标题 10px `--muted-foreground`。句点用 `--accent`。
- 分组标签：10px / 800 / 字距 1.2px / 大写 / `--muted-foreground`；不要用彩色。
- 导航项：34px 高、4px 圆角、14px / 500、图标 16px。悬停 `--surface-hover`；激活 `--sidebar-accent` 底 + `--accent` 字 + 1px `--accent` 40% 描边 + `var(--glow)`。右侧计数用 10px 药丸。
- 侧栏底部：角色配方卡（`--surface` + 1px border + 6px 圆角）显示 `3/3`；GitHub 链接 11px `--muted-foreground`。
- 手机：侧栏变 Sheet 抽屉，触发按钮在顶栏左侧；抽屉宽 288px，打开时背景压暗 `rgb(0 0 0 / .5)`。

### Header & Breadcrumb

- 56px 高，`--background` 底 + 底部 1px `--border`；暗色可用 `--glass-bg` + blur。
- 面包屑 12px `--muted-foreground`，当前页 `--foreground`；分隔符用 `/` 或 Lucide `ChevronRight` 12px。
- 右侧按钮顺序：安全模式 / 18+ 状态 → 随机捏人 → AI 合成台（主按钮）→ 语言 → 主题。
- 安全模式按钮：成功状态用 `--success-soft` 底 + `--success` 字 + 1px `--success` 40% 描边；18+ 状态用危险色同构。按钮高 30px、全圆、10px / 600。
- 主按钮：琥珀/陶土实心、全圆、30px 高、`--primary-foreground` 字；计数徽标用 `color-mix(in srgb, var(--primary-foreground) 20%, transparent)` 底 + 10px 字。
- 语言切换：32px 正方形、4px 圆角、国旗 20×14px、2px 圆角；激活 `--surface-hover` + 1px `--accent` 40% 描边。主题按钮 34px 图标按钮。

### Page Header

- 标题 `headline-lg`（桌面 24px / 手机 18px）衬线，`--foreground`，字距 `-0.5px`；描述 12px `--muted-foreground`，与标题 gap 4px。
- 右侧操作区可换行，按钮 30–34px；不要给页面标题加大面积底色或下划线。

### Stat Card

- `--card` 底 + 1px `--border` + 6px 圆角 + `padding: 16px`；最小高 96px。**不要再用 `bg-muted` 大灰块**。
- 标题：10px / 800 / 大写 / 字距 1.2px / `--muted-foreground`；右上角图标 24px 圆、`--surface-sunken` 底、`--accent` 图标。
- 数值：`stat-value` 衬线 28px / 700 / 字距 -0.7px；描述 11px `--muted-foreground`。
- 悬停：边框 `--accent` 40%、加 `var(--glow)`，不位移。

### Section Panel

- `--card` 底 + 1px `--border` + 6px 圆角，`padding: 16px`（手机）/ 24px（桌面）。
- 标题 `headline-md` 衬线 22px（桌面）/ 18px（手机）；右侧操作区 30px 按钮。
- 内容区默认 2 栏（`@container`），合成台区块在窄容器强制 1 栏；间距 12–16px。
- 面板里不再套一层 muted 面板；需要下沉时用 `--surface-sunken` 的单块，不要“面板套面板”。

### Library Card（三库入口）

- `--card` + 1px border + 6px 圆角 + `padding: 16px`；标题 14px / 600 无衬线 + 计数药丸。
- 四张 3:4 缩略图铺成 4 列、gap 6px、圆角 4px、`--surface-sunken` 底；图片 `object-fit: cover`。
- 悬停：边框 `--accent` 40%、`translateY(-1px)`、`var(--glow)`；不要放大整张卡。
- 标题前的 emoji 只作为内容标记（🕺 / 👗 / 👤），UI 图标一律 Lucide 16px。

### Gallery Card

- `--card` + 1px border + 6px 圆角 + `padding: 8px`；图片井 3:4 / 2:3 / 4:5，4px 圆角，`--surface-sunken` 底。
- 图片 `object-fit: cover`，悬停 `scale(1.03)`（300ms）；边框转 `--accent` 40%，加 `var(--glow)`。
- 左上角 ID 角标：`--overlay-ink` 底 + `--overlay-paper` 字，2px 圆角、10px / 700、等宽数字。
- 右上角角度/分类角标：`--overlay-light` 底 + `--overlay-light-foreground` 字，2px 圆角、10px / 600。
- 「加入配方」按钮：图片左下、全圆、30px 高、11px / 600；未选 `--overlay-ink` 底 + 米白字，已选 `--success` 实心 + 米白字 + 勾图标。不要用 emerald-600 配 10px 白字（对比不足），用 `--success` 的深色档。
- 标题：`title-serif` 17.28px / 700，两行截断，最小高 36px；元信息 11px `--muted-foreground`。
- 五角度缩略条：5 列、gap 4px；缩略图 1:1、4px 圆角；当前角度 1px `--accent` 描边，其余透明；角度文字 9px `micro`。
- 手机每行 2 张；卡片间距 12px；「加入配方」按钮保持 30px 高，点击热区 44px。

### Filter Pills & Search Panel

- 筛选面板：`--card` + 1px border + 6px 圆角 + `padding: 16px`；布局 `1fr 200px auto auto`（桌面），手机单列。
- 搜索框：34px 高、4px 圆角、`--surface-sunken` 底、左侧 16px 搜索图标、12px 字；聚焦边框 `--accent`。
- 下拉：34px 高、4px 圆角、`--surface-sunken` 底；下拉内容玻璃 + 8px 圆角。
- 体系/分类药丸：全圆、`padding: 6px 14px`、14px / 500；激活实心 `--primary`，未激活 `--muted` 底 + `--muted-foreground` 字；悬停字色转 `--foreground`。
- 18+ 解锁药丸：虚线 1px 危险色 40% + 危险色 5% 底 + 危险色文字；点击后才请求解锁，不要默认显示敏感内容。
- 「全部视角」分段控件：4px 圆角、`--surface-sunken` 底、内部按钮 4px 圆角；激活 `--primary`，未激活 `--muted-foreground`。

### Lightbox & Zoom

- 弹层最大宽 896px（`56rem`），90dvh；`--popover` 底 + 1px border + 8px 圆角 + `var(--shadow-strong)`；遮罩 `rgb(0 0 0 / .60)`。
- 图片区在左，`--surface-sunken` 底，最小 380px / 65dvh；图片完整显示，不裁切。右侧信息栏 300px，`--surface` 底，左侧 1px border。
- 标题 `headline-sm` 18px 衬线；角标用 `--secondary` / 描边徽标；ID 行 12px mono，`--surface-sunken` 底、4px 圆角，复制按钮 `--accent`。
- 信息表 12px，`dt` 用 `--muted-foreground`，`dd` 用 500 字重；视角缩略图 5 列，当前项 1px `--accent`。
- 缩放条：44px 按钮、12px 百分比、等宽数字；图片区 8px 圆角；`+ / - / 0` 键与滚轮/双指缩放保持一致。
- 操作按钮：主按钮全圆 44px 高；下载/新窗口用 outline 30px；关闭按钮 44×44。

### Recipe Dock

- 停在底部 12px，最大宽 1024px，`--card/95` 底 + `backdrop-filter: blur(12px)` + 1px `--glass-border` + 8px 圆角 + `var(--shadow)`。
- 三个槽位（人像 / 服装 / 姿势）等分：`--surface-sunken` 底 + 4px 圆角 + `padding: 8px`；缩略图 32×40 / 40×32、4px 圆角；标题 11px / 500 截断；空槽用虚线 1px `--border` + `--muted-foreground` 图标。
- 姿势槽内的「白模 / 深度」切换：2px 圆角小药丸，未选 `--muted-foreground`，选中 `--accent` 字 + `--accent` 10% 底。
- 右侧操作：随机 / 复制词 / 清空 / 合成（主按钮）。按钮 30px 高、全圆；主按钮带计数。手机上槽位保持 3 列，操作区换行。
- 配方栏只在图库页出现；合成台、设置、指南页隐藏，避免重复。

### Studio

- 12 栏：左 7、右 5，gap 16–24px；区块用 Section Panel（6px 圆角 + border）。
- 参考图槽位：`--surface-sunken` 底 + 4px 圆角 + `padding: 10px`；缩略图 48×60、4px 圆角；槽位缺失时用 1px 危险色描边 + 危险色 5% 底，并在顶部显示一条可聚焦的错误条。
- 附加参考图：4px 圆角、`--surface` 底、1px border；缩略图 44×36。
- 提示词：mono 12px、行高 1.6、`--surface-sunken` 底、4px 圆角；字符数/复制/重置用 30px 按钮。
- 生成按钮：44px 高、全圆、实心 `--primary`、14px / 600；生成中用 spinner + 「生成中…」，禁用态只降透明度，不改色。
- 结果区：`--surface-sunken` 底 + 8px 圆角，最小高 340px（手机）/ 460px（桌面）；生成结果图片 8px 圆角，点击打开灯箱；空状态图标 28px `--accent` 40% + 12px 说明。
- 生成历史：2 / 3 / 4 列，3:4 图片、4px 圆角；模型名用 `--overlay-ink` 底 + 米白 10px mono；操作按钮 44px 高。

### Settings & Guide

- 供应商切换：两列（手机）/ 五列（桌面），`--card` 底 + 1px border + 6px 圆角；激活 1px `--accent` 描边 + `--accent` 6% 底 + `--accent` 字。
- 状态圆点：8px；已连接 `--success`，已配置未检测 `--warning`，未配置 `--muted-foreground` 30%。
- 表单：标签 12px / 500；输入 34px、4px 圆角、`--surface-sunken` 底；说明 11px `--muted-foreground`；错误文字 `--destructive`。
- 指南长文：12px / 1.8，最大宽 760px；标题 18px 衬线；代码 12px mono、`--surface-sunken` 底、4px 圆角。

### Status & Badges

- 徽标统一全圆，高 20px，10px / 600，`padding: 0 8px`。
- 成功 / 安全 / 已连接：`--success-soft` 底 + `--success` 字 + 1px `--success` 40% 描边。
- 18+ / 危险 / 未解锁：`--info-soft`? 不，用危险色同构：危险色 8% 底 + 危险色字 + 1px 危险色 40% 描边。
- 警告 / 未配置：`--warning-soft` 底 + `--warning` 字。
- 热门 / 排名：`--warning` 或 `--accent` 实心药丸配深色字；不要用橙色 500 配 10px 白字（对比不足）。
- 图片上的 ID / 角度角标按 Gallery Card 的 overlay 配方，不参与状态色。

### Notifications

- Sonner 通知：最大宽 420px，最小高 56px，`--popover` 底 + 1px `--border` + 8px 圆角 + `var(--shadow)`；文字 14px，描述 12px `--muted-foreground`。
- 图标 18px：成功 `--success`、错误 `--destructive`、警告 `--warning`、信息 `--info`。
- 关闭按钮 44×44，悬停 `--surface-hover`；移动端提供 44px 的「查看通知」按钮。
- `prefers-reduced-motion` 下关闭所有过渡和动画。

### Empty & Loading

- 空状态：`--card` + 1px border + 6px 圆角，最小高 320px；图标座 48px 圆、`--surface-sunken` 底、`--accent` 图标；标题 14px / 600；说明 12px `--muted-foreground`。
- 骨架：6px 圆角、`--surface-sunken` 底；图库骨架高 520–540px；不要用脉冲彩色。

### ComfyUI Plugin Modal

ComfyUI 里的选图弹窗是同一产品的另一个表面，**也跟随 Night Craft，不再保留 indigo/violet 渐变**：

- 遮罩 `rgb(10 10 11 / .82)` + `blur(8px)`；弹窗最大 1280×860，`#14161F`? 不，改成 `--background #101114`、面板 `#171719`、卡片 `#1B1B1D`、输入 `#0B0C0E`。
- 强调色 `#E8A532`；主按钮实心琥珀 + 深墨字；选中态 2px 琥珀 + `0 0 0 3px rgb(232 165 50 / .25)` 光晕。
- 圆角：弹窗 8px、卡片 6px、输入/按钮 4px、角标 2px。
- 标题 15px 衬线；正文 12px Inter；卡片标题 12px / 600；ID/副标题 10px；卡片最小宽 158px、4:5 图片。
- 顶部 tab 用全圆药丸：未选透明 + 1px border，选中琥珀 12% 底 + 琥珀字 + 1px 琥珀 52% 描边。
- 底部槽位药丸：`#11131C` 底 + 1px border；选中琥珀 14% 底 + 1px 琥珀。
- 这个弹窗是暗色固定表面，不跟随网页的亮色模式；但颜色、圆角、字体必须来自本文件，不要另造一套紫色系统。

```yaml
components:
  app-shell:
    backgroundColor: "{colors.background}"
    textColor: "{colors.foreground}"
  sidebar-panel:
    backgroundColor: "{colors.sidebar}"
    textColor: "{colors.sidebar-foreground}"
    width: "{spacing.sidebar-width}"
  sidebar-item:
    textColor: "{colors.sidebar-foreground}"
    rounded: "{rounded.sm}"
    height: "{spacing.control-height}"
    padding: 8
  sidebar-item-active:
    backgroundColor: "{colors.sidebar-accent}"
    textColor: "{colors.sidebar-accent-foreground}"
    rounded: "{rounded.sm}"
  header-bar:
    backgroundColor: "{colors.background}"
    textColor: "{colors.foreground}"
    height: "{spacing.header-height}"
    padding: 16
  page-title:
    textColor: "{colors.foreground}"
    typography: "{typography.headline-lg}"
  page-description:
    textColor: "{colors.muted-foreground}"
    typography: "{typography.body-md}"
  section-panel:
    backgroundColor: "{colors.card}"
    textColor: "{colors.foreground}"
    rounded: "{rounded.md}"
    padding: 24
  section-title:
    textColor: "{colors.foreground}"
    typography: "{typography.headline-md}"
  stat-card:
    backgroundColor: "{colors.card}"
    textColor: "{colors.foreground}"
    rounded: "{rounded.md}"
    padding: 16
  stat-value:
    textColor: "{colors.foreground}"
    typography: "{typography.stat-value}"
  library-card:
    backgroundColor: "{colors.card}"
    textColor: "{colors.foreground}"
    rounded: "{rounded.md}"
    padding: 16
  gallery-card:
    backgroundColor: "{colors.card}"
    textColor: "{colors.foreground}"
    rounded: "{rounded.md}"
    padding: 8
  gallery-image-well:
    backgroundColor: "{colors.muted}"
    rounded: "{rounded.sm}"
  gallery-title:
    textColor: "{colors.foreground}"
    typography: "{typography.title-serif}"
  filter-pill-active:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.primary-foreground}"
    rounded: "{rounded.full}"
    typography: "{typography.body-lg}"
    padding: 8
  filter-pill:
    backgroundColor: "{colors.muted}"
    textColor: "{colors.foreground}"
    rounded: "{rounded.full}"
    typography: "{typography.body-lg}"
    padding: 8
  chip:
    backgroundColor: "{colors.card}"
    textColor: "{colors.muted-foreground}"
    rounded: "{rounded.xs}"
    typography: "{typography.label-sm}"
    padding: 4
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.primary-foreground}"
    rounded: "{rounded.full}"
    height: "{spacing.control-height}"
    padding: 12
  button-outline:
    backgroundColor: "{colors.card}"
    textColor: "{colors.foreground}"
    rounded: "{rounded.sm}"
    height: "{spacing.control-height}"
    padding: 10
  button-ghost:
    textColor: "{colors.muted-foreground}"
    rounded: "{rounded.sm}"
    height: "{spacing.control-sm}"
  button-destructive:
    backgroundColor: "{colors.destructive}"
    textColor: "{colors.primary-foreground}"
    rounded: "{rounded.full}"
    height: "{spacing.control-height}"
    padding: 12
  badge-secondary:
    backgroundColor: "{colors.secondary}"
    textColor: "{colors.secondary-foreground}"
    rounded: "{rounded.full}"
    height: 20px
    padding: 8
  badge-outline:
    textColor: "{colors.foreground}"
    rounded: "{rounded.full}"
    height: 20px
    padding: 8
  image-id-badge:
    backgroundColor: "{colors.overlay-ink}"
    textColor: "{colors.overlay-paper}"
    rounded: "{rounded.xs}"
    typography: "{typography.mono-sm}"
    height: 20px
    padding: 6
  image-angle-badge:
    backgroundColor: "{colors.overlay-light}"
    textColor: "{colors.overlay-light-foreground}"
    rounded: "{rounded.xs}"
    typography: "{typography.label-sm}"
    height: 20px
    padding: 6
  add-to-recipe-selected:
    backgroundColor: "{colors.success}"
    textColor: "{colors.primary-foreground}"
    rounded: "{rounded.full}"
    typography: "{typography.label-md}"
    height: 30px
    padding: 10
  status-badge-success:
    backgroundColor: "{colors.success-soft}"
    textColor: "{colors.success}"
    rounded: "{rounded.full}"
    typography: "{typography.label-sm}"
    height: 20px
    padding: 8
  status-badge-warning:
    backgroundColor: "{colors.warning-soft}"
    textColor: "{colors.warning}"
    rounded: "{rounded.full}"
    typography: "{typography.label-sm}"
    height: 20px
    padding: 8
  status-badge-info:
    backgroundColor: "{colors.info-soft}"
    textColor: "{colors.info}"
    rounded: "{rounded.full}"
    typography: "{typography.label-sm}"
    height: 20px
    padding: 8
  success-dot:
    backgroundColor: "{colors.success}"
    size: 8px
  warning-dot:
    backgroundColor: "{colors.warning}"
    size: 8px
  info-dot:
    backgroundColor: "{colors.info}"
    size: 8px
  recipe-dock:
    backgroundColor: "{colors.card}"
    textColor: "{colors.foreground}"
    rounded: "{rounded.lg}"
    padding: 8
  recipe-slot:
    backgroundColor: "{colors.muted}"
    textColor: "{colors.foreground}"
    rounded: "{rounded.sm}"
    padding: 8
  content-card:
    backgroundColor: "{colors.card}"
    textColor: "{colors.card-foreground}"
    rounded: "{rounded.md}"
    padding: 16
  dialog-panel:
    backgroundColor: "{colors.popover}"
    textColor: "{colors.popover-foreground}"
    rounded: "{rounded.lg}"
    padding: 16
  lightbox-image-area:
    backgroundColor: "{colors.muted}"
    rounded: "{rounded.lg}"
  lightbox-side-panel:
    backgroundColor: "{colors.popover}"
    textColor: "{colors.popover-foreground}"
    width: 300px
  input-field:
    backgroundColor: "{colors.muted}"
    textColor: "{colors.foreground}"
    rounded: "{rounded.sm}"
    height: "{spacing.control-height}"
    padding: 10
  menu-item-hover:
    backgroundColor: "{colors.accent}"
    textColor: "{colors.accent-foreground}"
    rounded: "{rounded.sm}"
    padding: 8
  focus-ring:
    backgroundColor: "{colors.ring}"
    size: 2px
  hairline-divider:
    backgroundColor: "{colors.border}"
    height: 1px
  chart-series-1:
    backgroundColor: "{colors.chart-1}"
  chart-series-2:
    backgroundColor: "{colors.chart-2}"
  chart-series-3:
    backgroundColor: "{colors.chart-3}"
  chart-series-4:
    backgroundColor: "{colors.chart-4}"
  chart-series-5:
    backgroundColor: "{colors.chart-5}"
  notification-toast:
    backgroundColor: "{colors.popover}"
    textColor: "{colors.popover-foreground}"
    rounded: "{rounded.lg}"
    padding: 12
  empty-state:
    backgroundColor: "{colors.card}"
    textColor: "{colors.foreground}"
    rounded: "{rounded.md}"
    padding: 24
  code-block:
    backgroundColor: "{colors.muted}"
    textColor: "{colors.foreground}"
    rounded: "{rounded.sm}"
    typography: "{typography.mono-md}"
    padding: 8
  admin-brand-mark:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.primary-foreground}"
    rounded: "{rounded.md}"
    size: 30px
```

## Do's and Don'ts

- **Do** 只用 `src/index.css` 的语义令牌（`var(--card)`、`var(--border)`、`var(--muted-foreground)`、`var(--accent)`）。shadcn 组件里继续用 `bg-card`、`text-muted-foreground`、`bg-primary`，不要写字面色阶。
- **Don't** 新增第二个品牌色相。每个模式只有一个强调色（暗琥珀 `#E8A532` / 亮陶土 `#B34631`）；成功 / 警告 / 信息 / 危险只表达状态。
- **Don't** 用 `bg-muted` 大面积铺面板。面板用 `--card` + `1px solid var(--border)`；muted 只用于输入框底、缩略图井、悬停和禁用态。
- **Don't** 给图库卡、图片格加玻璃。玻璃只给配方栏、弹层、通知和侧栏。
- **Do** 用衬线只做标题和统计数字：`headline-*` / `title-serif` / `stat-value` / `brand`。正文、按钮、筛选器、图片元信息一律 Inter。
- **Do** 把图库卡做成 6px 圆角 + 1px 暖色边框；图片井 4px；筛选药丸、主按钮、状态徽标全圆。不要再使用 20/22px 的大圆角面板。
- **Do** 用 `gap` 排版，不用 `space-x-*` / `space-y-*`；用 `min-w-0` + `truncate` 处理中文/泰文长名称；用容器查询适配顶栏和面板。
- **Do** 让焦点走 `outline: 2px solid var(--ring); outline-offset: 2px`（按钮内用 `ring-3 ring-ring/50`），不要移除焦点环。
- **Don't** 手写 `dark:` 颜色覆盖。暗色是 `.dark` 类覆盖同名 CSS 变量；只有状态色和图片角标允许 `dark:` 变体。
- **Do** 过渡统一暗色 `.14s` / 亮色 `.13s`；图片缩放 300ms；按下 `scale(.98)`；`prefers-reduced-motion` 下全部关闭。
- **Do** 移动端把可点区域做到 44px（图标可 34px，热区 44px）；桌面控件 34px；图库每行 2 张。
- **Do** 让泰文行高 ≥ 1.6，中文标题负字距，正文不压字距；不要把标签固定成刚好放英文的宽度。
- **Don't** 把 ComfyUI 插件的 indigo/violet 渐变带回网页，也不要把网页的亮色模式带进插件弹窗；插件固定暗色，但用同一套琥珀令牌。
- **Don't** 在图片上叠加琥珀色角标或彩色发光；图片角标只用中性黑/白，选中态才用 1px 琥珀描边 + 光晕。
- **Don't** 把给模型看的提示词或 API Key 展示在界面上；配置页只显示掩码和「已配置」状态。
- **Do** 保持图片懒加载、缩略图优先；不要为了“统一”给每张图加滤镜、圆角描边或渐变遮罩。
