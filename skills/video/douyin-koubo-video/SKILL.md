---
name: douyin-koubo-video
description: 复刻抖音口播类爆款视频，知识科普、观点输出、干货清单，含脚本公式、AI 配音、大字号硬字幕、BGM 闪避混音和 9:16 成片。
disable-model-invocation: true
---

# 抖音口播爆款

复刻抖音/视频号/Shorts 上最主流的口播形态：真人不出镜或数字人出镜，靠文案钩子、配音节奏、大字幕留住观众。成品是一条 9:16 竖屏带配音、带 BGM、带硬字幕的 MP4。

## 适用平台与规格

| 平台 | 画幅 | 分辨率 | 时长 | 备注 |
| --- | --- | --- | --- | --- |
| 抖音 | 9:16 | 1080x1920 | 90 到 120 秒（默认底线 90 秒） | 前 3 秒定生死 |
| 视频号 | 9:16 | 1080x1920 | 90 到 150 秒 | 中年受众，语速放慢 |
| YouTube Shorts | 9:16 | 1080x1920 | 90 到 180 秒 | 标题带 #Shorts |
| 小红书 | 3:4 | 1080x1440 | 90 到 120 秒 | 封面标题党权重极高 |

默认按抖音 1080x1920 出片，用户指定其他平台时换规格表里的参数。

## 第一步：定选题和钩子

问用户要主题。没有主题就给 3 个候选，按这个标准排序：反常识结论、直接利益（省钱/避坑/提速）、身份共鸣（打工人/宝妈/程序员）。

口播爆款脚本结构，严格按此写稿：

1. **钩子（前 3 秒）**：一句话抛反常识结论或痛点。例："90% 的人跑步都在伤膝盖。" 禁止自我介绍、禁止"大家好"。
2. **身份锚（1 句）**：为什么听你的，例："我做了 8 年运动康复。"
3. **干货 3 点**：每点一个论点加一到两个例子，单点 3 到 4 句。编号用"第一、第二、第三"，方便字幕分段。90 秒以上的片子每点要有足够展开，例子讲具体场景，不要只报结论。
4. **金句收尾加行动指令**：一句可截图的金句，加"关注我，下期讲 XX"。

全文控制在 420 到 500 字（对应 90 到 110 秒，成片不得少于 90 秒）。写完先给用户确认文案，再进制作。

## 第二步：配音（edge-tts）

安装：`pip install edge-tts`。

中文声音按人设选：

- 男声知识感：`zh-CN-YunjianNeural`
- 男声年轻活泼：`zh-CN-YunxiNeural`
- 女声亲和：`zh-CN-XiaoxiaoNeural`
- 女声情感：`zh-CN-XiaoyiNeural`
- 新闻腔：`zh-CN-YunyangNeural`

生成配音，语速加快 8% 更符合抖音节奏：

```bash
edge-tts --voice zh-CN-YunjianNeural --rate=+8% --text "$(cat script.txt)" --write-media voice.mp3 --write-subtitles voice.vtt
```

`--write-subtitles` 输出的时间轴后面直接用来做字幕。用 `ffprobe -v error -show_entries format=duration voice.mp3` 记下总时长 `VO_LEN`。

## 第三步：画面

三条路线，问用户选一条：

- **A 数字人**：用用户已有的数字人工具（腾讯智影、HeyGen、剪映数字人）生成 9:16 口播视频，配音对口型。
- **B 素材流**：找或生成与每段文案对应的画面（图片生成工具、用户素材库），每 2 到 3 秒切一个镜头，用 `scale=1080:1920:force_original_aspect_ratio=increase,crop=1080:1920` 统一规格。
- **C 纯色大字流**：深色渐变底加逐段放大的文字卡，纯 ffmpeg 可做，适合纯观点号。

所有素材统一成 1080x1920、30fps：

```bash
ffmpeg -i in.mp4 -vf "scale=1080:1920:force_original_aspect_ratio=increase,crop=1080:1920,fps=30" -c:v libx264 -crf 18 -an base.mp4
```

按 `VO_LEN` 裁剪或拼接画面到同长。

## 第四步：BGM 与混音

选纯音乐，节奏 90 到 110 BPM，无人声。音乐必须压在人声下面。用侧链压缩实现"人声一响音乐自动让路"：

```bash
ffmpeg -i voice.mp3 -stream_loop -1 -i bgm.mp3 -t "$VO_LEN" -filter_complex \
"[1:a]volume=0.28[bg]; \
 [bg][0:a]sidechaincompress=threshold=0.05:ratio=10:attack=5:release=400[duck]; \
 [0:a][duck]amix=inputs=2:duration=first:normalize=0[mix]; \
 [mix]loudnorm=I=-14:TP=-1.5:LRA=11[out]" \
-map "[out]" -y mixed.m4a
```

响度目标 -14 LUFS，是抖音手机端外放的舒适区。

## 第五步：大字幕

把 `voice.vtt` 转成 SRT（或直接写 ASS）。抖音风格字幕规范：

- 字号约为画面高度的 7%（1080x1920 用 90 到 100）
- 位置在画面下 1/3 偏上，避开底部昵称区和右侧按钮区
- 白字加黑描边，关键词换黄色高亮
- 每屏最多 12 个汉字

用 ASS 烧录示例（样式行）：

```
Style: DY,思源黑体,96,&H00FFFFFF,&H00FFFFFF,&H00000000,&H80000000,-1,0,0,0,100,100,0,0,1,4,0,2,60,60,640,1
```

```bash
ffmpeg -i base.mp4 -i mixed.m4a -vf "ass=subs.ass" -c:v libx264 -crf 18 -preset slow -c:a aac -b:a 192k -movflags +faststart -shortest -y final_douyin.mp4
```

## 第六步：封面和发布文案

- 封面：竖屏 1080x1920，大字标题 8 到 12 字，用视频第一帧加字或单独生成。
- 标题：重复钩子，带 1 个争议点或数字。
- 话题标签：3 到 5 个，1 个大流量词加 2 个垂直词。
- 发布时间：12 点到 14 点或 18 点到 22 点。

## 交付前自检

逐条核对，任何一条不过就修：

1. 静音播放只看字幕，前 3 秒能看懂钩子
2. 配音无机械停顿感，句间气口在 0.4 秒内
3. 手机竖屏预览，字幕不被平台 UI 遮挡
4. 成片时长 90 到 120 秒，最短不少于 90 秒，`VO_LEN` 与画面同长无黑帧
5. 外放听一遍，人声清晰、BGM 不抢戏
