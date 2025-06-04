<p align="center">
  <img src="img/logo.png" alt="QuietMind Logo" width="450"/>
</p>


## QuiteMind：你的AI智能消息托管助手

还在被社交媒体的信息轰炸打乱节奏吗？在专注工作或放松休息时，面对一堆非必要、不紧急的消息，不仅耗时费神，还严重影响效率。

**QuiteMind**正是为此而生 —— 基于 Dify 打造，帮你智能分类每一条消息：

- 📨 重要消息及时邮件推送，不错过关键内容
- 🤖 非紧急信息由 LLM 自动回复，高效代聊不失礼
- 📁 全部对话有序归档，随时可查，放心托管

让 AI 成为你的消息管家，从此摆脱碎片化干扰，专注当下、轻松应对。

---

## 项目特点

### 1. 四象限消息分类

结合 LLM Agent Workflow 与「重要/紧急」四象限原理，实现自动化分类策略：

- **重要且紧急**：即时邮件通知
- **重要不紧急**：标记“已收到”，待后续人工处理
- **紧急不重要**：由 LLM 先行沟通确认必要性
- **非重要非紧急**：LLM 自动回复日常对话或忽略

### 2. 消息完整性识别

自动检测消息意图是否表述清晰完整；若内容不完整，将等待补充后再处理，避免误解或误判。

### 3. 历史记录管理

- 以“联系人 + 时间”为单位自动归档消息
- 支持上下文联想提取，自动调用相关历史对话补充当前消息上下文
- 超出设定轮数 (`MAX_HISTORY_LEN`) 后自动压缩旧记录，节省存储空间

------

## 快速开始

以下步骤演示如何在本地或服务器上部署并运行本项目。建议先确保已完成 Dify 的安装与基础配置。

### 部署 Dify

1. **使用云服务**
   - 访问 Dify 云端管理界面：[Dify 云服务地址](https://cloud.dify.ai/apps)
   - 在“应用”中创建一个 Chatflow 应用。
2. **自建服务器（可选）**
   - 参考 Dify 官方文档完成社区版安装：
     - [Dify 社区版部署教程](https://docs.dify.ai/zh-hans/getting-started/install-self-hosted/readme)
   - 安装完成后，登录本地控制台，创建新的 Chatflow 应用。

> [!TIP] 
> 本项目示例已在 Dify v1.4.0 测试通过，建议使用对应版本以保证兼容性。

###  获取项目代码

```bash
git clone https://github.com/BAIKEMARK/QuietMind.git
cd QuietMind
```

- 根目录下可看到 `assistant.yml`（或其他后缀）文件，即本项目的 Chatflow 定义。

### 导入 DSL 文件到 Dify

1. 进入 Dify 控制台 → “工作室”（Studio）
2. 点击“导入 Chatflow” → 选择项目根目录下的 `assistant.yml` 文件
3. 导入完成后会自动生成一个新的 Chatflow 应用

### 配置环境变量

在 Dify 的“工作室”→“环境变量”中，添加以下变量：

- `ZAPIER_MCP_URL`：
  - 通过参考 [Dify MCP 插件指南](https://docs.dify.ai/zh-hans/plugins/best-practice/how-to-use-mcp-zapier) 获取 Zapier 端点（Zapier MCP Server URL）。
- `USER_EMAIL`：
  - 接收“重要且紧急”消息推送的个人邮箱地址。
- `MAX_HISTORY_LEN`：
  - 历史消息最大记录轮次（超出后自动触发旧记录压缩）。

------

## 部署到消息平台

本项目支持将智能助手接入 QQ、微信、Telegram 等常见聊天平台，以下以 AstrBot 为例进行说明。

### AstrBot 简介

[AstrBot](https://github.com/AstrBotDevs/AstrBot) 是一款多平台 LLM 聊天机器人框架，可以通过配置不同服务提供商，将 Chatflow 应用部署到各类社交平台。

### 部署 AstrBot

1. 参考官方文档安装 AstrBot：
   - Windows：
      [使用 Windows 一键安装器部署 AstrBot](https://astrbot.app/deploy/astrbot/windows.html)
   - Linux/macOS：
      可访问 [AstrBot 部署指南](https://astrbot.app/deploy/) 获取详细步骤。
2. 启动 AstrBot 服务后，访问其 Web UI（默认地址为 `http://localhost:6185`）。

### 在 AstrBot 中新增 “服务提供商”

1. 登录 AstrBot Web UI → 左侧菜单选择 **服务提供商** → **新增服务提供商**
2. 配置项填写如下：
   - **类型（Provider Type）**：选择 `Dify`
   - **Dify 应用类型（App Type）**：选择 `chatflow`
   - **API Base URL**：填写 Dify Chatflow 的 API 服务地址（默认 80 端口，如有修改需带上端口号）。
   - **API Key**：Dify 中为当前 Chatflow 应用生成的 API Key。
   - API Base URL 与 Key获取方法如图所示：
  ![API Base URL 与 Key获取方法](img\Dify2astrbot.png "API Base URL 与 Key获取方法" )
3. 点击「启用」→「保存」，等待 AstrBot 自动重启并加载新配置。

### 在 AstrBot 中绑定消息平台

1. 登录 AstrBot Web UI → 左侧菜单选择 **消息平台**
2. 根据需要接入的聊天平台（如 QQ、微信、Telegram），依照官方文档配置相应插件：
   - **QQ（NapCatQQ 协议）**：
      参考 [通过 NapCatQQ 协议实现 QQ 端接入](https://astrbot.app/deploy/platform/aiocqhttp/napcat.html#通过-napcatqq-协议实现端接入-qq)
   - **微信**：
      参考 [AstrBot 微信部署文档](https://astrbot.app/deploy/platform/wechat/wechatpadpro.html)
   - **Telegram**：
      参考 [AstrBot Telegram 部署文档](https://astrbot.app/deploy/platform/telegram.html)
3. 配置完成后，启动对应平台插件，AstrBot 会自动将聊天消息转发到 Dify Chatflow，由 LLM 进行消息分类与处理。

------

## 常见问题

1. **如何确认 Dify Chatflow 已正常运行？**
   - 进入 Dify 控制台，检查对应 Chatflow 的“状态”：
     - 若显示「已部署」且没有报错，即可正常调用 API。
   - 可在 AstrBot → “服务提供商”→“测试”中尝试发送一条简单消息，查看是否能收到 LLM 回复。
2. **我不想使用阿里云百炼，能换成其他 SDK 吗？**
   - 可以。只需在Dify中添加对应的模型供应商，并 Chatflow 中替换相应的 SDK 调用节点即可。
3. **为什么有些消息没被分类到正确的象限？**
   - 四象限分类依赖 LLM 的理解能力，若上下文缺失或表达不够清晰，可能导致分类偏差。

------

## 未来规划

- [ ] 将历史消息存储迁移至外部数据库（如 MySQL、MongoDB），方便统一管理与备份。
- [ ] 每日整理聊天记录与待办清单通知用户。
- [ ] 基于 RAG 技术优化历史记录检索，将相似对话快速检索出来并作为上下文输入。
- [ ] 将 Chatflow 逻辑迁移至 Python 本地部署，实现离线运行与更灵活的扩展。
- [ ] 增加更多消息渠道（如 Slack、Discord）和更多消息处理策略的支持。

---


## 💖贡献本项目

本项目源于一个灵光一现的idea，目前仍处于早期探索阶段，主要基于 Dify 实现了核心概念，功能尚不完善。欢迎各位同学、开发者和大佬提出宝贵建议，或通过贡献代码共同完善这一想法：

1. **提交 Issue**
   - 发现 BUG 或有功能建议，请直接在本仓库提交 Issue。
2. **发起 Pull Request**
   - 若您对某项功能有改进思路，可 Fork 本仓库，提交 Pull Request。
3. **加入讨论群**
   - 若需在线交流，可在 Issue 中留言或联系作者获取讨论群二维码。

------

## 致谢

- 感谢 **Dify 团队** 提供高效、易用的 Chatflow 平台。
- 感谢 **AstrBot 社区** 的开源贡献，让多平台部署变得简单。
- 感谢**所有试用并反馈**的小伙伴们，你们的建议让项目不断完善。

---

如有问题欢迎提交 Issue 或 PR，期待与你一同打磨更智能的消息助手！