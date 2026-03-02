# OpenClawCN - AI 助手 macOS 应用

## 📦 项目简介

OpenClawCN 是一个为 macOS 设计的 AI 助手应用，支持 DeepSeek 云大模型，提供完全独立的运行环境。

### 核心特性

- ✅ **跨架构支持**：同时支持 Intel x86_64 和 Apple Silicon ARM64
- ✅ **完全独立运行**：自动下载并配置 Node.js 运行时
- ✅ **智能环境配置**：首次启动自动检测系统并配置环境
- ✅ **实时通信**：WebSocket 实时对话和控制
- ✅ **用量统计**：详细的模型使用量监控
- ✅ **多模型支持**：支持 DeepSeek Chat 和 DeepSeek Coder

## 🚀 快速开始

### 方法 1: 标准安装（推荐）

1. **下载安装包**
   - 对于 Intel Mac：`openclawcn-intel-mac.dmg`
   - 对于 Apple Silicon Mac：`openclawcn.dmg`
   - 通用版本：`openclawcn-complete.dmg`

2. **安装应用**
   - 双击 DMG 文件
   - 将 OpenClawCN 拖拽到 `/Applications` 文件夹

3. **首次启动**
   - 打开 `/Applications/OpenClawCN.app`
   - 应用会自动：
     - 检测系统架构
     - 下载对应的 Node.js 运行时
     - 配置运行环境
     - 启动网关服务
   - 首次启动可能需要 1-2 分钟（下载时间）

### 方法 2: 使用命令行脚本

```bash
# 赋予执行权限
chmod +x /Users/elvinx/openclawcn/setup-and-run.sh

# 运行脚本
/Users/elvinx/openclawcn/setup-and-run.sh
```

脚本会自动：
- ✅ 检测系统和架构
- ✅ 检查/安装 Node.js
- ✅ 安装项目依赖
- ✅ 启动网关服务
- ✅ 打开应用

## 🔧 配置指南

### 配置 DeepSeek API Key

编辑 `~/.openclaw/openclaw.json`：

```json
{
  "models": {
    "providers": {
      "deepseek": {
        "baseUrl": "https://api.deepseek.com/v1",
        "api": "openai-completions",
        "apiKey": "你的 API key",
        "models": [
          {
            "id": "deepseek-chat",
            "name": "DeepSeek Chat"
          },
          {
            "id": "deepseek-coder",
            "name": "DeepSeek Coder"
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "deepseek/deepseek-chat"
      }
    }
  }
}
```

### 自定义运行时配置

编辑 `~/.openclawcn-runtime/config.json`：

```json
{
  "nodeVersion": "20.11.0",
  "installPath": "~/.openclawcn-runtime/node",
  "gatewayPort": 18789
}
```

## 🔍 故障排除

### 1. 应用无法连接服务器

**问题**: 应用启动后显示"Could not connect to the server"

**解决方案**:
```bash
# 检查端口占用
lsof -i :18789

# 如果没有输出，手动启动网关
cd ~/openclaw-cn
node scripts/run-node.mjs gateway --port 18789
```

### 2. Node.js 下载失败

**问题**: 网络问题导致下载失败

**解决方案**:
```bash
# 使用国内镜像手动下载
cd ~/.openclawcn-runtime
curl -L https://npmmirror.com/mirrors/node/v20.11.0/node-v20.11.0-darwin-x64.tar.gz -o node.tar.gz
tar -xzf node.tar.gz
mv node-v20.11.0-darwin-x64 node
rm node.tar.gz
```

### 3. 查看日志

```bash
# 应用日志
tail -f /tmp/clawdbot/clawdbot-$(date +%Y-%m-%d).log

# 网关状态
lsof -i :18789 | grep LISTEN
```

### 4. 清理运行时

```bash
# 删除运行时，下次启动会重新下载
rm -rf ~/.openclawcn-runtime

# 重新启动应用
open /Applications/OpenClawCN.app
```

## 📊 技术栈

- **前端**: SwiftUI
- **后端**: Node.js + openclaw-cn
- **模型**: DeepSeek Chat, DeepSeek Coder
- **通信**: WebSocket
- **运行时**: Node.js 20.11.0 LTS

## 📁 项目结构

```
~/openclawcn/
├── openclawcn/                # 应用源码
│   ├── Assets.xcassets/       # 应用资源
│   ├── control-ui/            # 控制界面
│   ├── ContentView.swift      # 主界面
│   └── openclawcnApp.swift    # 应用入口
├── openclawcn.xcodeproj/      # Xcode 项目
├── .gitignore                 # Git 忽略文件
├── setup-and-run.sh           # 安装运行脚本
├── start-openclawcn.sh        # 启动脚本
└── README.md                  # 项目文档
```

## 🎯 适用场景

### ✅ 适合使用

- **开发者**: 需要本地运行 AI 助手
- **企业用户**: 需要私有化部署
- **隐私敏感**: 不希望数据经过第三方
- **离线环境**: 需要内网部署
- **多 Mac 用户**: 需要在不同架构 Mac 上使用

### ⚠️ 注意事项

- 首次启动需要下载 Node.js（约 50MB）
- 需要稳定的网络连接（用于 API 调用）
- 需要至少 200MB 磁盘空间（运行时 + 缓存）

## 📞 技术支持

### 常用命令

```bash
# 查看应用版本
defaults read /Applications/OpenClawCN.app/Contents/Info.plist CFBundleVersion

# 查看运行时版本
~/.openclawcn-runtime/node/bin/node -v

# 查看网关状态
lsof -i :18789

# 重启应用
killall OpenClawCN
open /Applications/OpenClawCN.app
```

### 配置文件位置

- **应用配置**: `~/.openclaw/openclaw.json`
- **运行时配置**: `~/.openclawcn-runtime/config.json`
- **网关日志**: `/tmp/clawdbot/clawdbot-YYYY-MM-DD.log`
- **Agent 数据**: `~/.openclaw/agents/`

## 📄 许可证

MIT License - 详见项目源码

## 🤝 贡献指南

欢迎对 OpenClawCN 项目做出贡献！如果您有任何改进建议或发现了 bug，请：

1. **提交 Issue**：在 GitHub 仓库中创建一个新的 Issue，描述您的问题或建议
2. **提交 Pull Request**：如果您已经实现了改进，可以提交 Pull Request
3. **反馈意见**：通过 GitHub Issues 或邮件提供您的反馈

### 开发环境设置

```bash
# 克隆仓库
git clone https://github.com/Elv1nx/openclawcn.git

# 进入项目目录
cd openclawcn

# 安装依赖
npm install
```

## 📋 版本更新历史

### v2.0 (Complete Edition)
- ✅ 支持 Intel x86_64 和 Apple Silicon ARM64
- ✅ 自动下载并配置 Node.js 运行时
- ✅ 智能端口管理，避免端口冲突
- ✅ 优化启动速度和稳定性

### v1.0
- ✅ 基础功能实现
- ✅ 支持 DeepSeek 云大模型
- ✅ WebSocket 实时通信
- ✅ 对话和控制功能

## ❓ 常见问题解答

### Q: 首次启动应用时出现 "Could not connect to the server" 错误
**A:** 这通常是因为 Node.js 运行时还未完全下载和配置。请耐心等待 1-2 分钟，然后重新启动应用。

### Q: 如何更换 DeepSeek API Key
**A:** 编辑 `~/.openclaw/openclaw.json` 文件，将 `apiKey` 字段替换为您的新 API Key。

### Q: 应用运行缓慢怎么办
**A:** 确保您的网络连接稳定，并且 Mac 有足够的内存和 CPU 资源。您也可以尝试清理运行时缓存：
```bash
rm -rf ~/.openclawcn-runtime/cache
```

### Q: 如何查看应用日志
**A:** 应用日志存储在 `/tmp/clawdbot/` 目录中，您可以使用以下命令查看：
```bash
tail -f /tmp/clawdbot/clawdbot-$(date +%Y-%m-%d).log
```

## 🚀 性能优化建议

1. **使用 SSD 存储**：应用和运行时最好安装在 SSD 上，以获得最佳性能
2. **保持网络稳定**：API 调用需要稳定的网络连接
3. **定期清理缓存**：定期清理运行时缓存可以提高性能
4. **关闭不必要的应用**：运行 OpenClawCN 时，关闭其他占用资源的应用

## 📚 相关资源

- [DeepSeek 官方文档](https://www.deepseek.com)
- [Node.js 官方网站](https://nodejs.org)
- [SwiftUI 官方文档](https://developer.apple.com/documentation/swiftui)
- [WebSocket 教程](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

---

**🎉 享受完全独立的 OpenClawCN 体验！**
