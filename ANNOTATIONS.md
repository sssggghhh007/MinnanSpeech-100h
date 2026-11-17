# 标签使用说明

主清单使用每行一个 JSON 对象的 JSONL。字段以最终对外包中的 `schema.json` 和实际记录为准；本页解释关键含义，不是严格 JSON Schema。

| 字段 | 含义与边界 |
| --- | --- |
| `id` | 片段稳定编号，不是说话人编号 |
| `audio` | 相对数据包根目录的音频路径 |
| `series` / `series_title` | 对外来源组编号或概括名称，不列具体作品名 |
| `episode_id` | 集级来源编号，也是说话人编号的有效范围 |
| `duration_s`, `sample_rate`, `channels`, `frames` | 片段时长与实际音频信息；精确总时长按帧数计算 |
| `source_audio_start_s` / `source_audio_end_s` | 在提取后源音频中的片段位置 |
| `subtitle_text` | 字幕原文，可能混合繁简体；主要为普通话意译 |
| `text` | 字幕转换后的简体文本，不代表逐字纠错 |
| `asr_a` / `asr_b` | 两路自动识别原输出，不是人工正确答案 |
| `cer_a` / `cer_b` | 历史机器/字幕差异值；不是人工评测准确率 |
| `grade` | 原生产流程的机器分档；本候选集选 A 档 |
| `speaker_segments` | 自动说话人时间段；单人证据不足则编号为空 |
| `speaker` | 单一已知编号的便利字段，不代表未知部分也已识别 |
| `speaker_scope` | 集内有效，不保证跨集编号对应同一个人 |
| `overlap_speech` | 自动检测结果；false 不等于人工确认绝无叠话 |
| `legacy_*` | 历史整句视听推断线索，不是新区间的人工属性 |
| `edge_fix` | 可空布尔值；旧流程补救标记，不是修复明细或人工验收 |
| `acoustic_qc` | 声学筛选信息，能量活动率不能直接当作 VAD 话音占比 |
| `sha256` / `pcm_sha256` | 文件和解码 PCM 的完整性与精确重复检查标识 |
| `human_verified` | 是否人工核验；本候选集没有逐条人工验收 |

## 说话人时间轴

`speaker_segments` 内：

- `start` / `end`：相对当前片段起点的秒数。
- `source_start` / `source_end`：相对提取后源音频起点的秒数。
- `speaker`：自动集内编号；空字符串表示未知。
- `kind` / `confidence`：自动区间类型和模型线索，不是人工正确概率。

轮流说话可产生多个区间；重叠或不确定区间不强行指定人物。没有承诺每个片段的全部话音都有编号。

`speaker_turns.rttm` 只列已赋编号区间，未知区间仍应从 JSONL 读取。RTTM 中未标注的部分不能当作静音或负样本。当前说话人时间轴没有词级文本对应关系。

## 一个合成示意（不是实际语料）

```json
{
  "id": "illustration_only",
  "episode_id": "source_group_example_episode_example",
  "duration_s": 3.0,
  "speaker_scope": "episode",
  "speaker_segments": [
    {"start": 0.2, "end": 1.2, "speaker": "speaker_01"},
    {"start": 1.2, "end": 1.5, "speaker": "", "kind": "unknown"},
    {"start": 1.5, "end": 2.8, "speaker": "speaker_02"}
  ],
  "human_verified": false
}
```

## 使用建议

训练前保留字幕和 ASR 的来源区别。独立评测集需人工核验，并与训练素材隔离。不要用本批用于筛选的 ASR 自己给出的低差异值，冒充对该 ASR 的独立评测结果。
