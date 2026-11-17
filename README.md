# MinnanSpeech-100h

面向闽南语语音研究的约 100 小时对话语音数据，包含普通话字幕文本、双路自动识别参考、保守说话人时间段及质量信息。

**当前状态：项目说明已公开，完整音频下载入口尚未上线。** 不要将本仓库上线理解为音频包已经发布，或原始媒体的再分发授权已经完成核实。下载安排见 [DOWNLOAD.md](DOWNLOAD.md)。

## 数据概况

| 项目 | 数值或说明 |
| --- | --- |
| 实际音频时长 | 100.0006525 小时 |
| 音频片段 | 121,018 段 |
| 格式 | FLAC，16 kHz，单声道，16-bit PCM |
| 单段时长 | 1.000–14.958 秒 |
| 内容 | 来自公开可访问网络媒体的闽南语影视对话，涵盖日常交流和情绪化表达 |
| 文字 | 字幕原文、简体字幕文本、两路 ASR 自动识别结果 |
| 说话人 | 自动说话人区间和集内编号；不确定区间保留为空 |
| 人工核验 | 未逐条人工听审，不属于人工精标语料 |

音频整理时保留既有无损导出的字节，没有为凑整时长截断最后一个片段。16 kHz 是处理后音频的采样率，不代表原始媒体的录制采样率。

## 可以研究什么

- 闽南语语音到普通话书面文本的识别与语音翻译。
- 自动字幕对齐、低资源方言数据筛选和噪声标签学习。
- 使用已有时间段标签探索对话说话人处理；这些标签不是人工说话人基准。

**中文字幕大多是闽南语的普通话意译，不是闽南语逐字稿。** 不能不区分标签性质，直接将其当作逐字转写或词级强制对齐的正确答案。两路 ASR 输出也不是人工参考答案；本项目尚未报告微调收益或独立人工准确率。

## 标签与文件

计划提供的音频包包括：

```text
audio/<source_group>/<hash_prefix>/<clip_id>.flac
segments.jsonl         # 主清单：文本、时间、说话人和质量信息
transcripts.tsv       # 便于浏览的文本表
speaker_turns.rttm     # 仅包含已赋编号的自动说话人区间
review_samples.jsonl  # 分层抽检入口，不表示已人工通过
schema.json           # 字段和类型说明
SHA256SUMS            # 文件完整性校验
```

对外说明使用来源组编号，不列具体媒体作品名称。内部溯源记录保留；编号化不表示匿名化了声音，也不改变原始内容的权利归属。

说话人编号仅限同一 `episode_id` 内使用，不保证跨集是同一人。多人重叠或证据不足的位置不强行赋编号；RTTM 中没有标签的地方不能当作静音。详见 [ANNOTATIONS.md](ANNOTATIONS.md)。

## 质量与限制

- 采用原流程 A 档及附加机器规则筛选；这是流程等级，不是人工准确率。
- 已对所选音频进行完整解码、帧数、文件与 PCM 哈希、时间轴和包内精确重复检查。
- 63,525 个片段至少包含一个已赋编号的自动说话人区间，共 69,248 个已赋编号区间。
- 已准备 126 条分层抽检入口，但未完成逐条人工听审。
- 音乐、噪声、叠话漏检、字幕意译、首尾对齐误差和说话人误判仍可能存在。
- 不提供默认随机训练/验证/测试划分，不声称说话人隔离或与其他公开语料零重叠。

详见 [DATA_CARD.md](DATA_CARD.md) 和 [统计信息](metadata/statistics.json)。

## 来源与研究用途声明

本数据整理自公开可访问的网络媒体，仅用于研究。原始音视频与字幕的权利归原权利人所有。本项目不声称拥有原始媒体版权，也不声称已逐一取得全部权利人的再分发授权。

“公开可访问”和“仅用于研究”不等于公开再分发已经获得授权。本仓库不以代码或文档许可证替代音频、字幕和相关第三方权利许可。完整条款与权利反馈方式见 [NOTICE.md](NOTICE.md)。

## 联系

项目维护者：[sssggghhh007](https://github.com/sssggghhh007)。一般问题可通过本仓库 Issues 反馈；权利问题请先说明相关片段 ID，不要在公开评论中贴身份证件或其他敏感材料。

---

**English summary:** MinnanSpeech-100h is a curated 100-hour Minnan conversational speech collection with Mandarin subtitle-derived text, automatic ASR hypotheses, conservative episode-scoped speaker intervals, and quality metadata. Text and speaker labels are not manually verified ground truth. Repository documentation is public; the full audio download is not yet available. Source-media redistribution rights have not been independently established. See the data card and rights notice before use.
