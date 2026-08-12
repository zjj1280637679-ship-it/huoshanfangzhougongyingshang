<h1 align="center">火山方舟供应商（原生视频理解）</h1>
<p align="center"><strong>让 AstrBot 的火山方舟主模型直接理解文字、图片、QQ 语音与本轮视频，不再另接一条转述旁路。</strong></p>

[![Version](https://img.shields.io/badge/version-0.1.12-e85d3f)](CHANGELOG.md)
[![AstrBot](https://img.shields.io/badge/AstrBot-%3E%3D4.26.1-6b63ff)](https://github.com/AstrBotDevs/AstrBot)
[![Platform](https://img.shields.io/badge/platform-aiocqhttp%20%7C%20webchat-2f855a)](https://docs.astrbot.app/dev/star/plugin-new.html)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

这是一款面向 AstrBot 的火山方舟模型供应商插件。它提供普通 API 与 Agent Plan 两张独立供应商卡，并把视频能力补进模型卡原有的能力选择中。对使用者来说，你只需要按模型实际能力勾选“图像 / 音频 / 视频 / 工具”，然后像平常一样使用当前聊天模型。

QQ 语音不会先交给另一个 STT 或转录模型；本轮发送或引用的视频也不会先由另一个“视频转述模型”概括。它们会和文字、图片以及当前聊天上下文一起进入你正在使用的火山方舟主模型。

交流与反馈：**QQ 群 916646029**

---

# 使用说明

## 你会得到什么

- **原生视频理解**：模型卡勾选“视频”后，本轮发送或引用的视频直接进入火山方舟视频输入协议，由当前主模型理解。
- **QQ 语音直接进入主模型**：常见 QQ 语音会先转换成火山方舟可接受的标准音频格式，再随当前聊天上下文一起发送。
- **图片继续走 AstrBot 原生能力**：不另建图片下载、缓存或转述流程。
- **同一条主对话**：文字、图片、语音、视频和工具结果共享 AstrBot 当前会话上下文，不会拆成互相失忆的多条模型链路。
- **普通 API / Agent Plan 分开配置**：两张供应商卡使用各自的密钥和固定端点，避免插件内部把两种计费路线混在一起。
- **按模型选择能力**：插件不会因为模型名字看起来像“多模态模型”就擅自替你打开能力。

## 安装

### 从 AstrBot 插件市场安装

在 AstrBot 插件市场中找到 **火山方舟供应商（原生视频理解）** 并安装，然后完整重启 AstrBot。

### 手动安装

1. 下载或克隆本仓库。
2. 把插件目录放入 AstrBot 的 `data/plugins/`。
3. 完整关闭并重新启动 AstrBot。
4. 打开 `模型提供商 → 对话 → 新增`。
5. 确认列表里出现：
   - **火山方舟（原生视频）普通 API**
   - **火山方舟（原生视频）Agent Plan API**

> 插件最低支持 AstrBot `4.26.1`。安装、更新、禁用或卸载供应商插件后，建议完整重启 AstrBot；只刷新网页不能证明新的 Provider 注册已经生效。

## 选择哪张供应商卡

| 供应商卡 | 适合什么 | 应填写的密钥 |
| --- | --- | --- |
| **火山方舟（原生视频）普通 API** | 普通按量 / 免费额度的方舟推理接口 | 普通方舟推理 API Key |
| **火山方舟（原生视频）Agent Plan API** | 已购买或正在使用 Agent Plan | Agent Plan 专属 API Key |

两张卡是两条独立路线。不要把普通方舟 Key 填进 Agent Plan 卡，也不要把 Agent Plan Key 填进普通 API 卡。

## 配置普通方舟 API

1. 新增 **火山方舟（原生视频）普通 API**。
2. 填写普通方舟推理 API Key。
3. 获取模型列表，或手动填写官方模型 ID / 推理接入点 ID（例如 `ep-...`）。
4. 打开具体模型卡。
5. 按该模型真实能力勾选“图像 / 音频 / 视频 / 工具使用”。
6. 保存，并把这张模型卡选为当前聊天模型。

普通 API 会使用当前填写的推理 Key 获取该凭据真正可见的模型。不同 Key 的权限不同，所以模型列表数量可能不同。

## 配置 Agent Plan

1. 新增 **火山方舟（原生视频）Agent Plan API**。
2. 填写 **Agent Plan 专属 API Key**。
3. 选择一个带 `agentplan/` 前缀的套餐模型。
4. 按模型真实能力勾选“图像 / 音频 / 视频 / 工具使用”。
5. 保存，并把这张模型卡选为当前聊天模型。

Agent Plan 模型在 AstrBot 中会显示为例如：

```text
agentplan/doubao-seed-2.1-turbo
```

这个 `agentplan/` 只是本地用于区分计费路线的前缀，不会原样发送给火山方舟。

如果你希望使用控制台托管路由，可以选择 `agentplan/ark-code-latest`；它代表可变路由，而不是一个固定底层模型。

## 模型卡上的能力怎么选

| 选项 | 作用 |
| --- | --- |
| **文本** | 普通文字输入 |
| **图像** | 允许当前模型接收图片 |
| **音频** | 允许当前模型接收 QQ 语音等音频输入 |
| **视频** | 允许当前模型接收本轮或本轮引用的视频 |
| **工具使用** | 允许模型参与 AstrBot 的工具调用流程 |

能力开关应按你实际选择的模型来设置。插件不会在运行时再用另一套规则覆盖你的选择。

## 图片怎么用

模型卡勾选 **图像** 后，继续按 AstrBot 原来的方式发送或引用图片即可。

插件不复制 AstrBot 的图片下载、历史管理或缓存设施；图片仍走 AstrBot 原生 Chat Provider 路径。

## QQ 语音怎么用

模型卡勾选 **音频** 后，直接发送或引用 QQ 语音即可。

插件会把 QQ 常见音频输入归一化成火山方舟可接受的 WAV，再交给当前主聊天模型。你不需要另外配置全局 STT，也不会多出一个负责转录的第二语言模型。

支持的处理目标为：

```text
QQ 语音
  → 识别真实音频内容
  → 必要时解码 / 转码
  → 16 kHz、单声道、16-bit PCM WAV
  → 发送给当前火山方舟主模型
```

## 视频怎么用

模型卡勾选 **视频** 后：

- 可以直接发送本轮视频；
- 可以在本轮引用一条带视频的消息；
- 视频会作为当前主模型的输入，而不是先交给另一个模型生成文字摘要。

如果你希望模型在后续独立回合重新观看同一个视频，请重新引用或重新附加原视频。仅靠旧历史里残留的文字标记，插件不会重新打开本地文件。

## 计费与回退需要注意什么

插件内部不会因为 Agent Plan 请求失败，就自动改发普通 API；两张供应商卡的固定端点彼此独立。

但 AstrBot 自己还有全局 `fallback_chat_models`。如果你把普通 API 模型和 Agent Plan 模型混进同一条全局回退链，AstrBot 仍可能按照你的全局配置跨路线回退。

如果你需要严格控制计费路线，请分别配置回退模型，不要把两种通道混放。

## 常见问题

### 获取到的模型很少

先确认你使用的是 **普通 API** 供应商卡，并检查当前推理 Key 的权限。普通通道展示的是当前凭据真正可见的模型，不会用离线白名单把结果强行补齐。

### QQ 语音提示 `not of valid wav format`

确认正在使用当前版本插件，并在更新插件后完整重启 AstrBot。插件会以实际文件内容为准重新规范音频，而不是只相信临时文件的 `.wav` 后缀。

### 模型没有看见视频

依次检查：

1. 当前模型卡是否勾选 **视频**；
2. 视频是否属于本次发送或本次引用；
3. AstrBot 是否成功把视频附件交到 Provider 请求阶段。

如果视频在更早的媒体下载或转换阶段就失败，聊天 Provider 无法从普通文本里重新推断出原视频。

### 明明选择 Agent Plan，却出现普通 API 调用

检查当前会话的 Provider / 模型选择，以及 AstrBot 全局 `fallback_chat_models`。插件本身不会把 Agent Plan 请求主动改发普通 API。

### 安装或更新后供应商卡没有变化

完整关闭并重新启动 AstrBot。AstrBot 4.26.x 的 Provider 注册表没有适合插件热卸载的安全钩子，因此单纯刷新 Dashboard 可能仍看到旧状态。

## 插件不会替你做什么

- 不根据模型名称猜测图片、音频、视频或工具能力。
- 不因为额度不足自动从 Agent Plan 切到普通按量 API。
- 不替你开启 Agent Plan 的超额后付费。
- 不把 Seedream、Seedance、TTS、ASR 或向量模型塞进语言模型列表。
- 不给聊天卡偷偷注入豆包搜索。
- 不把 Responses API 内置工具假装成 Chat Function Calling 工具。
- 不接管 AstrBot 原生流式、工具结果回传或全局回退策略。

---

# 技术实现与设计边界

以下内容面向希望审计实现、排查协议问题或继续开发插件的人。普通使用者不需要阅读这一部分才能完成安装和配置。

## “原生视频理解”具体指什么

这里的“原生”不是指 AstrBot 4.26 已经完整内置了视频协议，而是指：**视频能力与图片、音频、工具一样，使用同一张模型卡的 `modalities` 能力集合进行选择，并由当前聊天模型直接接收视频输入。**

插件没有再创建一个“是否开启视频”的第二状态，也没有引入视频转述模型。

模型卡中的：

```text
text / image / audio / video / tool_use
```

属于同一个能力维度，其中 `modalities` 是模型级能力选择的唯一真值。

AstrBot 4.26.x 的 Dashboard 原本只有文本、图像、音频、工具四个可见标签。插件在自身生命周期内补齐第五个 `video / 视频` 选项，并在卸载生命周期中移除自己的 schema 包装，不修改 AstrBot 或 Dashboard 源文件。

## 请求路径

插件尽量保留 AstrBot 原生 Chat Provider 的工作流，只扩展音频和视频最后一段协议：

```text
文字 / 图片 / 工具
  └─→ AstrBot 原生 Chat Provider

本轮受信视频 + 模型卡启用 video
  └─→ 火山方舟 video_url

当前 QQ 音频 + 模型卡启用 audio
  └─→ 规范 WAV
      └─→ 火山方舟 input_audio

附件解析或校验失败
  └─→ 显式停止，不伪装成已经理解附件
```

普通 API 与 Agent Plan 共用同一套多模态处理，只在固定 Base URL、模型命名空间与 Provider 身份上分叉。

## 视频附件的信任边界

AstrBot 4.26.x 当前会把视频附件表示成框架生成的文本内容块。插件不会看到一个看起来像本地路径的字符串就直接读取文件，而是同时要求该附件标记存在于 AstrBot 为**当前请求**单独组装的内容部分中。

因此：

- 用户手打一个类似附件路径的字符串不会触发本地文件读取；
- 仅存在于旧历史中的附件标记不会重新打开视频；
- 当前请求中的可信视频附件才会被替换成 `video_url` 内容块。

HTTP(S) 视频可以保持远程引用；本地路径、`file://` 或 Base64 引用通过 AstrBot 的 `MediaResolver` 读取并转换成带 MIME 的 data URL。

## QQ 音频为什么要重新规范

QQ 侧返回的音频内容与临时文件后缀并不总是一致。例如真实 AMR 字节可能被保存到带 `.wav` 后缀的临时文件中。

因此插件不把文件扩展名或 HTTP MIME 当成最终真值，而是对实际内容进行处理：

```text
QQ Record
  → MediaResolver 获取文件
  → 检查真实内容
  → Tencent Silk 必要时先解码
  → ffmpeg 统一转码
  → 16 kHz / 单声道 / 16-bit PCM WAV
  → RIFF/WAVE、编码、非空帧与 25 MB 上限校验
  → input_audio(data=<Base64>, format="wav")
```

这样可以避免把格式不正确的字节仅因为文件名叫 `.wav` 就直接发送给上游。

## 模型元数据策略

普通方舟 `/models` 返回模型后，插件会尽可能把上游明确提供的事实映射到 AstrBot 模型元数据，包括：

- 输入 / 输出模态；
- 上下文窗口；
- 最大输出长度；
- 推理能力；
- 函数工具能力。

对于上游没有明确声明的可选能力，插件采用谨慎默认，不把“字段缺失”解释成“全部支持”。

用户之后仍然可以在模型卡中手动修改能力选择；请求阶段尊重保存后的 `modalities`，不会再建立第二套能力判定状态机。

## Agent Plan 模型命名空间

Agent Plan 在 AstrBot 内部使用：

```text
agentplan/<model>
```

作为本地命名空间。真正发往火山方舟前只移除 `agentplan/` 前缀。

例如：

```text
AstrBot：agentplan/doubao-seed-2.1-turbo
                     │
                     ▼
火山方舟：doubao-seed-2.1-turbo
```

Agent Plan 的推理 Key 不被用来探测一个未公开承诺的 OpenAI 风格 `/models` 路由；插件提供已核对的套餐模型候选，同时允许用户手动填写后续新增模型。

## 独立 Provider 身份

本仓库是从已验证的 0.1.12 实现中分出的独立平行迭代线。为避免与原供应商插件在同一 AstrBot 进程中注册同名 Provider，本插件使用独立身份：

| 身份 | 值 |
| --- | --- |
| 插件 ID | `astrbot_plugin_volcengine_native_video_provider` |
| 显示名 | `火山方舟供应商（原生视频理解）` |
| 普通 API Provider Type | `volcengine_native_video_ark_chat_completion` |
| Agent Plan Provider Type | `volcengine_native_video_agent_plan_chat_completion` |
| 普通 API 默认卡 ID | `volcengine-native-video-ark` |
| Agent Plan 默认卡 ID | `volcengine-native-video-agent-plan` |
| 仓库 | `zjj1280637679-ship-it/huoshanfangzhougongyingshang` |

Provider Type 与默认卡 ID 不沿用原仓库命名，因此两条开发线不会因为注册同名 Provider Type 而直接覆盖彼此。

## 安全、日志与失败策略

- 视频 data URL 与远程签名视频 URL 在 OpenAI SDK DEBUG 请求日志中会被替换为 `[REDACTED_VIDEO_URL]`。
- `input_audio.data` 的音频 Base64 会被替换为 `[REDACTED_AUDIO_BASE64]`。
- 插件不会记录 Authorization Header 或 API Key 内容。
- 视频或音频进入插件后，如果读取、转换或协议校验失败，本轮请求会显式停止，而不是删除附件后继续假装模型已经看见或听见。
- 429 重试路径不会记录 API Key 前缀。

## 已完成的验收

当前稳定实现曾完成以下真实验收：

- 普通 API 与 Agent Plan 均使用 4 秒红蓝顺序测试视频完成 Chat Completions 请求并返回 HTTP 200，模型能够识别前后半段颜色顺序。
- 一条真实 QQ Tencent Silk 语音完成标准化，得到 RIFF/WAVE、16 kHz、单声道、16-bit PCM WAV。
- 同一语音通过普通方舟模型完成真实 Chat Completions 请求并返回 HTTP 200。
- DEBUG 请求日志中的视频 URL 与音频 Base64 均完成脱敏。

真实额度相关测试不会默认主动运行，避免在没有明确授权的情况下消耗正式账户额度。

## 项目沿革

本仓库以原火山方舟供应商插件的稳定 `0.1.12` 为功能基线，随后完成插件 ID、Provider Type、默认供应商卡 ID 与仓库身份的独立化，以便作为“原生视频理解”方向的平行迭代线继续开发。

功能变更历史请查看 [CHANGELOG.md](CHANGELOG.md)。

## 官方资料

- [AstrBot 插件开发指南](https://docs.astrbot.app/dev/star/plugin-new.html)
- [火山方舟普通 Chat API](https://www.volcengine.com/docs/82379/1494384?lang=zh)
- [火山方舟音频理解](https://docs.volcengine.com/docs/82379/2377589?lang=zh)
- [火山方舟视频理解](https://www.volcengine.com/docs/82379/1895586?lang=zh)
- [Agent Plan 快速开始](https://www.volcengine.com/docs/82379/2373738?lang=zh)
- [Agent Plan 套餐概览](https://www.volcengine.com/docs/82379/2366394?lang=zh)
- [Agent Plan / Coding Plan 专用条款](https://www.volcengine.com/docs/82379/2278469?lang=zh)
- [豆包搜索](https://www.volcengine.com/docs/82379/2301412?lang=zh)
- [火山方舟 Python SDK（Apache-2.0）](https://github.com/volcengine/volcengine-python-sdk)
- [火山方舟 Ark CLI](https://github.com/volcengine/ark-cli)

仓库地址：<https://github.com/zjj1280637679-ship-it/huoshanfangzhougongyingshang>
