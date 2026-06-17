# faquan.yao's skills repository

## 产出路径约定

所有 Skill 的生成物写入 **{产出目录}**：

| 规则 | 说明 |
|------|------|
| 用户指定输出目录 | `{产出目录}` = 用户指定目录，产物直接写入其下 |
| 用户未指定 | `{产出目录}` = `{项目根}/项目汇总/`（默认） |

**{项目根}** = 当前工作区或被分析项目的根目录（不是 skill 安装目录 `~/.cursor/skills/`）。

| Skill | 草稿 | 最终产出 |
|-------|------|----------|
| [open-source-dev-onboarding-extractor](technology/open-source-dev-onboarding-extractor/SKILL.md) | `{产出目录}/dev-onboarding-scratch.md` | `{产出目录}/developer-onboarding-report.md` |
| [open-source-user-intro-extractor](technology/open-source-user-intro-extractor/SKILL.md) | `{产出目录}/user-intro-scratch.md` | `{产出目录}/user-project-info.md` |
| [document-to-presentation](technology/document-to-presentation/SKILL.md) | `{产出目录}/PPT/{basename}/presentation-scratch.md` 等 | `{产出目录}/PPT/{basename}/slides.pptx`、`narration-script.md` |
| [presentation-to-video](technology/presentation-to-video/SKILL.md) | `{产出目录}/video/{basename}/generation-log.md` | `{产出目录}/video/{basename}/narrated.mp4` |

`{basename}` = 源文档文件名（不含扩展名）。不得写入 skill 安装目录或源文档同目录。
