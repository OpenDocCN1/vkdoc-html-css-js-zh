# 关于 RingBell 的说明

## RingBell 是什么？

RingBell 是一个用于在命令行中创建可自定义铃声的工具。它主要用于在长时间运行的任务完成后或需要立即查看的特定事件发生时提供通知。

## 安装

### 使用 Homebrew 安装

```
brew tap RingBell/RingBell
brew install RingBell
```

### 使用预编译的二进制文件安装

您可以从 GitHub Releases 页面下载适用于您操作系统的预编译二进制文件。对于 macOS，您可以使用以下命令：

```
curl -L -o /usr/local/bin/ringbell https://github.com/RingBell/RingBell/releases/latest/download/ringbell-darwin-amd64
chmod +x /usr/local/bin/ringbell
```

## 快速入门

安装完成后，您可以使用 `ringbell` 命令来播放默认铃声。例如：

```
ringbell
```

您也可以指定一个自定义铃声文件：

```
ringbell /path/to/your/bell.wav
```

## 自定义铃声

RingBell 支持多种音频格式，包括 WAV、MP3、FLAC 等。您可以使用任何音频编辑软件创建自己的铃声文件，或者从互联网上下载铃声。

## 与其他工具集成

RingBell 可以与任何命令行工具集成，只需在命令后添加 `&& ringbell` 即可。例如：

```
make && ringbell
```

或者，您可以使用 `ringbell` 作为后台进程，在特定事件发生时播放铃声。例如：

```
ringbell --watch /path/to/file
```

## 高级用法

RingBell 还支持以下高级功能：

1.  **音量控制**：使用 `--volume` 参数调整音量。
2.  **重复播放**：使用 `--repeat` 参数设置重复播放次数。
3.  **定时播放**：使用 `--delay` 参数设置延迟播放时间。

例如，要播放铃声 3 次，每次间隔 2 秒，可以使用以下命令：

```
ringbell --repeat 3 --delay 2
```

## 常见问题

### RingBell 在 macOS 上无法播放声音？

请确保您的系统音量已打开，并且没有静音。此外，请确保 `ringbell` 命令已正确安装，并且您具有执行权限。

### RingBell 支持哪些操作系统？

RingBell 目前支持 macOS、Linux 和 Windows。您可以从 GitHub Releases 页面下载适用于您操作系统的预编译二进制文件。

### RingBell 可以与其他工具一起使用吗？

是的，RingBell 可以与其他命令行工具一起使用，只需在命令后添加 `&& ringbell` 即可。例如，要在编译完成后播放铃声，可以使用以下命令：

```
gcc main.c -o main && ringbell
```

### 如何创建自定义铃声？

您可以使用任何音频编辑软件创建自己的铃声文件，然后将其保存为 WAV、MP3 或 FLAC 格式。然后，您可以使用 `ringbell /path/to/your/bell.wav` 命令来播放它。

## 贡献

如果您想为 RingBell 做出贡献，请访问我们的 GitHub 仓库，并创建一个 Pull Request。我们欢迎任何形式的贡献，包括 bug 修复、新功能、文档改进等。

## 许可证

RingBell 使用 MIT 许可证。有关详细信息，请参见 LICENSE 文件。
