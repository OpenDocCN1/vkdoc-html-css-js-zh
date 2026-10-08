# 第二章：理解 Bridger

## 2.1 什么是 Bridger

Bridger 是一个 RPC 库，它帮助你在 Android 应用和服务之间建立通信。它像一座“桥梁”，连接你的公共 API 代码和私有实现代码。

## 2.2 Bridger 的核心概念

Bridger 基于“桥接模式”（Bridge Pattern），将接口（Interface）与实现（Implementation）解耦。这意味着你可以通过接口定义公共的 RPC 服务，而将具体的实现细节隐藏在幕后。

## 2.3 Bridger 的好处

- **清晰的 API 定义**：使用 Interface 定义 RPC 方法，使得 API 文档化更容易。
- **实现隐藏**：客户端只依赖接口，不知道具体的实现类。
- **易于测试**：你可以使用 mock 或 stub 来模拟 RPC 调用。
- **可扩展性**：可以轻松添加新的实现类，而不影响客户端。
