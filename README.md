# VRAMGlass MCP Server

Live GPU prices, VRAM requirements and local LLM hardware planning for your AI assistant, from [VRAMGlass](https://vramglass.com/mcp).

Connect it to Grok Bot, Claude, ChatGPT, Claude Code, Codex, Cursor or any MCP client, then ask in plain language. Answers come from the same data as vramglass.com, with links back to the source.

```
https://mcp.vramglass.com/mcp
```

Remote server, Streamable HTTP, read-only. Free; a VRAMGlass account is required.

[Developer docs](https://vramglass.com/docs#mcp) · [简体中文文档](https://vramglass.com/zh/docs#mcp) · [日本語ドキュメント](https://vramglass.com/ja/docs#mcp) · [MCP page](https://vramglass.com/mcp) · [Methodology](https://vramglass.com/methodology)

## What you can ask

- "What does an RTX 3090 cost on eBay right now?"
- "How much VRAM does Qwen3.8 27B need at 4-bit with a 32K context?"
- "Which models can a DGX Spark run, and how fast?"
- "Build me a PC for DeepSeek V4 Flash under $5,000."
- "Are used GPU prices going up or down this month?"
- "How much is renting an A100 per hour?"

For example, asked for RTX 3090 prices on October 22, 2026, the server returned a median eBay (US) asking price of $1,799 across 110 listings, ¥8,600 on Xianyu and ¥416,420 on Amazon.co.jp, each with its publish time and a link to the VRAMGlass page.

## Connect

### With an API key (recommended)

1. **Sign up or log in** at [vramglass.com/login](https://vramglass.com/login). It's free; an email address is enough.
2. **Create a key** under [Your account → MCP connections](https://vramglass.com/account/mcp). Give it a name, choose how long it stays valid and copy it right away; it is shown only once.
3. **Send this to your AI**, with `YOUR_KEY` replaced by your key. Grok Bot, Claude Code, Codex, Manus, WorkBuddy and other assistants that can change their own settings add the server themselves:

   ```
   Please add a remote MCP server:
   Name: VRAMGlass
   URL: https://mcp.vramglass.com/mcp
   Header: Authorization: Bearer YOUR_KEY
   Once it's added, try it by checking what an RTX 4090 costs right now.
   ```

### Manual configuration

Any client needs two things: the server URL and the header `Authorization: Bearer YOUR_KEY`. Choose HTTP (Streamable HTTP) as the transport.

Claude Code:

```sh
claude mcp add --transport http vramglass https://mcp.vramglass.com/mcp --header "Authorization: Bearer YOUR_KEY"
```

Cursor and other clients that use `mcp.json`:

```json
{
  "mcpServers": {
    "vramglass": {
      "url": "https://mcp.vramglass.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_KEY" }
    }
  }
}
```

### With OAuth (no key)

Connectors in Claude (web and desktop) and ChatGPT can't take a key, so they sign in instead. Any other client that supports MCP OAuth works the same way.

- **Claude:** Settings → Connectors → Add custom connector, and paste the server URL.
- **ChatGPT:** Settings → Apps, turn on developer mode, then create an app with the server URL.

Your client opens the VRAMGlass login page. Log in with an email code or Google (an account is created if you don't have one), check the app name and return address on the consent page, and click Allow. The client renews access by itself; after 30 days without use, or 180 days after you authorized it, you sign in again.

## Tools

The assistant picks the right tool by itself; you don't need to remember these names.

| Tool | What it returns |
| --- | --- |
| `get_prices` | Current prices for GPUs or memory on eBay, Xianyu and Amazon Japan: median, price range and listing count |
| `get_price_history` | Daily price history, from the last 7 days up to a year |
| `get_price_index` | The GPU price index and its price tiers |
| `search_hardware` | Find GPUs and memory by name |
| `search_models` | Find LLMs and their versions by name |
| `models_for_gpu` | Which models a GPU can run, and how long a context fits |
| `gpus_for_model` | How much VRAM a model needs, which cards can run it and roughly how fast |
| `recommend_build` | PC builds for a model (GPU, CPU, motherboard, RAM, PSU) with an optional budget, prices and buy links; GPU options instead when the model fits entirely in VRAM |
| `get_ebay_deals` | The eBay deals shown on GPU and memory pages: the same rule-filtered listings, up to six |
| `get_rent_prices` | Hourly rental prices on RunPod, Vast.ai and Compshare |

All tools are read-only. Results include a `page_url` and a `cite` line for attribution.

## Keys, access and usage

- A key or an authorized app can only read public price and spec data. It can't see your email, price alerts or watchlist, and it can't change anything in your account.
- Use one key per tool so you can turn each one off separately. Each account can have up to 5 keys, each valid for up to one year.
- Revoke a key or disconnect an app under [MCP connections](https://vramglass.com/account/mcp); it stops working within 30 seconds.
- It's free. If you call it too often you'll be rate-limited briefly, then it recovers on its own. For heavy use, email [hello@vramglass.com](mailto:hello@vramglass.com).

## Data and citation

- Prices are the median asking price of listings currently for sale, not sold prices. eBay (US), Xianyu (China) and Amazon.co.jp (Japan) are counted separately.
- Prices update every 6 hours. A round with too few valid listings doesn't publish a price.
- When you cite the data, credit "VRAMGlass", link back to the relevant page and state the marketplace and date of each price.
- VRAM and speed figures are planning estimates, not benchmarks. See the [methodology](https://vramglass.com/methodology).
- Buy and rental links carry VRAMGlass referral parameters. The price you pay stays the same.

## About this repository

This repository holds the documentation and the [MCP Registry](https://registry.modelcontextprotocol.io) metadata ([`server.json`](server.json)) for the hosted VRAMGlass MCP server. The server runs on VRAMGlass infrastructure; its source code is not published here, and there is nothing to install.

The server is listed in the official MCP Registry as [`com.vramglass/mcp`](https://registry.modelcontextprotocol.io/v0.1/servers?search=com.vramglass%2Fmcp), verified through the vramglass.com domain.

- Questions and data corrections: [hello@vramglass.com](mailto:hello@vramglass.com)
- Documentation issues: [open an issue](https://github.com/vramglass/mcp/issues)
- Security reports: see [SECURITY.md](SECURITY.md)
- Embeddable price cards for websites: [vramglass/widgets](https://github.com/vramglass/widgets)

The contents of this repository are licensed under [MIT](LICENSE). The hosted service and its data are provided under the [VRAMGlass terms](https://vramglass.com/terms), separately from this license.
