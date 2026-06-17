---
name: presentation-to-video
description: >-
  从 PPT 目录（slides.marp.md + narration-script.md）生成 Edge TTS 口播视频。
  使用 Marp 导出 PNG、ffmpeg 合成 MP4。适用于 document-to-presentation
  产出后的视频化。用户提到口播视频、TTS、配音视频、narrated.mp4 时使用。
version: 1.0.0
disable-model-invocation: true
---

# PPT + 口播稿 → 口播视频

将 [`document-to-presentation`](../document-to-presentation/SKILL.md) 产出合成为 **`{产出目录}/video/{basename}/narrated.mp4`**。

## 路径约定

**{项目根}** = 当前工作区或被分析项目的根目录。与上游 skill 一致。

**{产出目录}** = 用户指定的输出目录；若用户未指定，默认为 `{项目根}/项目汇总/`。

## 输入（目录 `{产出目录}/PPT/{basename}/`）

| 文件 | 必需 |
|------|------|
| `narration-script.md` | 是 |
| `slides.marp.md` | 是（优先；保留 tech-briefing 样式） |
| `video-config.yaml` | 否（可放在 PPT 目录或 `{产出目录}/video/{basename}/` 覆盖默认音色等） |

## 输出

```
{产出目录}/video/{basename}/
├── slides/          # PNG 逐页
├── audio/           # MP3 逐页配音
├── segments/        # 逐页 MP4
├── narrated.mp4     # 最终成片（必交付）
└── generation-log.md
```

输入仍从 `{产出目录}/PPT/{basename}/` 读取；视频产物写入 `{产出目录}/video/{basename}/`，与 PPT 目录平级。

## 前置条件

- Node.js + npx（Marp 导出 PNG）
- Python 3.10+ + `pip install -r scripts/requirements.txt`
- ffmpeg + ffprobe（PATH 中可用）

## 工作流

```
视频产出进度：
- [ ] 阶段 0：检测目录与依赖
- [ ] 阶段 1：Marp 导出 slides PNG
- [ ] 阶段 2：解析口播稿 + Edge TTS
- [ ] 阶段 3：ffmpeg 合成 narrated.mp4
- [ ] 阶段 4：校验页数对齐、记录 generation-log.md
```

### 一键生成

```bash
pip install -r ~/.cursor/skills/custom/technology/presentation-to-video/scripts/requirements.txt
python ~/.cursor/skills/custom/technology/presentation-to-video/scripts/generate-video.py \
  "项目汇总/PPT/developer-onboarding-report"
```

**Windows PowerShell：**

```powershell
py -3 -m pip install -r "$env:USERPROFILE\.cursor\skills\custom\technology\presentation-to-video\scripts\requirements.txt"
py -3 "$env:USERPROFILE\.cursor\skills\custom\technology\presentation-to-video\scripts\generate-video.py" `
  "项目汇总/PPT/developer-onboarding-report"
```

### 阶段 0：检测

- 目录含 `narration-script.md` 与 `slides.marp.md`
- `ffmpeg -version`、`ffprobe -version` 可用
- `edge-tts` 已安装（需网络）

### 阶段 4：校验

- 口播解析页数 = PNG 页数
- `narrated.mp4` 存在于 `{产出目录}/video/{basename}/` 且 > 1MB
- 总时长与口播稿预估 ±25%（记入 `{产出目录}/video/{basename}/generation-log.md`）

## 配置

复制 [templates/video-config.yaml](templates/video-config.yaml) 到 `{产出目录}/PPT/{basename}/video-config.yaml` 或 `{产出目录}/video/{basename}/video-config.yaml` 可覆盖：

- `voice`：默认 `zh-CN-YunxiNeural`
- `rate`：诙谐口播可设 `-5%`
- `min_slide_seconds`：章节页保底时长

音色列表见 [references/edge-tts-voices.md](references/edge-tts-voices.md)。

## 口播稿解析

见 [scripts/parse_narration.py](scripts/parse_narration.py)：

- 按 `### 第 N 页｜标题` 分节
- 提取 `**口播：**` 正文
- `**过渡：**` 并入当前页 TTS（最后一页「结束」过渡忽略）

## 反模式

- 仅有 `slides.pptx` 无 `slides.marp.md`（首期不支持 PPTX 转图）
- 口播页数与幻灯片页数不一致仍强行合成
- 未装 ffmpeg 仍声称成片完成

## 配套资源

| 资源 | 路径 |
|------|------|
| README | [README.md](README.md) |
| 主脚本 | [scripts/generate-video.py](scripts/generate-video.py) |
| 解析器 | [scripts/parse_narration.py](scripts/parse_narration.py) |
| 导出 PNG Win | [scripts/export-slide-images.ps1](scripts/export-slide-images.ps1) |
| Edge TTS | [references/edge-tts-voices.md](references/edge-tts-voices.md) |
| ffmpeg | [references/ffmpeg-compose.md](references/ffmpeg-compose.md) |

## 上游 Skill

- [document-to-presentation](../document-to-presentation/SKILL.md) — 生成 slides + 口播稿
