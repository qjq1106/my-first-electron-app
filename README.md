# my-first-electron-app

一个基于 `Electron + Vue 3 + TypeScript + Vite` 的桌面应用起步项目，适合用来快速搭建跨平台桌面端原型或小型工具应用。

当前项目仍保留了较多模板初始内容，但主进程、预加载脚本、渲染进程和打包链路已经接通，可以直接作为 Electron 应用开发基础继续扩展。

## 技术栈

- `Electron 30`
- `Vue 3`
- `TypeScript`
- `Vite 5`
- `vite-plugin-electron`
- `electron-builder`

## 项目结构

```text
.
|-- electron/                Electron 主进程与预加载脚本
|   |-- main.ts              创建窗口、加载页面、注册应用生命周期
|   `-- preload.ts           通过 contextBridge 暴露安全的 IPC 能力
|-- public/                  静态资源
|-- src/                     Vue 渲染进程代码
|   |-- App.vue              页面入口组件
|   |-- main.ts              Vue 应用挂载入口
|   |-- style.css            全局样式
|   `-- components/          示例组件
|-- dist/                    前端构建产物（执行构建后生成）
|-- dist-electron/           Electron 构建产物（执行构建后生成）
|-- electron-builder.json5   打包配置
`-- vite.config.ts           Vite 与 Electron 集成配置
```

## 环境要求

- `Node.js 18+`
- 推荐使用 `pnpm`

如果你还没有安装依赖，可以先执行：

```bash
pnpm install
```

## 开发启动

```bash
pnpm dev
```

执行后会启动 Vite 开发服务器，并由 Electron 加载开发环境页面。

## 打包构建

```bash
pnpm build
```

该命令会依次完成以下步骤：

1. 使用 `vue-tsc` 做 TypeScript 类型检查
2. 构建 Vue 渲染进程代码到 `dist/`
3. 构建 Electron 主进程与预加载脚本到 `dist-electron/`
4. 使用 `electron-builder` 生成安装包

默认打包配置位于 [electron-builder.json5](/E:/electronFiles/my-first-electron-app/electron-builder.json5)，产物输出目录为：

```text
release/<version>/
```

## 默认打包目标

- Windows: `nsis` 安装包
- macOS: `dmg`
- Linux: `AppImage`

如果你需要修改应用名称、`appId`、图标或安装包格式，可以编辑 [electron-builder.json5](/E:/electronFiles/my-first-electron-app/electron-builder.json5)。

## 运行机制说明

- `electron/main.ts` 负责创建 `BrowserWindow`，并在开发环境加载 Vite 地址，在生产环境加载本地 `dist/index.html`
- `electron/preload.ts` 通过 `contextBridge` 向渲染进程暴露 `ipcRenderer` 的常用方法
- `src/` 下是标准 Vue 单页应用结构，后续业务页面和组件可以从这里继续扩展

## 当前状态

目前页面内容仍以模板示例为主，适合作为一个可运行、可打包的 Electron + Vue 起点工程。  
如果要正式投入业务开发，通常下一步会继续补充以下内容：

- 替换默认示例页面
- 设计并收敛 IPC 通信接口
- 增加本地存储、文件系统或系统能力接入
- 按业务拆分页面、组件和状态管理

## 常用开发建议

- 优先把 Electron 能力封装到 `preload.ts`，不要在渲染进程直接暴露 Node 高权限能力
- 新增桌面端功能时，先明确代码属于主进程、预加载层还是渲染层
- 发版前同步检查 `productName`、`appId`、图标资源和安装包目标平台配置

## 脚本说明

```json
{
  "dev": "vite",
  "build": "vue-tsc && vite build && electron-builder",
  "preview": "vite preview"
}
```

其中：

- `pnpm dev` 用于本地开发
- `pnpm build` 用于类型检查、构建和打包
- `pnpm preview` 用于预览前端产物，不等同于完整 Electron 运行环境
