# Video

复刻各平台爆款短视频的 skill 集合，全部自带音频流水线（AI 配音、BGM、混音、字幕）。覆盖抖音、小红书、B站、YouTube 四大平台的横竖屏规格。这些 skill 未进 plugin，也不在顶层 README 推广，直接安装单个使用：

**全集合用默认时长：成片至少 90 秒起步。** 各 skill 的脚本字数、镜头数、自检清单都已按 90 秒以上校准，用户明确要更短的才允许例外。

```bash
npx skills@latest add hukeying85-ux/skills --skill=<name>
```

- **[douyin-koubo-video](./douyin-koubo-video/SKILL.md)**: 抖音口播爆款，知识科普和观点输出，脚本公式加 AI 配音加大字幕。User-invoked.
- **[novel-story-video](./novel-story-video/SKILL.md)**: 小说推文和民间故事号，AI 插画连环画加悬念旁白，片尾卡高潮。User-invoked.
- **[movie-recap-video](./movie-recap-video/SKILL.md)**: 影视解说，三幕结构文案，B站长版加抖音竖版一次双发。User-invoked.
- **[satisfying-asmr-mix](./satisfying-asmr-mix/SKILL.md)**: 解压治愈 ASMR 混剪，音效设计优先，全平台通发。User-invoked.
- **[emotion-quote-video](./emotion-quote-video/SKILL.md)**: 情感语录号，金句卡片加情感朗读，副歌卡点。User-invoked.
- **[digital-human-selling](./digital-human-selling/SKILL.md)**: AI 数字人带货口播，痛点到逼单五段脚本公式。User-invoked.
- **[beat-sync-vlog](./beat-sync-vlog/SKILL.md)**: 卡点变装和旅拍大片，音乐节拍驱动剪辑点。User-invoked.
- **[gaming-highlight-meme](./gaming-highlight-meme/SKILL.md)**: 游戏高光整活剪辑，梗音效加吐槽字幕。User-invoked.
- **[hot-news-commentary](./hot-news-commentary/SKILL.md)**: 热点快评号，黄金 6 小时极速流水线加事实闸门。User-invoked.
- **[podcast-clip-factory](./podcast-clip-factory/SKILL.md)**: 播客直播切片工厂，爆款打分筛选加卡拉OK 字幕，一鱼多吃。User-invoked.

## 共用依赖

全部 skill 的工具链一致，首次使用任一 skill 前装好：

```bash
pip install edge-tts
# ffmpeg 按系统安装：apt install ffmpeg / brew install ffmpeg
```

- 配音：`edge-tts`（免费，中文声音丰富），声音清单见各 skill 内说明
- 剪辑合成：`ffmpeg`，各 skill 内含可直接执行的配方
- 混音标准：人声侧链压 BGM，整体 `loudnorm=I=-14:TP=-1.5`
- 画面：优先用户素材和图片生成工具，各 skill 注明降级路线

## 选型速查

| 想做的内容 | 用哪个 |
| --- | --- |
| 讲知识、讲观点 | douyin-koubo-video |
| 讲故事、推小说 | novel-story-video |
| 讲电影剧集 | movie-recap-video |
| 无文案纯爽感 | satisfying-asmr-mix |
| 金句情绪共鸣 | emotion-quote-video |
| 卖货种草 | digital-human-selling |
| 颜值旅拍氛围 | beat-sync-vlog |
| 游戏内容 | gaming-highlight-meme |
| 蹭热点 | hot-news-commentary |
| 已有长视频拆条 | podcast-clip-factory |
