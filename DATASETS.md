# 数据集发布清单（DATASETS）

## Releases

| Release | 内容 | 日期 |
|---|---|---|
| **engineering-v0.1（最新）** | Groove 鼓窗口 10,208 个 / 结构标注 30,336 条 / DAWproject XSD / 参数样本 900 条 | 2026-11-07 |

下载：

```bash
gh release download engineering-v0.1 -R ShokouP/open-music-engineering-data -D data
cd data && for f in *.tar.gz; do tar xzf "$f"; done
```

## v0.1 附件与校验和

| 文件 | 大小 | sha256 |
|---|---|---|
| groove-midi.tar.gz | 4.1 MB | `93166e79c529f6a3782ec5a6a720f5a35cd22363d019f089a066f8cb1ee7c523` |
| salami-annotations.tar.gz | 0.35 MB | `76feb7af65f0f1944fde0ffc20b5c53167760ad88fb5ca3d733c9fa6bae963f5` |
| dawproject.tar.gz | 3.7 KB | `08fc139a4e74d6f8bb8b143711d01ba3313151df88db7fd129a9ddda0b67f155` |
| samples.tar.gz | 0.16 MB | `84f46d5da349677f7d154facee1557e1262cded738d9e2324aee7d3328515850` |
| registry-symbolic.lock.json | 2.8 KB | `c18c2eabd5959ea8408258ec217ab9928208db7cf1d4de511d16182b29b76ee9` |
| README.md | 3.9 KB | `997087b245741e037e63b06c102baa40fe59a56b9e87633c83ede10989fce760` |
| LICENSE | 636 B | `764431785bee97785cab3117a218896a0488ef89a8a0e3f861840d945703022d` |
| LICENSE-DATA.md | 504 B | `c7feb034ddec7363c247a61d786a41476bfbf5bd85af61625fa29b6b85ca4505` |

## 数据集说明

- `groove-midi/`：鼓演奏事件 + 两小节窗口；官方 split；CC BY 4.0（派生 JSONL，不再分发原始 MIDI）
- `salami-annotations/`：功能段落边界；song 分组 90/5/5；CC0 1.0
- `dawproject/`：DAWproject Project.xsd（MIT）
- `samples/`：mix/form/texture 参数样本各 300 条（CC0）
- `tools/` + `registry/symbolic.lock.json`：采集/校验脚本与版本锁

## 相关

- 文本 CPT 语料：https://github.com/ShokouP/open-music-production-corpus
