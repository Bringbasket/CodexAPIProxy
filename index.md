---
title: Codex 配置自定义 API 服务
description: 使用 Codex 自定义服务商配置连接兼容接口，并妥善管理 API Key。
---

# Codex 配置自定义 API 服务

Codex 可以通过本地配置文件选择模型和服务商。若你的团队提供了兼容接口，也可以按服务商文档配置自定义地址。本文给出一个通用示例；服务地址、模型标识、协议和认证方式都要以你所使用的服务文档为准。

## 开始前

先确认服务商明确支持 Codex 所需的接口协议，并准备好 API 根地址、模型标识和 API Key。使用第三方服务前，建议先了解其数据处理、日志留存和计费规则。

请勿将真实 API Key 写进文章、截图或公开仓库。

## 找到配置文件

Codex 配置文件通常位于用户目录下的 .codex/config.toml：

- Windows：C:\\Users\\你的用户名\\.codex\\config.toml
- macOS / Linux：~/.codex/config.toml

如果文件不存在，可创建 .codex 目录和 config.toml 文件。修改前建议先备份原配置。

## 配置模型和服务商

在 config.toml 中添加或调整默认模型设置。示例名称需要替换为服务商实际提供的值：

```toml
model = "your-model"
model_provider = "custom_gateway"
model_reasoning_effort = "medium"

[model_providers.custom_gateway]
name = "Custom Gateway"
base_url = "https://api.example.com/v1"
env_key = "CUSTOM_API_KEY"
wire_api = "responses"
requires_openai_auth = false
```

model_provider 的值需要与 [model_providers.custom_gateway] 中的名称对应。示例中的 base_url、wire_api 和认证选项必须符合服务端要求；不要假设所有兼容服务都使用相同路径或协议。

## 设置 API Key

配置里的 env_key 指定了 Codex 要读取的环境变量名。下面以 Windows PowerShell 为例，在当前终端会话中临时设置：

```powershell
$env:CUSTOM_API_KEY = "替换成你的密钥"
```

macOS / Linux 当前终端会话可使用：

```bash
export CUSTOM_API_KEY='替换成你的密钥'
```

如果要持久保存环境变量，请按操作系统的用户环境变量方式设置。保存后重新打开终端或编辑器，再启动 Codex。

## 检查配置

启动 Codex 后发送一个简单请求，并按顺序核对：

1. 服务商名称和默认选择的名称一致；
2. 模型标识是该服务实际支持的值；
3. API Key 环境变量已设置，且名称与 env_key 一致；
4. API 根地址和接口协议符合服务商文档；
5. 日志中没有认证失败、模型不存在或请求路径错误。

## 常见问题

### 提示缺少 API Key

确认变量设置在启动 Codex 的同一用户环境中。设置持久变量后，重启已经打开的终端、IDE 和 Codex。

### 提示模型不可用

检查模型标识是否与服务商控制台或文档中的 API 名称完全一致。展示名称不一定等于请求中使用的名称。

### 请求返回 404 或无法连接

核对服务商要求的 API 根路径、协议和网络访问方式。路径重复添加或漏掉必要前缀都可能导致请求失败。

### 遇到认证错误

检查密钥是否有效、是否有余额或调用权限，并确认服务商要求的认证格式。排查时不要公开粘贴完整密钥或带有认证头的日志。

## 安全建议

- 只使用你信任且了解其数据处理方式的服务；
- 使用单独的密钥，并设置合理的权限和有效期；
- 把本地密钥文件加入 .gitignore，不要提交到版本库；
- 分享日志前清理 API Key、Token、Cookie 等凭据；
- 遇到不符合预期的计费或数据行为时，及时停用密钥并联系服务提供方。

## 参考资料

- [Codex 配置参考](https://developers.openai.com/codex/config-reference/)
- [GitHub Pages 快速入门](https://docs.github.com/en/pages/quickstart)

## 更新记录

- 2026-11-06：创建原创配置指南初稿。
