# 安装

Substrate 框架提供了一组简单的命令来设置本地环境。

这些命令将会：

- 为你的操作系统安装包管理器
- 安装 Rust 开发环境
- 安装核心的 Substrate 二进制文件
- 安装[其他辅助脚本](https://github.com/paritytech/substrate-up) 以快速开发新的 Substrate 节点

准备好后，运行以下命令下载并安装 Substrate：

```bash
curl https://getsubstrate.io -sSf | bash
```

这个过程可能会需要花费一段时间，因为它将从源码编译多个二进制文件。

不幸的是，由于安装过程对于每个操作系统和机器来说都是唯一的，因此该脚本可能做不到开箱即用。你可以查看[脚本](https://github.com/paritytech/scripts/blob/master/get-substrate.sh)，以及一些与你开发环境相关的 [PR](https://github.com/paritytech/scripts/pulls)。我们正在尝试将相关依赖项的安装打包到一个脚本中。

在等待 Substrate 及其相关依赖项（如 Rust 等）下载和安装的同时，还需要在你的开发环境中安装其他可能未包含在包中的程序：

- [Node + NPM](https://nodejs.org/en/download/)
- [Visual Studio Code](https://code.visualstudio.com/) (或者你喜欢的 IDE)
- [Chrome](https://www.google.com/chrome/) 或者基于 Chromium 的浏览器 (抱歉 Firefox 用户 :[ )

如果你遇到任何问题或发现有需要改进的地方（例如支持不同的操作系统以自动化安装过程），请尽可能创建一个 [Issue](https://github.com/paritytech/scripts/issues) 或者 [PR](https://github.com/paritytech/scripts/pulls)。