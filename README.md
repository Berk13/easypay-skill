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

The API key goes into your MCP client config as an `X-Partner-Api-Key` HTTP header — **not into the chat**. The model never sees it; the FastMCP server at `mcp.appload.tech` validates and forwards it on every tool call.

### Claude Code

```bash
claude mcp add easypay \
  --transport sse \
  --header "X-Partner-Api-Key: <YOUR_PARTNER_API_KEY>" \
  https://mcp.appload.tech/sse

claude skill add https://github.com/Berk13/easypay-skill
```

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

### Codex CLI

```bash
codex mcp add easypay \
  --url https://mcp.appload.tech/sse \
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
  https://mcp.appload.tech/sse

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

## Backward compatibility (existing partners on body-auth)

Partners who configured EasyPay MCP before v0.2 use the legacy n8n MCP host (`https://n8n.thenextgen.store/mcp/easypay-payments-mcp/sse`) with the API key passed as a tool argument. That path still works — the same backend webhooks now accept either an `X-Partner-Api-Key` header (FastMCP path, key invisible to the model) or a body `partner_api_key` field (legacy path, key visible in tool args).

If you are on legacy: keep working, no migration needed for the regatta. To switch to header-based, replace your MCP client config with the snippet above and remove any prompt that pastes the key into chat.

## Emergency fallback

If `https://mcp.appload.tech/sse` is unreachable (rare — single Hetzner VPS + Cloudflare Tunnel), use the legacy host as a temporary fallback:

```bash
# Claude Code — emergency body-auth path
claude mcp add easypay-legacy \
  --transport sse \
  https://n8n.thenextgen.store/mcp/easypay-payments-mcp/sse
```

Then in your first message, paste:

```
Use the easypay-legacy MCP server. My EasyPay API key is: <YOUR_PARTNER_API_KEY>

<your real request>
```

The agent will pass the key as a tool argument on each call (not as elegant, but resilient — the n8n MCP server runs on a different host than `mcp.appload.tech`).

Notify [@andre_erokhin](https://t.me/andre_erokhin) so we can investigate the outage.

---

## What's inside

- [`SKILL.md`](./SKILL.md) — the system prompt: 17 tools, EasyPay domain language, JTBD flows, anti-patterns.
- [`LICENSE`](./LICENSE) — MIT.

---

## Status

**v0.2.0** — header-based authentication, public FastMCP server at `mcp.appload.tech`.

Changes since v0.1.0:
- Primary MCP host moved from `n8n.thenextgen.store/mcp/easypay-payments-mcp/sse` to `mcp.appload.tech/sse` (FastMCP wrapper, header-only auth).
- API key now lives in MCP client config as `X-Partner-Api-Key` header — invisible to the model, never in tool arguments.
- Legacy body-auth path remains live for backward compatibility (`Backward compatibility` section above).
- Added emergency fallback section in case the FastMCP host is down on regatta day.

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
