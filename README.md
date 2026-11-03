# 🚀 Pmod - 本地大模型一体化管理平台 (Pmod Desktop Local Agent)

<div align="center">

[![Release](https://img.shields.io/badge/Release-v1.0.0-blue.svg)](https://github.com/Q4446/Pmod)
[![License](https://img.shields.io/badge/License-Apache%202.0%20%26%20NVIDIA%20CUDA%20EULA-green.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Windows%20x64-orange.svg)](#)
[![QQ Group](https://img.shields.io/badge/QQ交流群-1128130199-red.svg)](https://qun.qq.com/universal-share/share?ac=1&authKey=O9lTkl%2B56Zf8U%2BMSk3UrLK7HmOIJZlhbfESHPi1PUSo2PBHzH6o003oDOf9UAJFg&busi_data=eyJncm91cENvZGUiOiIxMTI4MTMwMTk5IiwidG9rZW4iOiJ5a1M5T0dmeFBKVDg2bnRYaHJkcFhTYXh5NjJWcUtDdHlpNk1HZWdUT2FqZDJkTUMzci9EUFFPVU55b1ZCL01yIiwidWluIjoiNzg1MDI5NDk3In0%3D&data=qDBRJgG4lwWBURR5AYk5mFRku441wXmLM2AdJqE-E6ZIaL3X6ljAIp2woaaHgY6TQIqsfXnGPEhGUGW5m-KjXw&svctype=4&tempid=h5_group_info)

**Pmod** 是专为大语言模型本地高性能推理、长上下文长程思维及多智能体协同设计的桌面级一体化管理平台与智能对话工作台。

</div>

---

## 📸 功能界面全景展示

### 1. 智能对话工作台 (沉浸式智能体协同环境)
内置深度集成的智能体工作台，原生支持多轮深度思考推理、工作区环境绑定、工具调用及流式代码输出，让本地大模型发挥最强研发与交互能力。

![智能对话工作台](docs/images/02-chat.png)

---

### 2. 就绪待机与生态接口引导
启动前 GPU 处于零功耗休眠待命状态。内置标准 OpenAI Compatible API 代理网关（默认 `:1236` 端口），可无缝对接 VS Code、Cursor、Claude Code 等主流开发工具与客户端生态。

![就绪待机与生态接口引导](docs/images/01-standby.jpg)

---

### 3. 推理性能与会话统计面板
全周期监控大模型运行状态，实时捕获平均生成速率 (tok/s)、首字时延 (TTFT)、思考耗时与 KV 缓存命中率。提供直观的波动走势图、Token 构成堆叠分析与轮次级诊断参数。

![推理性能与会话统计面板](docs/images/03-stats.jpg)

---

### 4. 引擎、模型与显卡驱动配置
系统自动穿透二进制解析模型架构、参数体量及量化精度；自适应嗅探显卡硬件架构（深度适配 RTX 50 系 Blackwell 与 Ada 架构），并支持显存与系统内存分层 KV 缓存调度。

![引擎模型与显卡驱动配置](docs/images/04-model-config.jpg)

---

### 5. 显存管理与多卡进程智能调度
实时探针嗅探 Windows 当前独显物理显存与核显共享显存分配，分类列出所有占用进程，支持将各类后台应用一键迁移调度至核显，为本地大模型释放最大的极致显存空间。

![显存管理与多卡进程智能调度](docs/images/05-vram-manager.jpg)

---

## 🌟 核心架构与技术亮点

- **现代高颜值客户端**：基于 Tauri 2.0 (Rust) + Vue 3.5 构建，极低系统资源占用，秒级冷启动；
- **分层 KV 缓存调度 (KVMem)**：支持 GPU 原生显存与系统宿主内存协同驱动，从容承载 256K 超长上下文长程思维；
- **双模量化支持**：原生支持 `nvfp4`（NVIDIA Blackwell / Ada 架构 TensorCore 硬件加速）与 `rk4v4`（极低显存占用压缩格式）；
- **动态硬件自适应嗅探**：自适应读取真实 GPU 显存余量并进行智能推荐，杜绝显存写死；
- **零维护云端版本更新**：内置国内高可用多镜像源（支持 ghproxy、jsDelivr CDN 毫秒级轮询与超时熔断），支持客户端一键检测正式版。

---

## 📁 目录架构说明

```text
Pmod/
├── desktop/               # 桌面客户端源码工程 (Tauri 2.0 + Vue 3.5 + TypeScript)
│   ├── src/               # Vue 3 前端界面源码 (仪表盘、状态机、图表、配置面板)
│   ├── src-tauri/         # Tauri 2.0 Rust 后端核心 (进程守护、VRAM 探针、遥测总线)
│   ├── package.json       # 前端依赖配置
│   └── vite.config.ts     # Vite 构建配置
├── docs/                  # 项目文档与高清界面截图
│   └── images/            # README 演示图集
├── runtime/               # 独立运行时支持库
│   ├── engine/            # Ninfer 推理引擎与 CUDA/FFmpeg 动态库
│   ├── node/              # 便携式 Node.js 运行时
│   ├── logs/              # 运行日志输出目录
│   ├── cache/             # 运行时缓存与 Playwright 资源
│   ├── temp/              # 临时转储与运行时会话
│   └── dsh-home/          # DSH 智能体沙盒隔离目录
├── integrations/          # 守卫与智能体生态
│   ├── cache_guard.mjs    # Cache Guard 安全代理总线
│   └── dsh/               # DSH (DeepSeek Harness) 智能体环境与补丁
├── config/                # 配置文件
│   ├── local.json         # 本地实际运行参数 (包含模型路径、端口及显存池分配)
│   ├── example.json       # 标准默认配置模版
│   └── downloads.json     # 依赖下载哈希与来源清单
├── version.json           # 云端版本号与发布更新清单
├── scripts/               # 运维与自动化工具
│   ├── build-inno-installer.ps1  # Inno Setup 一键独立离线安装包编译器
│   ├── configure-dsh.mjs         # DSH 运行环境与补丁配置器
│   ├── get-gpu-process-vram.ps1  # GPU 显存精准嗅探脚本
│   └── common.ps1                # 基础运维函数库
├── start.cmd / start.ps1  # 快捷启动服务脚本
├── stop.cmd / stop.ps1    # 快捷优雅停止所有服务脚本
└── LICENSE                # 开源许可证
```

---

## 🛠️ 构建与打包指南

### 1. 前端编译
```powershell
cd desktop
pnpm install
pnpm build
```

### 2. 编译 Release 版桌面端程序
```powershell
cd desktop/src-tauri
cargo build --release
```
编译产物将生成在 `desktop/src-tauri/target/release/Pmod.exe`。

### 3. 一键制作独立发布安装包 (Inno Setup)
本工程集成了全自动化安装包生成脚本，自动收集推理引擎、CUDA DLLs、Node 运行时、DSH 智能体包并对配置进行脱敏清洗：
```powershell
powershell -ExecutionPolicy Bypass -File scripts/build-inno-installer.ps1
```
打包成功后，将在 `dist/` 目录下生成独立安装包：
- 文件名：`dist/Pmod-Setup-v1.0.0.exe`
- 特点：内含全套离线引擎与运行时环境，直接在任意新电脑安装即可运行。

---

## 🚀 启动与运行

- **桌面图形化运行**：直接双击运行编译出的 `Pmod.exe` 或通过安装包安装后由桌面快捷方式启动；
- **命令行快捷启动**：
  ```powershell
  .\start.cmd
  ```
  停止服务：
  ```powershell
  .\stop.cmd
  ```

---

## 👨‍💻 作者与交流群

- **作者**：复读机
- **作者 QQ**：`785029497`
- **官方交流群**：`1128130199`
- **一键加入官方交流群**：[点击直接加入 QQ 群](https://qun.qq.com/universal-share/share?ac=1&authKey=O9lTkl%2B56Zf8U%2BMSk3UrLK7HmOIJZlhbfESHPi1PUSo2PBHzH6o003oDOf9UAJFg&busi_data=eyJncm91cENvZGUiOiIxMTI4MTMwMTk5IiwidG9rZW4iOiJ5a1M5T0dmeFBKVDg2bnRYaHJkcFhTYXh5NjJWcUtDdHlpNk1HZWdUT2FqZDJkTUMzci9EUFFPVU55b1ZCL01yIiwidWluIjoiNzg1MDI5NDk3In0%3D&data=qDBRJgG4lwWBURR5AYk5mFRku441wXmLM2AdJqE-E6ZIaL3X6ljAIp2woaaHgY6TQIqsfXnGPEhGUGW5m-KjXw&svctype=4&tempid=h5_group_info)
