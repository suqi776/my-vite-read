# My Vite Read

一个基于 Vue 3 + Vite 的智能语音阅读助手，让文字发声，让阅读更轻松。

## 🎯 项目功能

### 1. 智能语音阅读
- 支持多种语音引擎，提供自然流畅的阅读体验
- 可调节语速（0.5x - 2x）和音调（0.5x - 2x）
- 支持暂停、继续、停止等阅读控制
- 实时语音状态反馈

### 2. 多语言支持
- 支持中文（简体、繁体、粤语等）
- 支持多种语音音色选择
- 智能识别文本语言

### 3. 用户友好的界面
- 简洁现代的 UI 设计
- 响应式布局，适配各种设备
- 深色模式支持
- 直观的操作控制

### 4. 核心特性
- 文本输入区域支持大段文字
- 一键清除文本内容
- 实时语音状态显示
- 流畅的动画效果

## 🛠️ 技术栈

- ⚡️ [Vue 3](https://github.com/vuejs/core) - 渐进式 JavaScript 框架
- 🎯 [Vite](https://github.com/vitejs/vite) - 下一代前端工具链
- 📦 [pnpm](https://pnpm.io/) - 快速、节省磁盘空间的包管理器
- 🎨 [UnoCSS](https://github.com/unocss/unocss) - 即时按需原子化 CSS 引擎
- 🔍 [VueUse](https://github.com/antfu/vueuse) - Vue Composition API 工具集合
- 🛣️ [Vue Router](https://github.com/vuejs/vue-router) - Vue.js 官方路由
- 🎯 [Iconify](https://iconify.design) - 通用图标框架
- 🧹 [ESLint](https://github.com/antfu/eslint-config) - 可插拔的 JavaScript 代码检查工具

## 📦 安装

```bash
# 克隆仓库
npx degit sqsuqi/myvite my-vite-app

# 进入项目目录
cd my-vite-app

# 安装依赖
pnpm install
# 如果没有安装 pnpm，请运行：npm install -g pnpm
```

## 🚀 使用说明

1. 在文本框中输入或粘贴要阅读的文本
2. 选择喜欢的语音（支持多种中文语音）
3. 调节语速和音调
4. 点击"开始阅读"按钮
5. 使用暂停、继续、停止按钮控制阅读过程

## 📝 项目结构

```
├── src/           # 源代码
│   ├── components/ # 组件
│   ├── pages/     # 页面
│   ├── router/    # 路由配置
│   ├── composables/ # 组合式函数
│   └── assets/    # 静态资源
├── public/        # 公共资源
└── ...            # 配置文件
```

## 🔧 开发

```bash
# 启动开发服务器（端口 7777）
pnpm dev

# 构建生产版本
pnpm build

# 预览生产构建
pnpm preview
```

## 📄 许可证

MIT
