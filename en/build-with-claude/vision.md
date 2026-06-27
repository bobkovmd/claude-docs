# Vision — Claude Platform Docs

## Overview

Claude's vision capabilities enable understanding and analysis of images across multiple access points: **claude.ai** (web UI), **Anthropic Workbench**, and the **API**. Images are sent as `image` content blocks using one of three source types:

1. **Base64-encoded** image in the request body
2. **URL reference** to an online-hosted image
3. **`file_id`** from the Files API (upload once, reference many times)

> **Note:** On Amazon Bedrock and Google Cloud, only base64-encoded sources are currently available.

> **Tip:** Place images **before** text in your message content for best results. Image-then-text structure outperforms text-then-image or interleaved approaches.

## Image Limits & Costs

| Environment | Max images per request |
|---|---|
| claude.ai | 20 per message |
| API (200k context models) | 100 per request |
| API (all other models) | 600 per request |

- **Max dimensions:** 8000×8000 px per image
- **Max size per image:** 10 MB (base64)
- **Request size limit:** 32 MB standard

## Supported Formats

**JPEG, PNG, GIF, WebP** — Animations unsupported, only first frame used.

## Resolution & Token Cost

Claude processes images in **28×28 px patches** (visual tokens). Cost = `⌈width/28⌉ × ⌈height/28⌉` visual tokens.

| Resolution Tier | Models | Max Long Edge | Max Visual Tokens |
|---|---|---|---|
| **High-res** | Claude Fable 5, Mythos 5, Opus 4.8, Opus 4.7 | 2576 px | 4784 |
| **Standard** | All other models | 1568 px | 1568 |

## Limitations

- ❌ **Cannot name people** in images (per Anthropic's Acceptable Use Policy)
- ⚠️ May **hallucinate or err** on low-quality, rotated, or very small (<200px) images
- ⚠️ **Coordinate/localization outputs are approximate**
- ⚠️ **Counting is approximate**
- ❌ **Cannot determine** if an image is AI-generated
- ❌ Does not process **inappropriate/explicit** images
- ❌ Not designed for **diagnostic medical scans** (CT/MRI)

---

*Source: https://platform.claude.com/docs/en/build-with-claude/vision*
