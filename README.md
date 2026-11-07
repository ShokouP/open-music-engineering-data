# Open Music Engineering Data (OMED)

面向音乐 AI 的**符号 / 工程数据**：鼓演奏窗口、歌曲结构标注、DAW 工程格式规范、任务级参数样本。

与姊妹项目 [Open Music Production & Audio Engineering Corpus](https://github.com/ShokouP/open-music-production-corpus)（文本 CPT 语料）互补：那边是"知识文本"，这边是"可执行的工程/符号数据"。

## 数据集

| 数据集 | 内容 | 规模 | 许可 |
|---|---|---|---|
| **groove-midi** | Groove MIDI Dataset 解析后的鼓演奏事件与完整两小节窗口（保留原 tick/音高/力度，Roland TD-11 鼓映射） | 1,149 performances / 445,949 hits / 10,208 windows；官方 split | CC BY 4.0（署名） |
| **salami-annotations** | SALAMI 结构标注：功能段落边界（seconds + label），按 song 分组的确定性 90/5/5 切分 | 1,359 songs / 2,243 annotations / 30,336 boundaries | CC0 1.0 |
| **dawproject** | DAWproject 开放工程格式 XSD 与 MIT 许可（跨 DAW 工程参数交换） | Project.xsd | MIT |
| **samples** | DAWcoder 任务级参数样本（mix/form/texture 各 300 条：工程状态 + 意图 + patch） | 900 rows | CC0 1.0 |

## 结构说明

**Groove windows**（`groove-midi/windows_{split}.jsonl`）：

```json
{"dataset_id":"groove-midi","task":"drum_pattern","group_key":"groove:drummer1/session1/....mid",
 "split":"train","bpm":94,"style":"neworleans/secondline","beat_type":"beat",
 "start_tick":0,"bars":2,"ticks_per_beat":480,
 "hits":[[tick, midi_pitch, velocity], ...],
 "license":"CC-BY-4.0","provenance":[...]}
```

- `hits` 为 note-on 事件 `[tick, pitch, velocity]`，未重新映射到 GM；
- 同一 performance 的所有窗口在同一 split（official split 保留）；
- `performances_*.jsonl` 是整段演奏级数据。

**SALAMI sections**（`salami-annotations/sections_{split}.jsonl`）：

```json
{"dataset_id":"salami-annotations","task":"section_annotations","song_id":"10","split":"train",
 "boundaries":[{"time_seconds":0.0,"label":"Silence"},{"time_seconds":0.223,"label":"Intro"}, ...],
 "group_key":"salami:10","license":"CC0-1.0"}
```

**DAWcoder 参数样本**（`samples/*.jsonl`）：`{"task":"mix","state":{...},"intent":"...","patch":{...}}`，可直接用于参数预测 / SFT。

## 用法

```python
import json
# 鼓模式建模 / 生成
rows = [json.loads(l) for l in open("groove-midi/windows_train.jsonl", encoding="utf-8")]
# 结构分割
secs = [json.loads(l) for l in open("salami-annotations/sections_train.jsonl", encoding="utf-8")]
```

- 鼓模式：条件（style/bpm/beat_type）→ 两小节 note 序列；
- 结构分割：音频（需自备）→ 段落边界；或边界 → 段落标签；
- 参数预测：`state + intent → patch`（配合 DAWcoder 模型 / 规则基线）。

## 复现

```bash
python -m pip install -r tools/requirements.txt
python tools/collect_symbolic.py     # 下载官方包 -> 解析 -> 派生 JSONL + lock
python tools/check_symbolic.py       # 校验 sha256 / split / 泄漏
```

`registry/symbolic.lock.json` 记录上游 URL、archive sha256、逐文件 sha256 与切分策略。

## 许可与署名

- **groove-midi**：CC BY 4.0 — Groove MIDI Dataset, Google LLC（Gillick et al., ICML 2019）；保留署名、标注修改；原始数据不随本仓库/Release 再分发，只发布派生的 JSONL。
- **salami-annotations**：CC0 1.0 — DDMAL / SALAMI（Smith et al., ISMIR 2011）；不包含音频。
- **dawproject**：MIT — Bitwig。
- **samples**：CC0 1.0 — DAWcoder 自建。
- 逐数据集署名见各目录 `ATTRIBUTIONS.md`。

## 已知限制

- 不含音频，也不含原始 MIDI 二进制（仅派生 JSONL）；
- 鼓音高为 Roland TD-11 映射，不是 GM；
- SALAMI 时间为秒，不做 BPM/小节推断；
- Groove 的 style/beat_type 标签沿用官方分类；
- 这些数据是"工程/符号"侧补充，不替代真实 DAW 工程参数数据。
