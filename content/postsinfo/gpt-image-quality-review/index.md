---
title: "GPT Image 三款模型画质档位实测"
date: 2026-10-19T11:00:00+08:00
lastmod: 2026-10-19T11:11:00+08:00
description: "用 Lucoo 官方 Key 对比 openai/gpt-image-2.5-flare、openai/gpt-image-2.5-sunburst 和 openai/gpt-image-2，在 1536×1024 与 3840×2160 上测试 low 到 max。"
slug: "gpt-image-quality-review"
tags: ["生图", "GPT Image", "中转站", "OpenAI"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
featureimage: "cover.jpg"
---

同一提示词、同一把 Lucoo 官方 Key，对比三款模型在两个分辨率上的画质档位。接口是 `https://lucoo.ai/v1/images/generations`。模型名为 `openai/gpt-image-2.5-flare`、`openai/gpt-image-2.5-sunburst`、`openai/gpt-image-2`。

## 先看结论

Flare 和 Sunburst 同档同价。1536×1024 的 high 大约 $0.043，是比较稳的日常档。3840×2160 的 high 大约 $0.102。max 大约是 high 的 4 倍，细节加在发丝、流苏和伞面上，构图不会因此换成另一种画。

Image 2 只有 low、medium、high。`xhigh` 和 `max` 会返回 400，不扣费。它的档位和 2.5 不是同名同价：

| Image 2 写下的档位 | 实际输出 token | 对应 2.5 的档位 |
| --- | --- | --- |
| low | 与 2.5 low 相同 | low |
| medium | 与 2.5 high 相同 | high |
| high | 与 2.5 max 相同 | max |
| xhigh、max | 不支持 | 2.5 才有 |

2.5 的 medium 和 xhigh 在 Image 2 上没有同价档。

画面倾向上，Flare 更靠近人物，Sunburst 更常把桥、河和灯火画进画面，Image 2 更接近实拍，背景经常收成光斑。每格只出了一张，构图里有随机采样，不能当成模型的固定风格。能对齐的是 token 和价格。

## 测试条件

- Key：Lucoo 官方 Key，请求发到 `https://lucoo.ai/v1`。花费记在这把 Key 上，不会出现在 OpenAI 个人后台。
- 提示词：约 20 岁的成年东方女性，侧后方，银蓝色轻纱，红色油纸伞，雨。1536 写明横幅 3:2，3840 写明横幅 16:9。
- 参数：`n=1`，`output_format=jpeg`，`output_compression=90`。没有设置 `moderation=low`。
- 文字输入每次都是 355 token。没有图片输入。
- 单价按接口返回的 usage 计算：文字输入 $5 / 100 万 token，图片输出 $30 / 100 万 token。JPEG 压缩不改变 token。

Sunburst 和 Image 2 这次一起跑完后，这把 Key 的累计用量增加了 $1.7603，和两款模型成功图片的 token 价之和一致。Flare 的十张是同一提示词、同一把 Key 稍早生成的，token 数和后来的 Sunburst 逐档相同。

## 价格

花费 = `355 × 5 / 1,000,000 + 输出 token × 30 / 1,000,000`。

### 1536×1024

| 档位 | 2.5 输出 token | 2.5 花费 | Image 2 输出 token | Image 2 花费 |
| --- | ---: | ---: | ---: | ---: |
| low | 158 | $0.006515 | 158 | $0.006515 |
| medium | 343 | $0.012065 | 1372 | $0.042935 |
| high | 1372 | $0.042935 | 5488 | $0.166415 |
| xhigh | 2459 | $0.075545 | 不支持 | — |
| max | 5488 | $0.166415 | 不支持 | — |

Flare 和 Sunburst 都是左列。一款 2.5 模型跑满五档是 $0.303475。

### 3840×2160

| 档位 | 2.5 输出 token | 2.5 花费 | Image 2 输出 token | Image 2 花费 |
| --- | ---: | ---: | ---: | ---: |
| low | 371 | $0.012905 | 371 | $0.012905 |
| medium | 865 | $0.027725 | 3336 | $0.101855 |
| high | 3336 | $0.101855 | 13342 | $0.402035 |
| xhigh | 5930 | $0.179675 | 不支持 | — |
| max | 13342 | $0.402035 | 不支持 | — |

一款 2.5 模型跑满五档是 $0.724195。Image 2 两个分辨率只跑成 6 张，合计 $0.732660。

4K high 比 1536 max 便宜，宽度却是 3840。要像素时优先加分辨率。要同一张图里的发丝和伞面纤维时，再加 quality。

## 1536×1024

每张图下方是这次请求使用的 `model`。low 三款都能看。Flare 裁得更近，雨丝和肩上的水珠更显。Sunburst 把桥栏、河水和灯火放进来，人物稍远。Image 2 更像实拍，发饰和刺绣更简，背景虚化更干净。

### low

#### openai/gpt-image-2.5-flare

![openai/gpt-image-2.5-flare](flare-1536-low.jpg "model=openai/gpt-image-2.5-flare · size=1536x1024 · quality=low")

#### openai/gpt-image-2.5-sunburst

![openai/gpt-image-2.5-sunburst](sunburst-1536-low.jpg "model=openai/gpt-image-2.5-sunburst · size=1536x1024 · quality=low")

#### openai/gpt-image-2

![openai/gpt-image-2](image2-1536-low.jpg "model=openai/gpt-image-2 · size=1536x1024 · quality=low")

medium 开始拉开价格。2.5 仍是 $0.012，刺绣和雨更清楚。Image 2 的 medium 已经是 $0.043，和 2.5 的 high 同价，伞面、湿发和皮肤都更像照片。

### medium

#### openai/gpt-image-2.5-flare

![openai/gpt-image-2.5-flare](flare-1536-medium.jpg "model=openai/gpt-image-2.5-flare · size=1536x1024 · quality=medium")

#### openai/gpt-image-2.5-sunburst

![openai/gpt-image-2.5-sunburst](sunburst-1536-medium.jpg "model=openai/gpt-image-2.5-sunburst · size=1536x1024 · quality=medium")

#### openai/gpt-image-2

![openai/gpt-image-2](image2-1536-medium.jpg "model=openai/gpt-image-2 · size=1536x1024 · quality=medium")

high 是 2.5 的实用档，约 $0.043。Flare 继续特写，Sunburst 继续把亭台和倒影画完整。Image 2 的 high 是 $0.166，和 2.5 的 max 同价，人物更干净，背景常收成大光斑。

### high

#### openai/gpt-image-2.5-flare

![openai/gpt-image-2.5-flare](flare-1536-high.jpg "model=openai/gpt-image-2.5-flare · size=1536x1024 · quality=high")

#### openai/gpt-image-2.5-sunburst

![openai/gpt-image-2.5-sunburst](sunburst-1536-high.jpg "model=openai/gpt-image-2.5-sunburst · size=1536x1024 · quality=high")

#### openai/gpt-image-2

![openai/gpt-image-2](image2-1536-high.jpg "model=openai/gpt-image-2 · size=1536x1024 · quality=high")

xhigh 和 max 只有 2.5。细节加在发丝、流苏、雨点和伞面纸纹上。max 约 $0.166，大约是 high 的 4 倍。Image 2 请求这两档时，上游返回 `quality` 参数 `invalid_value`，原文是模型不支持 `xhigh` 或 `max`。

### xhigh

#### openai/gpt-image-2.5-flare

![openai/gpt-image-2.5-flare](flare-1536-xhigh.jpg "model=openai/gpt-image-2.5-flare · size=1536x1024 · quality=xhigh")

#### openai/gpt-image-2.5-sunburst

![openai/gpt-image-2.5-sunburst](sunburst-1536-xhigh.jpg "model=openai/gpt-image-2.5-sunburst · size=1536x1024 · quality=xhigh")

### max

#### openai/gpt-image-2.5-flare

![openai/gpt-image-2.5-flare](flare-1536-max.jpg "model=openai/gpt-image-2.5-flare · size=1536x1024 · quality=max")

#### openai/gpt-image-2.5-sunburst

![openai/gpt-image-2.5-sunburst](sunburst-1536-max.jpg "model=openai/gpt-image-2.5-sunburst · size=1536x1024 · quality=max")

## 3840×2160

4K 的内容仍然是同一类构图，不是换了一张更大的场景。同档 token 高于 1536，因为像素更多。JPEG 文件大约 0.5MB 到 1MB，压缩质量 90，文件大小不代表 token。

Image 2 的 4K medium 有一次被安全审核拦住，类别是 sexual，重试后成功。失败那次没有计入上面的成功价格。这次没有改审核参数。

### low

#### openai/gpt-image-2.5-flare

![openai/gpt-image-2.5-flare](flare-3840-low.jpg "model=openai/gpt-image-2.5-flare · size=3840x2160 · quality=low")

#### openai/gpt-image-2.5-sunburst

![openai/gpt-image-2.5-sunburst](sunburst-3840-low.jpg "model=openai/gpt-image-2.5-sunburst · size=3840x2160 · quality=low")

#### openai/gpt-image-2

![openai/gpt-image-2](image2-3840-low.jpg "model=openai/gpt-image-2 · size=3840x2160 · quality=low")

### medium

#### openai/gpt-image-2.5-flare

![openai/gpt-image-2.5-flare](flare-3840-medium.jpg "model=openai/gpt-image-2.5-flare · size=3840x2160 · quality=medium")

#### openai/gpt-image-2.5-sunburst

![openai/gpt-image-2.5-sunburst](sunburst-3840-medium.jpg "model=openai/gpt-image-2.5-sunburst · size=3840x2160 · quality=medium")

#### openai/gpt-image-2

![openai/gpt-image-2](image2-3840-medium.jpg "model=openai/gpt-image-2 · size=3840x2160 · quality=medium")

### high

#### openai/gpt-image-2.5-flare

![openai/gpt-image-2.5-flare](flare-3840-high.jpg "model=openai/gpt-image-2.5-flare · size=3840x2160 · quality=high")

#### openai/gpt-image-2.5-sunburst

![openai/gpt-image-2.5-sunburst](sunburst-3840-high.jpg "model=openai/gpt-image-2.5-sunburst · size=3840x2160 · quality=high")

#### openai/gpt-image-2

![openai/gpt-image-2](image2-3840-high.jpg "model=openai/gpt-image-2 · size=3840x2160 · quality=high")

### xhigh

#### openai/gpt-image-2.5-flare

![openai/gpt-image-2.5-flare](flare-3840-xhigh.jpg "model=openai/gpt-image-2.5-flare · size=3840x2160 · quality=xhigh")

#### openai/gpt-image-2.5-sunburst

![openai/gpt-image-2.5-sunburst](sunburst-3840-xhigh.jpg "model=openai/gpt-image-2.5-sunburst · size=3840x2160 · quality=xhigh")

### max

#### openai/gpt-image-2.5-flare

![openai/gpt-image-2.5-flare](flare-3840-max.jpg "model=openai/gpt-image-2.5-flare · size=3840x2160 · quality=max")

#### openai/gpt-image-2.5-sunburst

![openai/gpt-image-2.5-sunburst](sunburst-3840-max.jpg "model=openai/gpt-image-2.5-sunburst · size=3840x2160 · quality=max")

## 怎么选

- 草稿看构图：1536 low，大约 $0.007。三款同价。
- 日常成图：2.5 选 high，大约 $0.043。要同样的 token，Image 2 要写 medium。
- 想要雨巷、桥和灯火：看 Sunburst。想要更近的人像：看 Flare。两者同价。
- 需要 4K 文件：2.5 选 high，大约 $0.102。Image 2 要写 medium，价格相同。
- 只在发饰、雨丝、伞面需要再抠一层时用 2.5 的 max。1536 约 $0.166，4K 约 $0.402。Image 2 的 high 就是这一档的价格。
- 不要给 Image 2 传 `xhigh` 或 `max`。

调用时把模型名和档位写死。`quality=auto` 不会按你以为的档位计价。

```json
{
  "model": "openai/gpt-image-2.5-sunburst",
  "size": "1536x1024",
  "quality": "high",
  "n": 1,
  "output_format": "jpeg",
  "output_compression": 90
}
```

Key 的模型列表里要能看到对应 id。这次三款都是用 Lucoo 官方 Key 调通的。
