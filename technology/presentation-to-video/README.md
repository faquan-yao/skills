# presentation-to-video

将 **PPT 目录**（`slides.marp.md` + `narration-script.md`）合成为 **Edge TTS 口播视频**。

- **版本：** 1.0.0  
- **位置：** `~/.cursor/skills/custom/technology/presentation-to-video/`  
- **上游：** [document-to-presentation](../document-to-presentation/README.md)

## 快速开始

```
用 presentation-to-video 把 报告/PPT/developer-onboarding-report 生成口播视频
```

或本地执行：

```powershell
py -3 -m pip install -r "$env:USERPROFILE\.cursor\skills\custom\technology\presentation-to-video\scripts\requirements.txt"
py -3 "$env:USERPROFILE\.cursor\skills\custom\technology\presentation-to-video\scripts\generate-video.py" `
  "d:\work\github\Agent-Reach\报告\PPT\developer-onboarding-report"
```

产出：`报告/PPT/{basename}/video/narrated.mp4`

## 管线

```
slides.marp.md  ──► video/slides/*.png     (Marp CLI)
narration-script.md ──► video/audio/*.mp3  (Edge TTS)
PNG + MP3 ──► video/segments/*.mp4 ──► narrated.mp4  (ffmpeg)
```

## 环境依赖

| 依赖 | 安装 |
|------|------|
| Node.js | Marp 导出 PNG |
| Python 3.10+ | `pip install -r scripts/requirements.txt` |
| ffmpeg | `winget install Gyan.FFmpeg`（Windows） |

pip 若遇代理 SSL 错误，可试：

```powershell
$env:NO_PROXY="*"
py -3 -m pip install -r scripts/requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
```

## 配置（可选）

在 `PPT/{basename}/video-config.yaml`：

```yaml
voice: zh-CN-YunxiNeural
rate: "-5%"
output: video/narrated.mp4
```

## 目录结构

```
presentation-to-video/
├── README.md
├── SKILL.md
├── references/
├── templates/video-config.yaml
└── scripts/
    ├── generate-video.py
    ├── parse_narration.py
    ├── export-slide-images.ps1
    └── requirements.txt
```

## 已知限制

- 需要网络（Edge TTS 在线）
- TTS 语气不如真人；幽默效果有限
- 视频画面以 Marp PNG 为准，与 PPTX 可能略有差异
- 首期不支持仅从 PPTX 生成（需 `slides.marp.md`）

## 延伸阅读

- [SKILL.md](SKILL.md) — Agent 工作流
- [edge-tts-voices.md](references/edge-tts-voices.md) — 音色选择
