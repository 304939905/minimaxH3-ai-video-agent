# MiniMax H3：官方能力与约束资料卡

研究日期：2026-08-06  
证据范围：仅 MiniMax 官方开放平台文档；产品手册中的方法论另见项目内《MiniMax H3：AI 视频生产研究笔记》。

## 先记住的结论

MiniMax H3 是开放的通用多模态视频模型：以文字、图像、视频和音频统一理解为输入，可做文生视频、首/尾帧图生视频、参考生成与视频编辑。输出为 **768P 或 2K**，时长为 **4–15 秒整数**。[官方视频生成指南](https://platform.minimax.io/docs/guides/video-generation)

| 目标 | 正确入口 | 使用原则 |
| --- | --- | --- |
| 从零探索概念 | 文生视频 | 仅传 `text`；必须指定固定画幅，不能用 `adaptive`。 |
| 锁定画面起点、终点或两者之间的变化 | 首/尾帧图生视频 | 使用 `first_frame` / `last_frame`；成片画幅由输入图决定。 |
| 借用人物、动作、运镜、风格、声音或剪辑节奏 | 参考生成 | 使用 `reference_image`、`reference_video`、`reference_audio`；可混合使用。 |
| 先把复杂参考整理成可用提示词 | H3-Context-IR | 只返回增强后的结构化提示词，不生成视频。 |
| 将合格的 768P H3 片升级为 2K | Regeneration | 需复现原 `content`，并额外传入一个 `base_video`。 |

图生与参考生成**互斥**：一旦使用任意 `reference_*`，就不能再同时使用 `first_frame` / `last_frame`。每个请求还必须有一个非空 `text` 项。[创建视频任务 API](https://platform.minimax.io/docs/api-reference/video-generation-v2-create) [H3-Context-IR API](https://platform.minimax.io/docs/api-reference/video-generation-v2-h3-context-ir)

## 素材预算（制作前硬检查）

- 首/尾帧：总计 0–2 张；单张边长 256–5760 px、比例 2:5–5:2。
- 参考图：最多 9 张；同样为 256–5760 px。
- 参考视频：最多 3 条，每条 2–15 秒，合计最多 15 秒；MP4/MOV、H.264/H.265，音频 AAC/MP3，23.976–60 fps。
- 参考音频：最多 3 条，每条 2–15 秒，合计最多 15 秒；WAV/MP3。
- 全部混合素材最多 12 个；单文件视频 ≤50 MB、图片 ≤30 MB、音频 ≤15 MB；请求体 ≤64 MB（大文件优先使用公网 URL）；提示词 ≤7000 字符。

以上均为当前官方 API 限制。[官方视频生成指南](https://platform.minimax.io/docs/guides/video-generation) [创建视频任务 API](https://platform.minimax.io/docs/api-reference/video-generation-v2-create)

## 可执行的提示词工作法

1. **先写一条文本主线，再挂素材角色。** 文本不是可选附注；它要说清主体、地点、事件、镜头变化和声音意图。参考素材只负责“参考什么”。
2. **一条素材一个职责。** 例如 `reference_image` 锁角色或服装，`reference_video` 锁动作/运镜，`reference_audio` 锁音色或节奏；不要让同一段参考同时承担互相冲突的角色、风格和镜头命令。
3. **把复杂画面先交给 Context-IR。** 它能分析文字、图、音频、视频间关系，并在保留原意的前提下输出语义更完整的结构化提示词；成功后从 `content.prompt` 取回，作为生成前的提示词草案。
4. **生成任务是异步的。** 创建任务后用 `task_id` 查询；成功状态为 `succeeded`，下载地址在 `task.content.url`。官方示例使用 10 秒查询间隔。

上述流程与字段均见官方指南/API；第 2 条为基于其角色化 `content` 设计作出的制作建议。[官方视频生成指南](https://platform.minimax.io/docs/guides/video-generation) [H3-Context-IR API](https://platform.minimax.io/docs/api-reference/video-generation-v2-h3-context-ir)

## 模式选择的常见误区

- **把首尾帧与参考素材堆在同一次请求里**：接口不允许，须拆成不同生成策略。
- **文生视频未给画幅**：文生时 `ratio` 必填，且不可为 `adaptive`；可用 21:9、16:9、4:3、1:1、3:4、9:16。
- **误把 Context-IR 当作生成接口**：它只给增强提示词，仍需再发起视频生成任务。
- **高分辨率一开始就长片押注**：H3 支持先生成 768P，再以满足规格的源视频做 2K 再生成；适合先验证镜头与素材关系，再提升交付规格。

## 官方来源

- [MiniMax H3 视频生成指南](https://platform.minimax.io/docs/guides/video-generation)
- [创建 MiniMax H3 视频生成任务](https://platform.minimax.io/docs/api-reference/video-generation-v2-create)
- [创建 H3-Context-IR 任务](https://platform.minimax.io/docs/api-reference/video-generation-v2-h3-context-ir)
- [创建 H3 视频再生成任务](https://platform.minimax.io/docs/api-reference/video-generation-v2-regeneration)
