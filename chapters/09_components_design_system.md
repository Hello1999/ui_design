# 第 09 章：组件库与设计系统

> 目标：理解设计系统的构成和价值，学会构建可复用的组件库，实现设计与开发的高效协作。

---

## 9.1 什么是设计系统

**设计系统（Design System）** 是一套完整的设计语言、组件库、规范文档的集合，为产品提供统一的视觉和交互标准。

### 设计系统的三个层次

```
┌─────────────────────────────────────────┐
│  产品界面（Product UI）                  │  ← 最终呈现给用户的界面
├─────────────────────────────────────────┤
│  组件库（Component Library）             │  ← 可复用的 UI 组件
├─────────────────────────────────────────┤
│  设计基础（Design Tokens / Foundation）  │  ← 颜色、字体、间距等基础变量
└─────────────────────────────────────────┘
```

### 为什么需要设计系统

**没有设计系统**：
- 不同页面的按钮颜色略有差异
- 开发需要反复询问"这个间距是多少"
- 新设计师上手需要很长时间理解规范
- 改一个颜色需要更新几十个文件

**有了设计系统**：
- 改一个 Token，全局生效
- 组件复用，设计效率提升 3-5 倍
- 设计与开发用同一套语言沟通
- 产品视觉高度一致

### 行业知名设计系统

| 设计系统 | 所属公司 | 特点 |
|---------|---------|------|
| Material Design | Google | 通用性强，文档详尽 |
| Human Interface Guidelines (HIG) | Apple | iOS/macOS 原生规范 |
| Ant Design | 阿里巴巴 | 中文企业级产品最流行 |
| Fluent Design | Microsoft | Windows/Office 生态 |
| Base Web | Uber | 开源，工程化强 |

---

## 9.2 Atomic Design（原子设计）

Brad Frost 提出的原子设计理论，将 UI 拆分为五个层次：

```
原子（Atoms）
  ↓ 组合
分子（Molecules）
  ↓ 组合
有机体（Organisms）
  ↓ 组合
模板（Templates）
  ↓ 填充内容
页面（Pages）
```

### 各层级示例

**原子（Atoms）**：最小的不可再分的 UI 元素
```
• 按钮（Button）
• 输入框（Input）
• 标签（Label）
• 图标（Icon）
• 头像（Avatar）
• 颜色色块
• 字体样式
```

**分子（Molecules）**：由原子组合的功能单元
```
• 搜索框 = 输入框 + 按钮
• 表单项 = 标签 + 输入框 + 错误提示
• 用户信息 = 头像 + 姓名 + 描述
• 导航项 = 图标 + 文字标签
```

**有机体（Organisms）**：由分子组合的完整区块
```
• 导航栏 = Logo + 多个导航项 + 用户信息
• 商品卡片 = 图片 + 标题 + 价格 + 操作按钮
• 表单 = 多个表单项 + 提交按钮
• 评论列表 = 多个评论条目
```

**模板（Templates）**：页面的骨架结构
```
• 列表页模板 = 导航栏 + 搜索区 + 卡片网格 + 底部Tab
• 详情页模板 = 导航栏 + 图片区 + 信息区 + 操作按钮
```

**页面（Pages）**：填充真实内容的最终界面

---

## 9.3 设计 Token

**Design Token** 是存储设计决策的命名变量，连接设计工具和代码实现。

### Token 的类型

**全局 Token（Global Tokens）**：原始值
```
color-blue-400: #4299E1
color-blue-500: #3182CE
spacing-4: 4px
spacing-8: 8px
font-size-14: 14px
```

**语义 Token（Semantic Tokens）**：引用全局 Token，赋予语义
```
color-primary: {color-blue-400}
color-primary-hover: {color-blue-500}
spacing-card-padding: {spacing-16}
font-size-body: {font-size-14}
```

**组件 Token（Component Tokens）**：特定组件使用的 Token
```
button-primary-bg: {color-primary}
button-primary-text: white
button-primary-padding-x: {spacing-16}
button-border-radius: 8px
```

### 在 Figma 中使用 Variables

```
Figma Variables 对应 Token：

创建变量集合（Collection）：
  ├── Global（全局原始值）
  │     ├── Color/Blue/400 = #4299E1
  │     └── Spacing/16 = 16
  ├── Semantic（语义变量）
  │     ├── Color/Primary = {Global/Color/Blue/400}
  │     └── Spacing/Card = {Global/Spacing/16}
  └── Modes（深浅色模式）
        ├── Light Mode
        └── Dark Mode
```

---

## 9.4 构建组件库

### 组件构建流程

**步骤 1：列出所有需要的组件**
```
基础组件（必须）：
• Button（按钮）
• Input（输入框）
• Select（下拉选择）
• Checkbox / Radio（多选/单选）
• Toggle（开关）
• Icon（图标）
• Avatar（头像）

展示组件：
• Card（卡片）
• Badge（徽标）
• Tag（标签）
• Tooltip（工具提示）
• Modal（弹窗）
• Drawer（抽屉）
• Toast（消息提示）

导航组件：
• Tab Bar（标签栏）
• Navigation Bar（导航栏）
• Breadcrumb（面包屑）
• Pagination（分页）
```

**步骤 2：为每个组件设计所有状态（States）**

以按钮为例：
```
Primary Button 状态：
• Default（默认）
• Hover（悬停）    ← Web 端
• Active/Pressed（按下）
• Focus（聚焦）    ← 键盘导航
• Disabled（禁用）
• Loading（加载中）
```

**步骤 3：在 Figma 中创建 Variants**
```
Button 组件 Variants：

Type: Primary | Secondary | Tertiary | Destructive
Size: Large | Medium | Small
State: Default | Hover | Pressed | Disabled | Loading
Icon: None | Left Icon | Right Icon | Icon Only
```

**步骤 4：添加组件文档**

每个组件应包含：
- 何时使用（Usage）
- 何时不使用（Don't）
- 变体说明
- 与开发的对应关系

---

## 9.5 按钮组件设计详解

按钮是最基础也最复杂的组件之一，以下是完整规范：

### 按钮类型

```
主要按钮（Primary）：最重要的操作，每个界面最多1个
  [   确认   ]  ← 填充色，主色背景，白色文字

次要按钮（Secondary）：重要但非最优先的操作
  [ 取 消 ]  ← 边框，白色背景，主色文字/边框

文字按钮（Tertiary/Ghost）：低优先级操作
  忽略    ← 无边框无背景，纯文字

危险按钮（Destructive）：破坏性操作（删除等）
  [   删除   ]  ← 红色系，用于不可逆操作
```

### 按钮尺寸规范

```
大型按钮（Large）：
  高度：48px  内边距：16px  字号：16px  圆角：8px

中型按钮（Medium，默认）：
  高度：40px  内边距：16px  字号：14px  圆角：8px

小型按钮（Small）：
  高度：32px  内边距：12px  字号：14px  圆角：6px

图标按钮（Icon Only）：
  大：48×48px  中：40×40px  小：32×32px
```

### 按钮可点击区域

```
⚠️ 重要：最小可点击区域 44×44px（iOS HIG 要求）

视觉上按钮可以更小，但实际可点击热区必须 ≥ 44px
```

---

## 9.6 表单组件设计

### 输入框状态

```
默认（Default）：
  ┌─────────────────────────────────────┐
  │ 请输入内容                           │
  └─────────────────────────────────────┘
  边框：#E0E0E0，圆角：8px

聚焦（Focus）：
  ┌─────────────────────────────────────┐
  │ 用户正在输入                          │
  └─────────────────────────────────────┘
  边框：主色 #4299E1，2px

填写完成（Filled）：
  ┌─────────────────────────────────────┐
  │ user@example.com                    │
  └─────────────────────────────────────┘

错误（Error）：
  ┌─────────────────────────────────────┐
  │ invalid-email                       │
  └─────────────────────────────────────┘
  边框：红色 #FF4D4F
  ⚠ 请输入有效的邮箱地址

禁用（Disabled）：
  ┌─────────────────────────────────────┐
  │ 该字段不可编辑                        │
  └─────────────────────────────────────┘
  背景：#F5F5F5，文字：#999999，不可交互
```

### 表单布局

```
单列表单（移动端推荐）：
  姓名 *
  ┌─────────────────────────┐
  │ 请输入姓名               │
  └─────────────────────────┘

  邮箱 *
  ┌─────────────────────────┐
  │ 请输入邮箱               │
  └─────────────────────────┘

  [    提交    ]

双列表单（Web 端宽屏）：
  姓 *              名 *
  ┌─────────┐      ┌─────────┐
  │          │      │          │
  └─────────┘      └─────────┘
```

---

## 9.7 设计规范文档

设计系统除了 Figma 文件，还需要配套文档。

### 文档内容结构

```
设计规范文档
├── 品牌指南
│   ├── Logo 使用规范
│   ├── 品牌色彩
│   └── 品牌声音/调性
├── 基础样式
│   ├── 颜色系统（所有颜色 Token）
│   ├── 字体规范（层级、字重、行距）
│   ├── 图标库
│   └── 间距规范（8pt 系统）
├── 组件库
│   ├── 每个组件的：使用说明 / 变体 / 规范尺寸 / 示例
│   └── 反例（Don't）
└── 页面模板
    └── 常用页面的布局规范
```

### 交付给开发的规范

**颜色**：提供 HEX/RGB/HSL 值，以及 CSS 变量名
```css
--color-primary: #4299E1;
--color-primary-dark: #3182CE;
--color-text-primary: #1A1A1A;
```

**字体**：提供字号、字重、行高的 CSS 样式
```css
.text-h1 { font-size: 28px; font-weight: 700; line-height: 1.3; }
.text-body { font-size: 15px; font-weight: 400; line-height: 1.6; }
```

**组件**：提供 Figma 链接 + 交互说明文档

---

## 9.8 Figma 组件最佳实践

### 文件组织结构

```
🎨 Design System（Figma 文件）
├── 📄 Cover（封面）
├── 📄 Tokens（颜色/字体/间距变量）
├── 📄 Icons（图标库）
├── 📄 Components
│   ├── Frame：Buttons
│   ├── Frame：Form Elements
│   ├── Frame：Navigation
│   ├── Frame：Cards
│   ├── Frame：Modals & Dialogs
│   └── Frame：Feedback（Toast/Alert）
└── 📄 Documentation（使用示例）
```

### 命名规范

```
组件命名：[类别]/[名称]/[变体]

示例：
  Button/Primary/Default
  Button/Primary/Hover
  Button/Secondary/Large
  Input/Text/Default
  Input/Text/Error
  Navigation/Tab Bar/4-Items
```

---

## 本章实践作业

**作业 1**：在 Figma 中创建一个基础组件库，包含：
- 按钮（3种类型 × 3种状态）
- 输入框（5种状态）
- 头像（3种尺寸）
- 卡片（基础卡片模板）

**作业 2**：为你的组件库定义颜色 Token：
- 5个主色层级（400-800）
- 语义色（成功/警告/错误）
- 中性色（6个灰度）

**作业 3**：使用你创建的组件，拼装一个完整的"登录页面"，要求所有元素使用组件实例（Instance），不使用独立绘制的元素。

---

## 本章小结

| 概念 | 要点 |
|------|------|
| 设计系统 | 基础 Token + 组件库 + 规范文档 |
| Atomic Design | 原子→分子→有机体→模板→页面 |
| Design Token | 命名变量，连接设计与代码 |
| 组件规范 | 每个组件需要所有状态和变体 |
| 文档交付 | 设计规范需配合文档给到开发团队 |

---

[← 上一章：布局与栅格系统](./08_layout_grid.md) | [下一章：交互设计与微交互 →](./10_interaction_design.md)
