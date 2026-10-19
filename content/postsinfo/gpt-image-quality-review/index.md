---
title: "GPT Image 三款模型画质档位实测"
date: 2026-10-19T11:00:00+08:00
lastmod: 2026-10-19T11:31:00+08:00
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

日常成图用 2.5 的 high。想要雨巷、桥和灯火，选 `openai/gpt-image-2.5-sunburst`。想要更近的人像，选 `openai/gpt-image-2.5-flare`。需要 4K 文件时，仍然用 high，把 `size` 写成 `3840x2160`。

Image 2 只有 low、medium、high。`xhigh` 和 `max` 会返回 400。它的档位名字和 2.5 对不齐：

| Image 2 写下的档位 | 画面接近 2.5 的 |
| --- | --- |
| low | low |
| medium | high |
| high | max |
| xhigh、max | 不支持，2.5 才有 |

所以用 Image 2 时，日常写 medium，要最细的发饰和伞面再写 high。

画面倾向上，Flare 更靠近人物，Sunburst 更常把桥、河和灯火画进画面，Image 2 更接近实拍，背景经常收成光斑。每格只出了一张，构图里有随机采样，不能当成模型的固定风格。

## 测试条件

- Key：Lucoo 官方 Key，请求发到 `https://lucoo.ai/v1`。
- 提示词：约 20 岁的成年东方女性，侧后方，银蓝色轻纱，红色油纸伞，雨。1536 写明横幅 3:2，3840 写明横幅 16:9。
- 参数：`n=1`，`output_format=jpeg`，`output_compression=90`。没有设置 `moderation=low`。
- 三款都用同一提示词。Flare 稍早生成，Sunburst 和 Image 2 是同一次补齐的。

## 1536×1024

每张图下方是这次请求使用的 `model`。low 三款都能看。Flare 裁得更近，雨丝和肩上的水珠更显。Sunburst 把桥栏、河水和灯火放进来，人物稍远。Image 2 更像实拍，发饰和刺绣更简，背景虚化更干净。

### low

#### openai/gpt-image-2.5-flare

![openai/gpt-image-2.5-flare](flare-1536-low.jpg "model=openai/gpt-image-2.5-flare · size=1536x1024 · quality=low")

#### openai/gpt-image-2.5-sunburst

![openai/gpt-image-2.5-sunburst](sunburst-1536-low.jpg "model=openai/gpt-image-2.5-sunburst · size=1536x1024 · quality=low")

#### openai/gpt-image-2

![openai/gpt-image-2](image2-1536-low.jpg "model=openai/gpt-image-2 · size=1536x1024 · quality=low")

medium 上，2.5 的刺绣和雨更清楚。Image 2 的 medium 已经接近 2.5 的 high，伞面、湿发和皮肤都更像照片。

### medium

#### openai/gpt-image-2.5-flare

![openai/gpt-image-2.5-flare](flare-1536-medium.jpg "model=openai/gpt-image-2.5-flare · size=1536x1024 · quality=medium")

#### openai/gpt-image-2.5-sunburst

![openai/gpt-image-2.5-sunburst](sunburst-1536-medium.jpg "model=openai/gpt-image-2.5-sunburst · size=1536x1024 · quality=medium")

#### openai/gpt-image-2

![openai/gpt-image-2](image2-1536-medium.jpg "model=openai/gpt-image-2 · size=1536x1024 · quality=medium")

high 是 2.5 的实用档。Flare 继续特写，Sunburst 继续把亭台和倒影画完整。Image 2 的 high 接近 2.5 的 max，人物更干净，背景常收成大光斑。

### high

#### openai/gpt-image-2.5-flare

![openai/gpt-image-2.5-flare](flare-1536-high.jpg "model=openai/gpt-image-2.5-flare · size=1536x1024 · quality=high")

#### openai/gpt-image-2.5-sunburst

![openai/gpt-image-2.5-sunburst](sunburst-1536-high.jpg "model=openai/gpt-image-2.5-sunburst · size=1536x1024 · quality=high")

#### openai/gpt-image-2

![openai/gpt-image-2](image2-1536-high.jpg "model=openai/gpt-image-2 · size=1536x1024 · quality=high")

xhigh 和 max 只有 2.5。细节加在发丝、流苏、雨点和伞面纸纹上，构图不会换成另一种画。Image 2 请求这两档时，上游返回 `quality` 参数 `invalid_value`，原文是模型不支持 `xhigh` 或 `max`。

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

4K 的内容仍然是同一类构图，不是换了一张更大的场景。要大图就改 `size`，档位仍按上面的建议选。JPEG 压缩质量 90，文件大小不代表画质档位。

Image 2 的 4K medium 有一次被安全审核拦住，类别是 sexual，重试后成功。这次没有改审核参数。

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

- 草稿看构图：1536，`quality=low`。三款都可以。
- 日常成图：2.5 写 `quality=high`。Image 2 写 `quality=medium`。
- 想要雨巷、桥和灯火：`openai/gpt-image-2.5-sunburst`。想要更近的人像：`openai/gpt-image-2.5-flare`。
- 需要 4K 文件：`size=3840x2160`。2.5 仍写 high，Image 2 仍写 medium。
- 只在发饰、雨丝、伞面需要再抠一层时，2.5 才用 `quality=max`。Image 2 写 `quality=high`。
- 不要给 Image 2 传 `xhigh` 或 `max`。

调用时把模型名和档位写死。`quality=auto` 不会按你以为的档位出图。

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
