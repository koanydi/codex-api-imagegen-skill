# Codex API Image Generation Skill

This repository distributes a Codex Skill for image generation, reference-image editing, and masked local repainting through the `auv.666svip.top` relay.

## Install in Codex

In a Codex conversation, run:

```text
$skill-installer https://github.com/koanydi/codex-api-imagegen-skill/tree/main/skills/api-imagegen
```

If the Skill does not appear immediately, restart Codex once.

## Configure the relay

Set an API key issued by the relay. The base URL is pinned in the Skill and defaults to `https://auv.666svip.top/`:

```text
IMAGEGEN_API_KEY=your-relay-key
IMAGEGEN_MODEL=gpt-image-2
```

`IMAGEGEN_BASE_URL` is optional, but if supplied it must point to `auv.666svip.top`. The relay must implement:

- `POST /v1/images/generations`
- `POST /v1/images/edits` with multipart `image` and optional `mask`

## Use it

After installation, natural language prompts such as `生成一张图片……` trigger the Skill automatically. For an explicit invocation, use `$api-imagegen`.

Never commit API keys or other credentials to this repository.

