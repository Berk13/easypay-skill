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

Three CLI agents (**Claude Code / Codex / Gemini**) can install themselves — paste **one prompt**, the agent runs the commands. **Cursor** uses a JSON config paste because its MCP setup lives in Settings UI.

After install, the **first prompt** to send to your agent is universal:

```
Walk me through EasyPay onboarding so I can pick what fits my business and start accepting real payments.
```

For example prompts, alternative skill paths, and troubleshooting — see the [EasyPay MCP Customer Guide](https://docs.thenextgen.store/s/228aae3c-2e06-4b22-9982-8508a08d9d04).

### Claude Code

**1. The easy path** — paste this prompt into Claude and let it install everything:

```
Install the EasyPay MCP server and connect the skill for this project. To do this, run two commands — adapt them to your current environment if needed: claude mcp add easypay --transport sse https://mcp.appload.tech/sse --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>" ; mkdir -p .claude/skills/easypay && curl -fsSL https://raw.githubusercontent.com/EasyPay-Labs/easypay-skill/main/SKILL.md -o .claude/skills/easypay/SKILL.md
```

**2. Or install manually**:

```bash
claude mcp add easypay --transport sse https://mcp.appload.tech/sse --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>"
```

> If `claude mcp list` shows `easypay: ✗ Failed to connect`, run `claude mcp get easypay` — if it shows `Type: stdio` or `Command: \`, the shell mangled quoting. `claude mcp remove easypay` and rerun the exact command above (URL before `--header`, double quotes — works on bash and PS).

### Codex

Codex doesn't accept `--header` on `codex mcp add` — we bridge through the `mcp-remote` npm package (stdio ↔ SSE+headers). Requires Node.js + `npx`.

**1. The easy path** — paste this prompt into Codex:

```
Install the EasyPay MCP server and connect the skill for this project. To do this, run two commands — adapt them to your current environment if needed: codex mcp add easypay -- npx -y mcp-remote https://mcp.appload.tech/sse --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>" ; mkdir -p .agents/skills/easypay && curl -fsSL https://raw.githubusercontent.com/EasyPay-Labs/easypay-skill/main/SKILL.md -o .agents/skills/easypay/SKILL.md
```

**2. Or install manually**:

```bash
codex mcp add easypay -- npx -y mcp-remote https://mcp.appload.tech/sse --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>"
```

### Gemini CLI

**1. The easy path** — paste this prompt into Gemini:

```
Install the EasyPay MCP server and connect the skill for this project. To do this, run two commands — adapt them to your current environment if needed: gemini mcp add easypay --transport sse --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>" https://mcp.appload.tech/sse ; mkdir -p .agents/skills/easypay && curl -fsSL https://raw.githubusercontent.com/EasyPay-Labs/easypay-skill/main/SKILL.md -o .agents/skills/easypay/SKILL.md
```

**2. Or install manually**:

```bash
gemini mcp add easypay --transport sse --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>" https://mcp.appload.tech/sse
```

> If your Gemini version doesn't accept `--header` (rare — verified working 2026-05-10), use the `mcp-remote` bridge as in Codex.

### Cursor

Cursor's MCP install is a JSON config pasted into Settings → **MCP** → **Add new MCP server**:

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

The skill for Cursor goes into your project folder — see the [Customer Guide](https://docs.thenextgen.store/s/228aae3c-2e06-4b22-9982-8508a08d9d04) for the exact path.

---

## After install — full guide

Example prompts (Stripe products, crypto/bank invoices, payouts, balance), alternative skill paths (user-home vs project-local), per-CLI nuances, and troubleshooting all live in the [EasyPay MCP Customer Guide](https://docs.thenextgen.store/s/228aae3c-2e06-4b22-9982-8508a08d9d04) (RU + EN).

For products and invoices that need EasyPay moderation, you'll get a notification in your DM via `@easypay_onboarding_bot` once the link or invoice is live.

---

## What's inside

- [`SKILL.md`](./SKILL.md) — the system prompt: 17 tools, EasyPay domain language, JTBD flows, anti-patterns.
- [`LICENSE`](./LICENSE) — MIT.

---

## Status

**v0.4.0** — agentic-primary install (paste one prompt, agent runs it) + manual fallback per CLI. Skill downloaded into project (`.claude/skills/easypay/` for Claude Code, `.agents/skills/easypay/` for Codex & Gemini, project-local for Cursor — see Customer Guide). README is the install quickstart; [Customer Guide](https://docs.thenextgen.store/s/228aae3c-2e06-4b22-9982-8508a08d9d04) is the source of truth for examples, alternative paths, and troubleshooting.

Not yet:
- CI markdown linter
- npm package
- Localizations beyond RU/EN

## Support

- Bot: [@easypay_onboarding_bot](https://t.me/easypay_onboarding_bot) — get an API key, ask for help with install.
- Direct: [@andre_erokhin](https://t.me/andre_erokhin)

## License

[MIT](./LICENSE) — use freely, including commercial. Attribution appreciated.
