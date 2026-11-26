# openclaw-xingchen-agi-provider

OpenClaw provider plugin for the **Xing4.1-Preview** model from
**China Telecom AI Technology Co., Ltd.**, distributed on ClawHub as
`@xingchen-agi/xingchen-agi-provider`.

This repository is the public source for the community (external) plugin. It is
a sibling of, and independent from, the bundled OpenClaw provider entry in the
OpenClaw core repository.

## Install

```bash
openclaw plugins install @xingchen-agi/xingchen-agi-provider
```

Installation applies to a running Gateway automatically; otherwise it takes
effect on the next startup.

## Setup

Create an API key in the China Telecom AI MaaS dashboard, then store it through
normal model auth:

```bash
openclaw onboard --auth-choice xingchen-agi-api-key
```

For scripted or CI installs, pass everything on the command line:

```bash
openclaw onboard --non-interactive --accept-risk --skip-health \
  --auth-choice xingchen-agi-api-key \
  --xingchen-api-key "$XINGCHEN_API_KEY"
```

Or set the key in the environment instead:

```sh
export XINGCHEN_API_KEY="<your-xingchen-api-key>" # pragma: allowlist secret
```

## Model

| Model ref                      | Input | Context | Max output |
| ------------------------------ | ----- | ------- | ---------- |
| `xingchen-agi/xing4.1-preview` | text  | 1,024,000 | 128,000    |

Provider id: `xingchen-agi`
Base URL: `https://api-maas.teleai.com.cn/v1`
API: OpenAI-compatible (`openai-completions`)

## Pricing

USD per 1M tokens. Public price card: https://xingchen-agi.github.io/pricing/

| Xing4.1-Preview | List price | Limited-time offer (20% of list, through 2026-12-20) |
| --- | --- | --- |
| Input (cache miss) | $0.225 | $0.045 |
| Input (cache hit) | $0.01125 | $0.00225 |
| Output | $0.90 | $0.18 |

The plugin's single-value `cost` fields carry the offer price currently in effect.
They will be switched to the list prices when the offer ends on 2026-12-20.
Cache-hit price is 5% of the corresponding input price; no peak/off-peak tiers apply.

## Package layout

- `index.ts` - TypeScript source for the plugin entry
- `dist/index.js` - compiled runtime entry (the file OpenClaw loads)
- `openclaw.plugin.json` - plugin manifest
- `assets/icon.png` - ClawHub catalog icon

## Build

```sh
# install a local build tool into the managed runtime project (or use esbuild)
npm install -D esbuild
npm run build   # transpiles index.ts -> dist/index.js (SDK imports stay external)
```

## Publish

```sh
clawhub login
clawhub package validate .
clawhub package publish . --dry-run
clawhub package publish .
```

## License

MIT-0