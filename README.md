# EasyPay AI Skill

Connect your AI agent to **EasyPay** in one command. Speak natural language:

> «Создай продукт за 5000₽ через СБП»
> «Сколько у меня сейчас денег в долларах и крипте?»
> «Выставь инвойс на $1200 john@example.com»
> «Закажи выплату подрядчику в РФ на 200 000 ₽»

The skill teaches your agent the EasyPay vocabulary (Stripe / Mercury / Crypto / T-Bank, USD / EUR / RUB / CRYPTO, product lifecycle, payout flow) and which of the 17 MCP tools to call for each job.

---

## What you need

1. **An EasyPay partner account** with an API key.
   Don't have one? Open [@easypay_onboarding_bot](https://t.me/easypay_onboarding_bot) in Telegram → choose the **MCP** path.
2. **One of these CLI agents**: Claude Code, Cursor, Codex CLI, or Gemini CLI.

---

## Install in 30 seconds

> **Note (v0.1.0)**: header-based auth (`X-Partner-Api-Key`) is being rolled out across the MCP server. The snippets below use the new header form. If you get `Invalid API key`, your partner account is still on the old body-auth — message [@andre_erokhin](https://t.me/andre_erokhin) and we'll migrate you in <24h.

### Claude Code

```bash
claude mcp add easypay \
  --transport sse \
  --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>" \
  https://n8n.thenextgen.store/mcp/easypay-payments-mcp/sse

claude skill add https://github.com/Berk13/easypay-skill
```

### Cursor

Open Cursor settings → **MCP** → **Add new MCP server** → paste:

```json
{
  "mcpServers": {
    "easypay": {
      "url": "https://n8n.thenextgen.store/mcp/easypay-payments-mcp/sse",
      "transport": "sse",
      "headers": {
        "X-Partner-Api-Key": "<YOUR_PARTNER_API_KEY>"
      }
    }
  }
}
```

Then drop [`SKILL.md`](./SKILL.md) into your `.cursor/rules/` folder (or paste its content into Cursor's "Rules for AI" settings).

### Codex CLI

```bash
codex mcp add easypay \
  --url https://n8n.thenextgen.store/mcp/easypay-payments-mcp/sse \
  --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>"

# Skill: paste SKILL.md content into ~/.codex/instructions.md
curl -s https://raw.githubusercontent.com/Berk13/easypay-skill/main/SKILL.md \
  >> ~/.codex/instructions.md
```

### Gemini CLI

```bash
gemini mcp add easypay \
  --transport sse \
  --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>" \
  https://n8n.thenextgen.store/mcp/easypay-payments-mcp/sse

# Skill: append SKILL.md to your Gemini system instructions
curl -s https://raw.githubusercontent.com/Berk13/easypay-skill/main/SKILL.md \
  >> ~/.gemini/system-instructions.md
```

---

## Try it

Once installed, ask your agent:

```
Покажи мой профиль EasyPay и какие методы оплаты доступны
```

The agent should call `verify_partner_api_key_and_get_profile` (no key in the prompt — it comes from your config) and reply with your partner name and available methods.

More example prompts will live in `examples.md` in the next release.

---

## What's inside

- [`SKILL.md`](./SKILL.md) — the system prompt: 17 tools, EasyPay domain language, JTBD flows, anti-patterns.
- [`LICENSE`](./LICENSE) — MIT.

---

## Status

**v0.1.0** — first public release.

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
