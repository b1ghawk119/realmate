# RealMate（真·群友）

> 一个有灵魂的 QQ 群聊 AI 机器人 🎭

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/node-%3E%3D18.0.0-brightgreen)](https://nodejs.org/)

**不是客服，是真·群友。** 傲娇、话少、有个性。

### 效果展示：
<img width="1452" height="4086" alt="PixPin_2025-10-26_19-30-33" src="https://github.com/user-attachments/assets/fdcbaf85-cecb-49da-9fe0-93df2e7a65ee" />


---

## ✨ 特点

- 🎭 **像人一样聊天** - 拒绝AI客服腔，真实的傲娇人设
- 🧠 **理解上下文** - 看得懂聊天记录，理解引用和话题
- 🔧 **自动调用工具** - 需要查天气、搜索时自动调用，不废话
- 🖼️ **图片识别** - 支持多模态Vision模型
- 💬 **流式响应** - 实时打字效果，长消息自动合并转发
- 🔌 **兼容性强** - 支持所有OpenAI格式的API

---

## 🚀 快速开始

### 1. 安装

```bash
git clone https://github.com/b1ghawk119/realmate.git
cd realmate
npm install
```

### 2. 配置

复制 `.env.example` 为 `.env`，填入配置：

```env
# NapCat配置
NAPCAT_HTTP_URL=http://127.0.0.1:3000
NAPCAT_WS_URL=ws://127.0.0.1:3001
NAPCAT_TOKEN=你的token

# 机器人信息
BOT_QQ=机器人QQ号
BOT_NICKNAME=机器人昵称

# LLM配置（以DeepSeek为例，可换成任何OpenAI兼容的API）
LLM_BASE_URL=https://api.deepseek.com
LLM_API_KEY=sk-xxxxxxxxxxxxxxxx
LLM_MODEL=deepseek-chat
```

### 3. 运行

```bash
node bot.js
```

看到 `✨ RealMate 已启动` 就成功了！

---

## 🔧 支持的模型

**只要兼容OpenAI API格式，都能用。** 比如：

**国内：**
- DeepSeek - 超便宜（1M tokens ≈ ¥1）
- 智谱GLM - 免费额度多
- 月之暗面Kimi - 超长上下文
- 硅基流动 - 完全免费（镜像各种开源模型）

**国外：**
- OpenAI GPT-4o
- Anthropic Claude（需适配）
- Groq - 免费且超快

**本地：**
- Ollama
- LM Studio
- vLLM

**配置示例：**

<details>
<summary>DeepSeek（推荐）</summary>

```env
LLM_BASE_URL=https://api.deepseek.com
LLM_API_KEY=sk-xxxxxxxx
LLM_MODEL=deepseek-chat
```
</details>

<details>
<summary>智谱GLM</summary>

```env
LLM_BASE_URL=https://open.bigmodel.cn/api/paas/v4
LLM_API_KEY=xxxxxxxx.xxxxxxxx
LLM_MODEL=glm-4-flash
```
</details>

<details>
<summary>硅基流动（免费）</summary>

```env
LLM_BASE_URL=https://api.siliconflow.cn/v1
LLM_API_KEY=sk-xxxxxxxx
LLM_MODEL=deepseek-ai/DeepSeek-V3
```
</details>

<details>
<summary>Ollama（本地）</summary>

```bash
# 先安装Ollama并下载模型
ollama pull qwen2.5:7b
```

```env
LLM_BASE_URL=http://localhost:11434/v1
LLM_API_KEY=ollama
LLM_MODEL=qwen2.5:7b
```
</details>

---

## 🖼️ 图片识别

如果主模型不支持Vision（比如DeepSeek Chat），单独配置视觉模型：

```env
VISION_BASE_URL=https://api.siliconflow.cn/v1
VISION_API_KEY=sk-xxxxxxxx
VISION_MODEL=Pro/Qwen/Qwen2-VL-7B-Instruct
```

---

## 🎮 使用示例

```
👤 用户: @粽子 你好
🤖 机器人: 嗯，咋了？

👤 用户: @粽子 北京今天天气
🤖 机器人: [调用天气工具] 多云，15°C，还行吧

👤 用户: [发图片] @粽子 这是什么
🤖 机器人: [分析图片] 这是一只橘猫
```

---

## ⚙️ 常用配置

```env
# 群白名单（留空=所有群）
ENABLED_GROUPS=713984600,987654321

# 用户白名单（留空=所有用户）
ENABLED_USERS=

# 流式响应（推荐开启）
ENABLE_STREAM=true

# 长消息合并转发（避免刷屏）
ENABLE_FORWARD_MSG=true
FORWARD_MSG_THRESHOLD=500

# 历史消息压缩（省Token）
COMPRESS_TEXT=true
COMPRESS_CODE=true
```

完整配置见 [`.env.example`](.env.example)

---

## 🐛 常见问题

**Q: 机器人不回复？**
- 确认消息里有 @机器人 或提及昵称
- 检查WebSocket是否连接成功
- 查看群/用户是否在白名单内

**Q: 图片识别失败？**
- 主模型不支持Vision，需单独配置 `VISION_*` 参数

**Q: Token消耗太快？**
- 开启历史压缩：`COMPRESS_TEXT=true`
- 减少上下文：`MAX_CONTEXT_SEND=20`
- 用免费服务：硅基流动、Groq
- 本地部署：Ollama

**Q: 想用本地模型？**
```bash
# 安装Ollama
curl https://ollama.ai/install.sh | sh
ollama pull qwen2.5:7b

# 配置.env
LLM_BASE_URL=http://localhost:11434/v1
LLM_API_KEY=ollama
LLM_MODEL=qwen2.5:7b
```

---

## 📊 统计

机器人每30分钟输出一次运行统计：

```json
{
  "messagesReceived": 248,
  "messagesResponded": 67,
  "totalTokens": 58023,
  "avgPromptTokens": 681,
  "uptime": "2h 30m 0s"
}
```

**成本估算（DeepSeek为例）：**
- 每天100次对话 ≈ ¥0.08
- 每月 ≈ ¥2.4

---

## 📁 项目结构

```
realmate/
├── bot.js           # 主程序
├── .env             # 配置（需自己创建）
├── .env.example     # 配置模板
├── package.json     # 依赖
└── README.md        # 说明文档
```

---

## 🛣️ TODO

- [ ] 私聊支持
- [ ] 数据持久化
- [ ] Web管理面板
- [ ] 插件系统

---

## 📜 开源协议

[MIT License](LICENSE) - 可自由使用、修改、商用

---

## 🙏 致谢

- [NapCat](https://github.com/NapNeko/NapCatQQ) - QQ机器人框架
- [OpenAI](https://openai.com/) - API标准

---

<div align="center">

**⭐ 喜欢就给个Star吧！**

Made with ❤️ by [b1ghawk119](https://github.com/b1ghawk119)

</div>
