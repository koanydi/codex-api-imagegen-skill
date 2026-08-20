# Codex API Image Generation Skill

一个面向 Codex 的图片生成与编辑 Skill。它通过用户自己配置的 OpenAI 兼容图片接口工作，当前发行版固定使用 `https://auv.666svip.top/` 中转站。

This repository distributes a Codex Skill for image generation and editing through the user's own OpenAI-compatible Images API. This release is intentionally pinned to the `https://auv.666svip.top/` relay.

## 功能 Features

- 文字生成新图：`POST /v1/images/generations`
- 上传一张或多张参考图进行编辑：`POST /v1/images/edits`
- 使用 PNG 蒙版进行局部重绘
- 自动选择生成或编辑模式，并保存结果到本机
- 支持 PNG、JPEG、WEBP、GIF 输入；最多 7 张输入图，最多 4 个结果

- Text-to-image generation via `POST /v1/images/generations`
- Reference-image editing via `POST /v1/images/edits`
- Masked local repainting with a PNG mask
- Automatic generate/edit mode selection and local result saving
- PNG, JPEG, WEBP, and GIF inputs; up to 7 input images and 4 outputs

## 安装 Install

在 Codex 对话中运行：

In a Codex conversation, run:

```text
$skill-installer https://github.com/koanydi/codex-api-imagegen-skill/tree/main/skills/api-imagegen
```

如果 Skill 没有立即出现，重启一次 Codex。

Restart Codex once if the Skill does not appear immediately.

## 配置 Configure

请向 AUV 中转站申请属于你自己的 API Key。不要把 Key 提交到 GitHub，也不要把 Key 发到聊天中。

Obtain your own API key from the AUV relay. Never commit the key to GitHub or send it in chat.

Windows PowerShell:

```powershell
[Environment]::SetEnvironmentVariable("IMAGEGEN_BASE_URL", "https://auv.666svip.top", "User")
[Environment]::SetEnvironmentVariable("IMAGEGEN_API_KEY", "<your-relay-key>", "User")
[Environment]::SetEnvironmentVariable("IMAGEGEN_MODEL", "gpt-image-2", "User")
```

Linux/macOS:

```bash
export IMAGEGEN_BASE_URL="https://auv.666svip.top"
export IMAGEGEN_API_KEY="<your-relay-key>"
export IMAGEGEN_MODEL="gpt-image-2"
```

`IMAGEGEN_BASE_URL` 可省略，Skill 默认使用 AUV；如果填写，脚本会拒绝其他主机。

`IMAGEGEN_BASE_URL` is optional because the Skill defaults to AUV. If supplied, the script rejects other hosts.

检查配置但不发起生图：

Check configuration without generating an image:

```text
python <skill-directory>/scripts/generate.py --check-config
```

## 使用 Usage

直接说“生成一张图片……”通常会自动触发 Skill，也可以显式使用：

Natural language prompts usually trigger the Skill automatically. Explicit invocation:

```text
使用 $api-imagegen 生成一张 16:9 的未来城市海报：夜景、雨后街道、霓虹灯、电影感，不要文字。
```

编辑原图：

Edit an original image:

```text
使用 $api-imagegen 修改我上传的图片：只把天空改成日落，保持主体、构图和其他区域不变。
```

蒙版局部重绘：

Masked repainting:

```text
使用 $api-imagegen 修改这张图：只重绘透明蒙版区域，把那里改成一扇木门，其他区域保持不变。
```

底层命令示例：

Underlying command:

```powershell
python "<skill-directory>\\scripts\\generate.py" `
  --prompt "只重绘蒙版区域，把那里改成一扇木门，其他区域保持不变。" `
  --image "D:\\images\\original.png" `
  --mask "D:\\images\\mask.png" `
  --size 1024x1024 `
  --n 1
```

## 接口要求 API requirements

中转站需要实现 OpenAI 兼容的以下接口：

The relay must implement these OpenAI-compatible endpoints:

- `POST /v1/images/generations` for text-to-image
- `POST /v1/images/edits` for multipart image editing with `image` and optional `mask`
- JSON responses with `data[].url` or `data[].b64_json`

本 Skill 只允许连接 `auv.666svip.top`，不会自动使用其他上游。编辑参数是否被模型完全支持，以中转站返回结果为准。

This Skill only allows the `auv.666svip.top` host and does not silently use another upstream. Exact edit-parameter support remains provider-dependent.

## 安全 Security

- API Key 只存在于用户本机环境变量。
- 仓库不包含任何密钥、个人图片或生成结果。
- 输入图片只会发送到用户配置的 AUV 中转站。

- Keep API keys in local environment variables only.
- This repository contains no keys, personal images, or generated results.
- Input images are sent only to the user's configured AUV relay.

## License

Use this Skill in accordance with the terms of your relay provider and Codex environment.
