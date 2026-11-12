# MiniMax H3 AI 视频全流程 Agent

一个面向 AI 视频生产的全流程工作区模板，在 Codex 环境中开发和使用。输入故事、小说片段或创意 brief，按剧本、分镜、资产、提示词、生成、剪辑、审片和记录的顺序推进。

> 本仓库只保留方法、流程和展示片段。模型账号、API 密钥、供应商配置、本地推理环境、生成结果和生产项目文件均不在仓库内。

## 项目功能

- **文本到剧本**：把故事、小说片段或创意 brief 拆成可执行的剧集、场次、节拍和台词。
- **分镜与连续性**：按镜头组织景别、机位、动作、时长和声音桥，记录角色、场景、道具和光线状态的承接。
- **资产与提示词**：管理角色、场景、道具和首尾帧参考，分别按 MiniMax H3、Grok 等模型的规范生成中文提示词。
- **模型接入**：MiniMax H3 作为主要视频模型，接口由使用者自行接入；生图模型同样自行接入；本地 H3 模型需自行开发。
- **付费前校验**：提交前展示提示词、模式、素材、参数、价格依据、预计费用和验收目标。
- **审片与记录**：保存任务 ID、模型、参数、费用、输出路径、文件哈希、技术检查和审片状态。
- **双产线验证**：主产线负责正式制作；五 Skill 实验产线用于同题对比，确认有效后再逐项吸收。
- **可替换接口**：模型、对象存储和生图服务都可以替换，不把任何一个平台写死为默认依赖。

## 工作流

### 主产线

```text
源文本或创意
-> 证据与设定
-> 剧本
-> 分镜与连续性
-> 资产与提示词
-> 付费前确认
-> 逐镜生成与成片复核
-> 剪辑与审片
-> 记录与归档
```

主产线强调单镜完整动作、镜间承接、16:9 画幅和中文提示词。每个生成单元对应一个完整剪辑镜头，过长内容通过分镜、景别、机位和声音桥重新设计。

### 实验产线

```text
novel-outline
-> novel-characters + novel-art
-> novel-script
-> novel-storyboard
```

实验产线保留五 Skill 套件的原生方法，用于同题试制和对比。它的产出统一标记为 `experimental`，确认前不进入正式制作上下文或资产状态。

## 展示片段

### 无剪辑直接合成版

[下载无剪辑直接合成版（1080p 压缩预览）](https://github.com/304939905/minimaxH3-ai-video-agent/releases/download/demo-videos/MiniMax-H3-demo-direct-synthesis-1080p-preview.mp4)

- 2 分 25 秒，1920×1080、24fps、H.264/AAC。
- 58.1 MB 压缩预览版；原始 164.5 MB 文件保留在本地生产记录中。
- 按分镜顺序直接合成，未做人工精剪。
- 用于展示合成后的镜头组织、连续性和技术状态。
- 不是最终成片；字幕、配乐和直接听觉验收未完成。
- 仅用于展示工作流，不代表模型稳定具备同等能力。

### 人工剪辑版

[下载人工剪辑版](https://github.com/304939905/minimaxH3-ai-video-agent/releases/download/demo-videos/MiniMax-H3-demo-human-edited.mp4)

- 58.8 秒，1290×720、30fps、H.264/AAC。
- 在合成素材基础上进行人工剪辑处理，用于展示剪辑后的节奏和观感。
- 这是人工剪辑版产物，不代表模型直接生成结果。
- 仅用于展示工作流，不代表模型稳定具备同等能力。

两段视频通过 GitHub Release 分发，不进入仓库历史；也可以从 [Release 页面](https://github.com/304939905/minimaxH3-ai-video-agent/releases/tag/demo-videos) 下载。

## 使用方法

> **作者亲笔**
>
> 使用方法很简单：把这个项目接入到你的 DSH、WorkBuddy 或 Codex 等环境，然后把小说内容或你的故事内容直接丢给它。想让它一键完成全流程成片，还是得调教一下；建议一个片段一个片段走，反正多试试。各类模型需要自行接入，生图模型和 MiniMax H3 都接入好了才能正常使用。

### 1. 克隆仓库

```powershell
git clone https://github.com/304939905/minimaxH3-ai-video-agent.git
cd minimaxH3-ai-video-agent
```

环境建议：Windows 10/11、PowerShell、Git、Python 3.12、Node.js 20+（五 Skill 套件脚本，已在 v24.18.0 验证）、FFmpeg（剪辑、抽帧、转码时使用）。

### 2. 接入模型

本项目在 Codex 环境中开发和使用，仓库不绑定任何 API 供应商，也不提供可直接使用的账号、密钥、代理或计费服务。

- **MiniMax H3 API**：建议自行接入 MiniMax H3 的官方 API 或你选择的兼容服务，自行完成请求格式、任务提交、查询、下载、错误处理和费用核对。
- **本地 H3 模型**：本仓库不包含本地推理、权重管理、显存调度、服务封装或硬件配置。需要调用本地 H3 模型时，请自行开发这项能力。
- **生图模型**：本仓库不绑定生图平台，请自行选择并接入你需要的模型或服务。

密钥只从环境变量读取，不写入脚本、JSON、日志或仓库。接入约定和最低接口要求见 `docs/模型接入.md`。

### 3. 执行生产流程

按主产线顺序推进：先整理源文本和设定，再写剧本、分镜和资产，随后生成提示词、提交生成、复核结果、剪辑和审片。每一步都保留输入、版本和验收状态。

### 4. 记录与归档

保存任务 ID、模型、渠道、提示词、输入素材、参数、费用依据、输出路径、文件哈希和审片状态。生成成功只证明任务完成；是否采用仍由实际观看和项目验收决定。

## H3 资料与 Skill

> **官方使用手册原文**：[MiniMax H3 官方使用手册（飞书原文）](https://vrfi1sk8a0.feishu.cn/wiki/FIWjwgL33ipnkekzk30crmKUnIh)

- **官方使用手册**：[下载 docx](https://github.com/304939905/minimaxH3-ai-video-agent/releases/download/h3-official-reference/MiniMax-H3-user-manual.docx)，419 MB。SHA-256：`7B34D58BE939E264B57BF2D9419C4D6E9B5257D1CE07780E614645321B6E4E01`。
- **H3 资料库**：`docs/H3/library/`，20 个文本文件，包含模型定位、官方资料核验、提示词框架、模式选择、案例库和验证日志。
- **官方能力卡**：`docs/H3/research/2026-08-06_minimax-h3_official-capability-card.md`。
- **官方示例视频**：[下载打包 zip](https://github.com/304939905/minimaxH3-ai-video-agent/releases/download/h3-official-reference/MiniMax-H3-official-samples.zip)，79 个视频，原始总大小 712.6 MB，压缩包 750.9 MB；文件清单见 `reference/MiniMax-H3-official/samples/MANIFEST.md`。
- **H3 Prompt Skill**：`skills/h3-prompt-writing/`，包含 `SKILL.md`、基础模式和 Ref2VA 参考指南。
- **官方文档入口**：[视频生成指南](https://platform.minimax.io/docs/guides/video-generation)、[创建视频任务 API](https://platform.minimax.io/docs/api-reference/video-generation-v2-create)、[H3-Context-IR API](https://platform.minimax.io/docs/api-reference/video-generation-v2-h3-context-ir)、[H3 再生成 API](https://platform.minimax.io/docs/api-reference/video-generation-v2-regeneration)。

手册和示例视频通过 GitHub Release 分发，不进入 Git LFS 或仓库历史。它们和资料库都是本地学习/研究存档，版权归原发布方；公开分发前请自行确认授权。

## 整合的 Skill

主产线用导演、Grok 和 H3 三个专项 Skill 覆盖正式制作；实验产线用五件套覆盖从大纲到分镜的另一套完整方法。`h3-prompt-writing` 已随仓库放在 `skills/h3-prompt-writing/`；其余 Skill 不在仓库内，需要自行安装。

| Skill | 在项目中的角色 | 主要产出 | 所属产线 |
| --- | --- | --- | --- |
| `cinematic-director` | 导演与预演监督 | 剧本拆解、节拍、导演书、走位、镜头表、关键帧、运镜、声音、剪辑、失败诊断与 QC | 主产线 |
| `grok-video-director` | Grok Imagine 视频提示词 | 文生视频、图生视频、参考图、视频编辑与续写；中文提示词与稳定性检查 | 主产线 |
| `h3-prompt-writing` | MiniMax H3 提示词 | T2VA、I2VA、FL2VA、L2VA、Ref2VA；多模态整合描述、整体声音景观、非叙事音乐 | 主产线 |
| `novel-outline` | 小说改编大纲 | `outline.json`、人物表、爽点表、分集梗概、资产清单 | 实验产线 |
| `novel-characters` | 角色设定 | `cast.json`、角色卡、形象与音色提示词、角色设定图 | 实验产线 |
| `novel-art` | 场景与道具美术设定 | `art.json`、场景/道具设定、空景提示词、一致性锚点 | 实验产线 |
| `novel-script` | 短剧剧本 | `script.json`、场次与节拍、逐句台词、时长折算 | 实验产线 |
| `novel-storyboard` | 分镜与投产包 | `storyboard.json`、H3 提示词、关键帧清单、生成批次单 | 实验产线 |

### 主产线

`cinematic-director` 负责“怎么导演、怎么拆镜头、怎么保持连续性”；`grok-video-director` 和 `h3-prompt-writing` 负责把导演意图转换成对应模型可执行的提示词。三个 Skill 与项目自己的记录、连续性、资产登记和审片流程配合使用。

### 实验产线

五 Skill 套件保留原生方法，用于同题试制和对比。未经确认，不把套件默认值直接覆盖主产线，也不把实验产出登记为正式资产。

### 使用边界

- Skill 产出的 JSON、Markdown、HTML 报告、图片和提示词默认都是实验产物，不自动成为正式设定或资产。
- 实际生图由使用者自行接入的模型执行；Skill 内置生图步骤只作为方法说明，不构成默认渠道。
- H3 和 Grok 的实际提示词默认使用中文；需要语言 A/B 时必须由用户明确提出，并保持素材、模型、时长、画幅和其他参数一致。
- 付费生成、渠道切换、密钥处理和素材授权不由 Skill 自动决定。
- 参考资料、旧提示词和生成结果不能独立成为事实来源。

## 目录

- `reference/`：MiniMax H3 官方参考资料索引，手册和示例视频通过 Release 下载。
- `docs/`：模型接入约定、H3 资料库和官方能力卡。
- `skills/`：`h3-prompt-writing` Skill 文件。
- `demo/`：两段展示片段的说明和 Release 下载链接。
- `.gitignore`：媒体、配置和输出文件的忽略规则。

## 边界

- 仓库不包含源文本、剧集剧本、分镜、角色/场景资产、生成结果或费用记录。
- 仓库不保存视频文件；两段展示视频通过 Release 分发，仅用于展示工作流。
- H3 手册、示例视频、资料库和 Skill 文件是本地学习/研究存档；版权与再分发条件以原发布方为准。
- 示例片段中的角色、场景和画面不构成对任何第三方作品的授权或权利主张。
- 使用者应自行确认输入文本、参考素材、生成内容和音乐的权利，不得将示例片段用于商业用途或再分发。
- 技术通过不等于创作验收；视觉、节奏、表演和声音仍需实际观看确认。

## 支持项目

如果这个项目对你有帮助，欢迎点一个 Star。Star 不会解锁额外内容，也不会改变下载权限。

## 开发

本仓库为纯文本仓库，保存通用说明、H3 资料索引、接入约定和 Skill。展示视频、官方手册和官方示例视频都通过 GitHub Release 分发，不进入仓库历史。

提交前检查：

```powershell
git status
git diff --stat
```

不要把 API Key、签名 URL、生成请求、输出文件和未授权素材提交到仓库。
