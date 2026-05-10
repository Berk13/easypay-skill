# EasyPay AI Skill

Connect your AI agent to **EasyPay** in one command. Speak natural language:

> «Создай Stripe-продукт «Продвинутый AI-курс» за $299 в test-режиме»
> «Выставь крипто-инвойс на $20 за бандл виртуальной валюты»
> «Выставь Bank Invoice клиенту john@example.com на $250 за LinkedIn-консультацию»
> «Какие функции EasyPay у меня подключены, а какие ещё нет?»

The skill teaches your agent the EasyPay vocabulary (Stripe / Mercury / Crypto / T-Bank, USD / EUR / RUB / CRYPTO, product lifecycle, payout flow) and which of the 17 MCP tools to call for each job.

---

## What you need

1. **An EasyPay partner account** with an API key.
   Don't have one? Open [@easypay_onboarding_bot](https://t.me/easypay_onboarding_bot) in Telegram → choose the **MCP** path.
2. **One of these CLI agents**: Claude Code, Cursor, Codex, or Gemini CLI.

---

## Install in 30 seconds

The API key goes into your MCP client config as an `X-Partner-Api-Key` HTTP header — **not into the chat**. The model never sees it; the FastMCP server at `mcp.appload.tech` validates and forwards it on every tool call.

### Claude Code

**macOS / Linux (bash / zsh):**

```bash
claude mcp add easypay \
  --transport sse \
  --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>" \
  https://mcp.appload.tech/sse

claude skill add https://github.com/EasyPay-Labs/easypay-skill
```

**Windows (PowerShell):**

PowerShell quoting eats the URL if `--header` comes before it. Use **URL before `--header`**, and **single quotes** around the header value:

```powershell
claude mcp add easypay --scope user --transport sse https://mcp.appload.tech/sse --header 'X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>'

claude skill add https://github.com/EasyPay-Labs/easypay-skill
```

If `claude mcp list` shows `easypay: ✗ Failed to connect`, run `claude mcp get easypay` — if `Type: stdio` (instead of `sse`) or `Command: \`, the PS quoting broke the install. Run `claude mcp remove easypay` and re-add with the exact PowerShell command above. Do NOT use `--header="..."` (the `=` confuses PS), do NOT put `--header` before the URL on Windows.

### Cursor

Open Cursor settings → **MCP** → **Add new MCP server** → paste:

```json
{
  "mcpServers": {
    "easypay": {
      "url": "https://mcp.appload.tech/sse",
      "transport": "sse",
      "headers": {
        "X-Partner-Api-Key": "<YOUR_PARTNER_API_KEY>"
      }
    }
  }
}
```

Then drop [`SKILL.md`](./SKILL.md) into your `.cursor/rules/` folder (or paste its content into Cursor's "Rules for AI" settings).

### Codex

Codex (OpenAI's standalone desktop app) `codex mcp add` does **not** support
custom headers directly — bridge through the `mcp-remote` npm package
(stdio ↔ SSE+headers). Single-line, works on Windows / macOS / Linux without
backslash continuation. Requires Node.js + `npx` (most devs already have).

```bash
codex mcp add easypay -- npx -y mcp-remote https://mcp.appload.tech/sse --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>"
```

Skill — paste SKILL.md content into `~/.codex/instructions.md`:

```bash
curl -s https://raw.githubusercontent.com/EasyPay-Labs/easypay-skill/main/SKILL.md \
  >> ~/.codex/instructions.md
```

### Gemini CLI

```bash
gemini mcp add easypay \
  --transport sse \
  --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>" \
  https://mcp.appload.tech/sse

# Skill: append SKILL.md to your Gemini system instructions
curl -s https://raw.githubusercontent.com/EasyPay-Labs/easypay-skill/main/SKILL.md \
  >> ~/.gemini/system-instructions.md
```

> **If Gemini errors on `--header`** (older versions don't support it), use
> the same mcp-remote bridge as Codex above:
>
> ```bash
> gemini mcp add easypay -- npx -y mcp-remote https://mcp.appload.tech/sse --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>"
> ```

---

## Try it

Once installed, paste any of these into your agent:

```
Создай Stripe-продукт «Продвинутый AI-курс» за $299 в test-режиме
```

```
Выставь крипто-инвойс на $20 за бандл виртуальной валюты
```

```
Выставь Bank Invoice клиенту john@example.com на $250 за LinkedIn-консультацию
```

```
Какие функции EasyPay у меня подключены, а какие ещё нет?
```

The agent calls the right MCP tool (no key in the prompt — it comes from your config) and returns the result. For the first two test products / invoices, the EasyPay care team reviews and approves; you'll get a notification when the link or invoice is live.

---

## What's inside

- [`SKILL.md`](./SKILL.md) — the system prompt: 17 tools, EasyPay domain language, JTBD flows, anti-patterns.
- [`LICENSE`](./LICENSE) — MIT.

---

## Status

**v0.3.0** — primary install path is FastMCP at `mcp.appload.tech` with header-based auth. Single source of truth for setup is this README.

Not yet:
- `examples.md` with copy-paste prompts (next release)
- CI markdown linter
- npm package
- Localizations beyond RU/EN

## Support

- Bot: [@easypay_onboarding_bot](https://t.me/easypay_onboarding_bot) — get an API key, ask for help with install.
- Direct: [@andre_erokhin](https://t.me/andre_erokhin)

## License

[MIT](./LICENSE) — use freely, including commercial. Attribution appreciated.
