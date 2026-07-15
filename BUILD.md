# Luban Build Guide

本文档说明如何在 Windows 和 Linux 上为当前仓库构建 Luban。

仓库根目录：

```text
D:\code\app\luban
```

当前解决方案：

```text
src/Luban.sln
```

当前 CLI 项目：

```text
src/Luban/Luban.csproj
```

## Prerequisites

本仓库当前使用 `.NET 8 SDK` 构建。

先确认本机已安装 `.NET 8 SDK`，而不是只安装 Runtime。

- 官方下载页: [Download .NET](https://dotnet.microsoft.com/en-us/download)
- 安装总览: [Install .NET on Windows, Linux, and macOS](https://learn.microsoft.com/en-us/dotnet/core/install/)

安装完成后，先验证：

```bash
dotnet --version
dotnet --info
```

如果输出的是 `8.x`，通常就可以继续构建。

## Windows

### 1. 安装 .NET 8 SDK

推荐从官方页面下载 Windows x64 的 `.NET 8 SDK` 安装包并安装：

- 下载入口: [Download .NET](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)
- Windows 安装说明: [Install .NET on Windows](https://learn.microsoft.com/en-us/dotnet/core/install/windows)

安装完成后重新打开终端，再执行：

```powershell
dotnet --version
dotnet --info
```

### 2. 还原依赖

在仓库根目录执行：

```powershell
dotnet restore src/Luban.sln
```

### 3. 构建 Release

```powershell
dotnet build src/Luban.sln -c Release
```

### 4. 运行 CLI 验证

```powershell
dotnet run --project src/Luban -- --help
```

### 5. 可选: 生成 Windows 发布产物

生成不依赖目标机器 `.NET` 环境的自包含发布包：

```powershell
dotnet publish src/Luban/Luban.csproj -c Release -r win-x64 --self-contained true -o publish/win-x64
```

### 6. 可选: 代码格式化

```powershell
scripts\format.bat
```

## Linux

### 1. 安装 .NET 8 SDK

Linux 安装方式和发行版有关，优先按官方文档执行：

- Linux 安装总览: [Install .NET on Linux](https://learn.microsoft.com/en-us/dotnet/core/install/linux)
- Ubuntu 安装说明: [Install .NET on Ubuntu](https://learn.microsoft.com/en-us/dotnet/core/install/linux-ubuntu-install)
- Debian 安装说明: [Install .NET on Debian](https://learn.microsoft.com/en-us/dotnet/core/install/linux-debian)

如果你的发行版已提供 `.NET 8 SDK` 包，通常可以直接安装。  
例如 Debian 或 Ubuntu 上常见命令为：

```bash
sudo apt-get update
sudo apt-get install -y dotnet-sdk-8.0
```

安装完成后验证：

```bash
dotnet --version
dotnet --info
```

### 2. 还原依赖

在仓库根目录执行：

```bash
dotnet restore src/Luban.sln
```

### 3. 构建 Release

```bash
dotnet build src/Luban.sln -c Release
```

### 4. 运行 CLI 验证

```bash
dotnet run --project src/Luban -- --help
```

### 5. 可选: 生成 Linux 发布产物

生成不依赖目标机器 `.NET` 环境的自包含发布包：

```bash
dotnet publish src/Luban/Luban.csproj -c Release -r linux-x64 --self-contained true -o publish/linux-x64
```

### 6. 可选: 代码格式化

```bash
sh scripts/format.sh
```

### 7. 服务器部署示例

生成并上传 Linux x64 发布目录：

```bash
dotnet publish src/Luban/Luban.csproj -c Release -r linux-x64 --self-contained true -o publish/linux-x64
```

将 `publish/linux-x64/` 部署到 `/www/wwwroot/luban/` 后，执行：

```bash
# 创建脚本
sudo touch /usr/local/bin/luban

# 写入脚本
sudo tee /usr/local/bin/luban > /dev/null <<'EOF'
#!/bin/bash
exec /www/wwwroot/luban/Luban "$@"
EOF

# 修改权限并测试
sudo chmod +x /usr/local/bin/luban
luban --help
```

## Common Output Paths

本文档命令指定的发布目录：

```text
publish/win-x64/
publish/linux-x64/
```

## Troubleshooting

`dotnet` 不可用或构建失败时，确认已安装 `.NET 8 SDK`，并重新打开终端后执行 `dotnet --info` 检查。
