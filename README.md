# Pmod 本地大模型管理平台 (Pmod Desktop Local Agent)

Pmod 是专为大语言模型本地高性能推理、长上下文长程思维及多智能体协同设计的桌面级一体化管理平台与客户端。

---

## 🌟 核心特性

- **现代高颜值客户端**：基于 Tauri 2.0 + Vue 3 构建的高性能、极低系统资源占用的桌面端；
- **分层 KV 缓存调度**：支持 GPU 原生显存与系统宿主内存协同的 KVMem 分层管理，从容支持 256K 超长上下文；
- **双模量化支持**：原生支持 `nvfp4`（NVIDIA Blackwell / Ada 架构 TensorCore 硬件加速）与 `rk4v4`（极低显存占用压缩格式）；
- **动态硬件探测与适配**：自适应读取真实 GPU 显存余量并进行智能推荐，绝无机型与显存写死；
- **全周期监控与度量**：实时捕获思考耗时、首字时延 (TTFT)、解码生成速度及 Token 吞吐分布；
- **零写死路径自适应架构**：采用动态根目录嗅探引擎，安装到任意电脑、任意磁盘（如 `C:`、`D:`、移动硬盘）均可即装即用。

---

## 📁 目录架构说明

```text
Pmod/
├── desktop/               # 桌面客户端源码工程
│   ├── src/               # Vue 3 前端界面源码 (仪表盘、状态机、图表、配置面板)
│   ├── src-tauri/         # Tauri 2.0 Rust 后端核心 (进程守护、VRAM 探针、遥测总线)
│   ├── package.json       # 前端依赖配置
│   └── vite.config.ts     # Vite 构建配置
├── runtime/               # 独立运行时支持库
│   ├── engine/            # Ninfer 1.0.0 推理引擎与 CUDA/FFmpeg 动态库
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
├── templates/             # Jinja2 增强对话模版
├── scripts/               # 运维与自动化工具
│   ├── build-inno-installer.ps1  # Inno Setup 一键全自动独立安装包编译器
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
- **命令行静默调试**：
  ```powershell
  .\start.cmd
  ```
  停止服务：
  ```powershell
  .\stop.cmd
  ```

---

## 👨‍💻 作者信息

- **作者**：复读机
- **作者 QQ**：`785029497`
- **交流群**：`1128130199`

