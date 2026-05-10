# EasyPay AI Skill

Connect your AI agent to **EasyPay** in one command. Speak natural language:

> «Создай новый продукт в Stripe «Продвинутый AI-курс» за $299 в месяц»
> «Создай для customer@example.ru ссылку на 25 000₽ через СБП»
> «Сколько у меня сейчас денег в долларах и крипте?»
> «Выставь банковский инвойс на $1200 john@example.com»
> «Организуй выплату подрядчику в РФ на 200 000 ₽ с наших долларов»

The skill teaches your agent the EasyPay vocabulary (Stripe / Mercury / Crypto / T-Bank, USD / EUR / RUB / CRYPTO, product lifecycle, payout flow) and which of the 17 MCP tools to call for each job.

---

## What you need

1. **An EasyPay partner account** with an API key.
   Don't have one? Open [@easypay_onboarding_bot](https://t.me/easypay_onboarding_bot) in Telegram → choose the **MCP** path.
2. **One of these CLI agents**: Claude Code, Cursor, Codex, or Gemini CLI.

---

## Install in 30 seconds

The API key goes into your MCP client config as an `X-Partner-Api-Key` HTTP header — **not into the chat**. The model never sees it; the FastMCP server at `mcp.appload.tech` validates and forwards it on every tool call.

Each CLI gets the same three steps: **(1)** connect the MCP server, **(2)** verify the agent sees the tools, **(3)** install the skill so the agent speaks EasyPay's vocabulary. All commands are single-line and work on Windows, macOS, and Linux unless noted.

### Claude Code

**1. Connect the MCP server** (one command, works in bash/zsh AND PowerShell):

```bash
claude mcp add easypay --transport sse https://mcp.appload.tech/sse --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>"
```

**2. Verify** — ask Claude:

```
Use the EasyPay MCP server and explain what tools are available.
```

**3. Recommended for permanent use** — install the skill (teaches Claude EasyPay vocabulary and flows):

```bash
claude skill add https://github.com/EasyPay-Labs/easypay-skill
```

> **Diagnostic:** if `claude mcp list` shows `easypay: ✗ Failed to connect`, run `claude mcp get easypay` — if it shows `Type: stdio` or `Command: \`, the shell mangled quoting. `claude mcp remove easypay` and rerun the exact command above (URL before `--header`, double quotes — works on both bash and PS).

### Cursor

**1. Connect the MCP server** — Settings → **MCP** → **Add new MCP server** → paste:

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

**2. Verify** — ask Cursor:

```
Use the EasyPay MCP server and explain what tools are available.
```

**3. Recommended for permanent use** — drop the skill into your project:

```bash
mkdir -p .cursor/skills/easypay && curl -fsSL https://raw.githubusercontent.com/EasyPay-Labs/easypay-skill/main/SKILL.md -o .cursor/skills/easypay/SKILL.md
```

### Codex

Codex (OpenAI's standalone desktop app) doesn't accept custom headers on `codex mcp add` — we bridge through the `mcp-remote` npm package (stdio ↔ SSE+headers). Requires Node.js + `npx` (most devs have it).

**1. Connect the MCP server**:

```bash
codex mcp add easypay -- npx -y mcp-remote https://mcp.appload.tech/sse --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>"
```

**2. Verify** — ask Codex:

```
Use the EasyPay MCP server and explain what tools are available.
```

**3. Recommended for permanent use** — install the skill into your global agents folder:

```bash
mkdir -p ~/.agents/skills/easypay && curl -fsSL https://raw.githubusercontent.com/EasyPay-Labs/easypay-skill/main/SKILL.md -o ~/.agents/skills/easypay/SKILL.md
```

### Gemini CLI

**1. Connect the MCP server**:

```bash
gemini mcp add easypay --transport sse --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>" https://mcp.appload.tech/sse
```

**2. Verify** — ask Gemini:

```
Use the EasyPay MCP server and explain what tools are available.
```

**3. Recommended for permanent use** — install the skill:

```bash
mkdir -p ~/.gemini/easypay && curl -fsSL https://raw.githubusercontent.com/EasyPay-Labs/easypay-skill/main/SKILL.md -o ~/.gemini/easypay/SKILL.md
```

> **If your Gemini version doesn't accept `--header`** (rare — verified working 2026-05-10), use the `mcp-remote` bridge as in Codex:
>
> ```bash
> gemini mcp add easypay -- npx -y mcp-remote https://mcp.appload.tech/sse --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>"
> ```

---

## Try it

Once installed, paste any of these into your agent:

```
Создай новый продукт в Stripe «Продвинутый AI-курс» за $299 в месяц
```

```
Создай для customer@example.ru ссылку на 25 000₽ через СБП
```

```
Сколько у меня сейчас денег в долларах и крипте?
```

```
Выставь банковский инвойс на $1200 john@example.com
```

```
Организуй выплату подрядчику в РФ на 200 000 ₽ с наших долларов
```

The agent calls the right MCP tool (no key in the prompt — it comes from your config) and returns the result. For products and invoices that need EasyPay moderation, you'll get a notification in your DM via `@easypay_onboarding_bot` once the link or invoice is live.

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
