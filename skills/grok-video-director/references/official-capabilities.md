# Grok Imagine 视频能力核对表

核对日期：2026-08-13。以下内容来自 xAI 官方文档和公告。涉及付费或正式生产时，应再次打开链接核验。

## 目录

- 通用生成模型
- Video 1.5
- 五种请求模式
- 参数限制
- 声音与提示词依据
- 官方来源

## 通用生成模型

`grok-imagine-video` 支持文字、图片和视频输入，可用于文生视频、图生视频、参考图生成、视频编辑与视频续写。

普通生成参数：

- 时长：1–15 秒。
- 画幅：`1:1`、`16:9`、`9:16`、`4:3`、`3:4`、`3:2`、`2:3`。
- 分辨率：`480p` 或 `720p`；官方文档标注普通生成默认 `480p`。
- 图生视频默认沿用输入图画幅；强行指定不同画幅会拉伸输入图，应优先在生成前裁好首帧。

## Video 1.5

`grok-imagine-video-1.5` 是官方已正式发布的图生视频模型。官方模型页明确标注它当前不支持文生视频。不要仅因为模型更新而把所有模式都切到 1.5。

官方公告强调：1.5 改进运动、物理、原生声音、环境声与对白同步；示例支持传入首帧、动作提示词、时长和分辨率。

## 五种请求模式

| 模式 | 核心输入 | 重要边界 |
|---|---|---|
| 文生视频 | `prompt` | 使用 `grok-imagine-video`，不要用只支持图生视频的 1.5 |
| 图生视频 | `prompt` + 一张首帧 | 1.5 可用；首帧承担外观、构图、光线和风格 |
| 参考图生成 | `prompt` + `reference_images` | 最长 10 秒；官方文档标注 1.5 不支持此模式；不能与首帧 `image` 同时使用 |
| 视频编辑 | 视频 + 修改提示词 | 输入 MP4 最长 8.7 秒；不支持自定义时长、画幅、分辨率；输出继承输入并最高 720p |
| 视频续写 | 视频 + 下一拍提示词 | 输入 2–15 秒；新增 2–10 秒，默认新增 6 秒；画幅和分辨率继承输入，最高 720p |

视频编辑和续写的输入应为 `.mp4`，使用兼容 MP4 的 H.264、H.265、AV1 等编码。

## 提示词依据

xAI 对 Video 1.5 的官方说明是：提供起始图像，描述运动，并选择分辨率和时长。预览公告还明确建议用自然语言描述镜头移动、节奏与声音设计；首帧本身负责细节和光线，结果应延续输入画面而不是重新解释它。

因此图生视频提示词优先写变化：

```text
[主动作]，camera [单一镜头路径]，[环境或材质响应]。Audio: [与画面同步的声音]。Preserve: [身份、结构、服装、布局、光线]。
```

这是一种工作模板，不是 xAI 官方强制语法。

## 价格边界

官方模型页显示按视频秒数和分辨率计费，并可能另收图片或视频输入费用。不同模型、分辨率、区域和促销的价格不同。不要把本文件内任何历史价格当作报价；准备调用 API 时，重新检查相应模型页并按本次实际参数估算。

## 官方来源

- 视频生成总览：https://docs.x.ai/developers/model-capabilities/video/generation
- 图生视频：https://docs.x.ai/developers/model-capabilities/video/image-to-video
- 参考图生成：https://docs.x.ai/developers/model-capabilities/video/reference-to-video
- 视频编辑：https://docs.x.ai/developers/model-capabilities/video/editing
- 视频续写：https://docs.x.ai/developers/model-capabilities/video/extension
- 通用视频模型：https://docs.x.ai/developers/models/grok-imagine-video
- Video 1.5 模型：https://docs.x.ai/developers/models/grok-imagine-video-1.5-preview
- Video 1.5 正式公告：https://x.ai/news/grok-imagine-video-1-5
