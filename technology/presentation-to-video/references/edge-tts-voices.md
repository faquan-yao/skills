# Edge TTS 中文音色参考

## 推荐解说音色

| 音色 ID | 风格 | 适用 |
|---------|------|------|
| `zh-CN-YunxiNeural` | 男声，清晰 | 技术项目介绍（默认） |
| `zh-CN-XiaoxiaoNeural` | 女声，亲切 | 产品宣讲 |
| `zh-CN-YunyangNeural` | 男声，新闻感 | 正式汇报 |
| `zh-CN-XiaoyiNeural` | 女声，活泼 | 轻松分享 |

## 参数

| 参数 | 示例 | 说明 |
|------|------|------|
| `rate` | `+0%`、`-5%` | 语速；诙谐口播可略慢 `-5%` |
| `volume` | `+0%` | 音量 |
| `pitch` | `+0Hz` | 音高 |

## 列出全部音色

```bash
edge-tts --list-voices | findstr zh-CN
```

## 限制

- 需要网络连接
- 语气与停顿不如真人；幽默效果有限
- 长句可拆分为多段合成后 ffmpeg concat（首期按页合成即可）
