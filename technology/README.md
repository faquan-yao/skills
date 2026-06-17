# Technology Skills — 开源项目 → PPT → 视频

本目录包含一条可串联的产出管线，用于从开源仓库提取信息、生成演讲材料，再合成为口播视频。

```
开源仓库
    │
    ├─► open-source-user-intro-extractor ──► user-project-info.md ──┐
    │   （面向使用者：产品宣讲、获客介绍）                              │
    │                                                                 ├──► document-to-presentation ──► slides.pptx + narration-script.md
    └─► open-source-dev-onboarding-extractor ──► developer-onboarding-report.md ──┘
        （面向开发者：贡献 onboarding、技术分享）                              │
                                                                              ▼
                                                              presentation-to-video ──► narrated.mp4
```

## Skill 一览

| 阶段 | Skill | 适用场景 | 最终产出 |
|------|-------|----------|----------|
| 1a | [open-source-user-intro-extractor](open-source-user-intro-extractor/SKILL.md) | 终端用户向介绍、产品宣讲素材 | `{产出目录}/user-project-info.md` |
| 1b | [open-source-dev-onboarding-extractor](open-source-dev-onboarding-extractor/SKILL.md) | 开发者 onboarding、架构与贡献指南 | `{产出目录}/developer-onboarding-report.md` |
| 2 | [document-to-presentation](document-to-presentation/SKILL.md) | 将 Markdown 转为专业 PPT + 诙谐口播稿 | `{产出目录}/PPT/{basename}/slides.pptx`、`narration-script.md` |
| 3 | [presentation-to-video](presentation-to-video/SKILL.md) | 将 PPT 目录合成为配音视频 | `{产出目录}/video/{basename}/narrated.mp4` |

阶段 1 的两个 Skill **二选一或都做**，取决于受众；阶段 2、3 按顺序执行。

## 路径约定

与仓库根 [README.md](../README.md) 一致：

| 符号 | 含义 |
|------|------|
| **{项目根}** | 当前工作区或被分析开源项目的根目录 |
| **{产出目录}** | 用户指定目录；未指定时默认为 `{项目根}/项目汇总/` |
| **{basename}** | 源文档文件名（不含扩展名），如 `user-project-info` |

完整目录示例（默认 `{产出目录}`）：

```
{项目根}/
└── 项目汇总/
    ├── user-project-info.md              # 阶段 1a
    ├── developer-onboarding-report.md    # 阶段 1b
    ├── PPT/
    │   └── user-project-info/
    │       ├── slides.marp.md
    │       ├── slides.pptx
    │       └── narration-script.md
    └── video/
        └── user-project-info/
            └── narrated.mp4
```

## 常规使用流程

### 阶段 1：分析开源项目

在 Cursor 中打开目标仓库（或 clone 到工作区），按需调用提取 Skill。

**使用者向（产品宣讲、对外介绍）：**

```
用 open-source-user-intro-extractor 分析当前仓库，产出写到 项目汇总/
```

产出：`{产出目录}/user-project-info.md`（六段叙事：钩子、痛点、转变、差异化、降低行动成本、信任收尾）

**开发者向（贡献指南、技术 onboarding）：**

```
用 open-source-dev-onboarding-extractor 分析当前仓库，产出写到 项目汇总/
```

产出：`{产出目录}/developer-onboarding-report.md`（十维：问题域、系统上下文、主执行链路、架构原则、代码地图等）

两个 Skill 可独立使用，也可先后各跑一遍，分别生成两份源文档供不同场合使用。

### 阶段 2：生成 PPT 与口播稿

以阶段 1 产出的 Markdown 为输入，调用 `document-to-presentation`（需在对话中**显式提及**，不会自动触发）。

```
用 document-to-presentation 把 项目汇总/user-project-info.md 做成 15 分钟产品宣讲，幽默度中等
```

或：

```
用 document-to-presentation 把 项目汇总/developer-onboarding-report.md 做成 20 分钟技术分享
```

Agent 将依次完成：事实内核 → 幻灯片大纲 → Marp 源稿 → 导出 `slides.pptx` → 撰写 `narration-script.md`。

**必交付：** `{产出目录}/PPT/{basename}/slides.pptx`、`narration-script.md`

**环境：** Node.js + npx（导出 PPTX）

详见 [document-to-presentation/README.md](document-to-presentation/README.md)。

### 阶段 3：生成口播视频

PPT 与口播稿就绪后，调用 `presentation-to-video`。

```
用 presentation-to-video 把 项目汇总/PPT/user-project-info 生成口播视频
```

或本地一键执行（路径相对 `{项目根}`）：

**Windows：**

```powershell
py -3 -m pip install -r "$env:USERPROFILE\.cursor\skills\custom\technology\presentation-to-video\scripts\requirements.txt"
py -3 "$env:USERPROFILE\.cursor\skills\custom\technology\presentation-to-video\scripts\generate-video.py" `
  "项目汇总/PPT/user-project-info"
```

**macOS / Linux：**

```bash
pip install -r ~/.cursor/skills/custom/technology/presentation-to-video/scripts/requirements.txt
python ~/.cursor/skills/custom/technology/presentation-to-video/scripts/generate-video.py \
  "项目汇总/PPT/user-project-info"
```

**必交付：** `{产出目录}/video/{basename}/narrated.mp4`

**环境：** Node.js、Python 3.10+、ffmpeg、网络（Edge TTS）

详见 [presentation-to-video/README.md](presentation-to-video/README.md)。

## 端到端示例

以某开源项目为例，从产品介绍到成片的一条完整链路：

```
# 1. 提取使用者向信息
用 open-source-user-intro-extractor 分析当前仓库

# 2. 生成 PPT + 口播稿
用 document-to-presentation 把 项目汇总/user-project-info.md 做成 15 分钟产品宣讲

# 3. 合成视频
用 presentation-to-video 把 项目汇总/PPT/user-project-info 生成口播视频
```

若面向内部开发者培训，将阶段 1 换为 `open-source-dev-onboarding-extractor`，源文档改为 `developer-onboarding-report.md`，后续步骤相同。

## 各阶段输入 / 输出对照

| 步骤 | 输入 | 输出 |
|------|------|------|
| 1a 使用者提取 | 开源仓库 README / docs | `user-project-info.md` |
| 1b 开发者提取 | 开源仓库源码 / 构建脚本 | `developer-onboarding-report.md` |
| 2 生成 PPT | 上述任一 `.md` 文档 | `PPT/{basename}/slides.pptx`、`narration-script.md` |
| 3 生成视频 | `PPT/{basename}/`（需含 `slides.marp.md` + `narration-script.md`） | `video/{basename}/narrated.mp4` |

## 注意事项

- **产出目录统一：** 整条管线使用同一 `{产出目录}`，避免阶段 2、3 找不到上游文件。
- **basename 对齐：** 阶段 3 的路径是 `PPT/{basename}/`，`{basename}` 与源文档文件名一致（如 `user-project-info.md` → `PPT/user-project-info/`）。
- **视频依赖 Marp 源稿：** `presentation-to-video` 需要 `slides.marp.md`，不能仅用 `slides.pptx`。
- **显式调用：** `document-to-presentation` 与 `presentation-to-video` 设置了 `disable-model-invocation: true`，须在对话中明确提及才会执行。

## 延伸阅读

| Skill | README | Agent 工作流 |
|-------|--------|--------------|
| open-source-user-intro-extractor | — | [SKILL.md](open-source-user-intro-extractor/SKILL.md) |
| open-source-dev-onboarding-extractor | — | [SKILL.md](open-source-dev-onboarding-extractor/SKILL.md) |
| document-to-presentation | [README.md](document-to-presentation/README.md) | [SKILL.md](document-to-presentation/SKILL.md) |
| presentation-to-video | [README.md](presentation-to-video/README.md) | [SKILL.md](presentation-to-video/SKILL.md) |
