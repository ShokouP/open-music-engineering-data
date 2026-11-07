# 1 万个鼓 Pattern + 3 万条结构标注：开放音乐工程数据 v0.1

> 音乐 AI 不只需要文本知识，还需要**可执行的符号/工程数据**：鼓演奏事件、歌曲结构边界、DAW 工程格式、参数补丁样本。
> 这是与文本语料库互补的独立数据发布。

[![GitHub stars](https://img.shields.io/github/stars/ShokouP/open-music-engineering-data?style=social)](https://github.com/ShokouP/open-music-engineering-data)

**10,208 个两小节鼓 Pattern · 445,949 个鼓击打事件 · 30,336 条结构边界 · 900 条参数样本**

如果这些数据对你有用，欢迎给仓库点个 Star，让更多做音乐 AI 的人找到它。

## 数据集

| 数据集 | 内容 | 规模 | 许可 |
|---|---|---|---|
| **groove-midi** | Groove MIDI Dataset 派生的鼓演奏事件与完整两小节窗口（保留原 tick/音高/力度，Roland TD-11 映射） | 1,149 performances / 445,949 hits / **10,208 windows**；官方 split | CC BY 4.0 |
| **salami-annotations** | SALAMI 结构标注：功能段落边界（seconds + label），按 song 确定性 90/5/5 切分 | 1,359 songs / 2,243 annotations / **30,336 boundaries** | CC0 1.0 |
| **dawproject** | DAWproject 开放工程格式 XSD（跨 DAW 工程参数交换） | Project.xsd | MIT |
| **samples** | DAWcoder 任务级参数样本（mix/form/texture 各 300 条） | 900 rows | CC0 1.0 |

## 文件与校验和

| 文件 | 大小 | sha256 |
|---|---|---|
| groove-midi.tar.gz | 4.1 MB | `93166e79c529f6a3782ec5a6a720f5a35cd22363d019f089a066f8cb1ee7c523` |
| salami-annotations.tar.gz | 0.35 MB | `76feb7af65f0f1944fde0ffc20b5c53167760ad88fb5ca3d733c9fa6bae963f5` |
| dawproject.tar.gz | 3.7 KB | `08fc139a4e74d6f8bb8b143711d01ba3313151df88db7fd129a9ddda0b67f155` |
| samples.tar.gz | 0.16 MB | `84f46d5da349677f7d154facee1557e1262cded738d9e2324aee7d3328515850` |
| registry-symbolic.lock.json | 2.8 KB | `c18c2eabd5959ea8408258ec217ab9928208db7cf1d4de511d16182b29b76ee9` |
| README.md | 3.9 KB | `997087b245741e037e63b06c102baa40fe59a56b9e87633c83ede10989fce760` |
| LICENSE (Apache-2.0) | 636 B | `764431785bee97785cab3117a218896a0488ef89a8a0e3f861840d945703022d` |
| LICENSE-DATA.md | 504 B | `c7feb034ddec7363c247a61d786a41476bfbf5bd85af61625fa29b6b85ca4505` |

下载：

```bash
gh release download engineering-v0.1 -R ShokouP/open-music-engineering-data -D data
cd data && for f in *.tar.gz; do tar xzf "$f"; done
```

## 结构速览

**鼓窗口**：`{"task":"drum_pattern","bpm":94,"style":"neworleans/secondline","beat_type":"beat","start_tick":0,"bars":2,"ticks_per_beat":480,"hits":[[tick,pitch,velocity],...]}`

**结构标注**：`{"task":"section_annotations","song_id":"10","split":"train","boundaries":[{"time_seconds":0.0,"label":"Silence"},{"time_seconds":53.16,"label":"Verse"},...]}`

**参数样本**：`{"task":"mix","state":{...},"intent":"...","patch":{"ops":[...]}}`

## 用途

- 鼓模式条件生成/分析（style + bpm + beat_type → note 序列）
- 歌曲结构分割（音频 → 段落边界；需自备音频）
- DAW 工程参数预测（state + intent → patch）
- 与文本语料联合训练：`open-music-production-corpus`（CPT 文本）+ 本仓库（符号/工程数据）

## 许可与署名

- **groove-midi**：CC BY 4.0 — Groove MIDI Dataset, Google LLC（Gillick et al., ICML 2019）；保留署名与修改说明；**只发布派生 JSONL，不再分发原始 MIDI/音频**。
- **salami-annotations**：CC0 1.0 — DDMAL / SALAMI（Smith et al., ISMIR 2011）；不含音频。
- **dawproject**：MIT — Bitwig。
- **samples**：CC0 1.0 — DAWcoder 自建。
- 逐数据集署名见各目录 `ATTRIBUTIONS.md`，混合许可见 `LICENSE-DATA.md`。

## 已知限制

- 不含音频，也不含原始 MIDI 二进制（仅派生 JSONL）；
- 鼓音高为 Roland TD-11 映射，不是 GM；
- SALAMI 时间为秒，不做 BPM/小节推断；
- 参数样本是合成任务数据，不代表真实工程分布。

## 姊妹项目

- 文本 CPT 语料：https://github.com/ShokouP/open-music-production-corpus
