# VeriApi.Pro MCP server — Turkey data for AI agents

[![VeriApi.Pro MCP connector](https://glama.ai/mcp/connectors/io.github.veriapipro/veriapi/badges/score.svg)](https://glama.ai/mcp/connectors/io.github.veriapipro/veriapi)

Hosted (remote) [Model Context Protocol](https://modelcontextprotocol.io) server for **[VeriApi.Pro](https://veriapi.pro)**,
a single-key data API for Turkey. Connect Claude, Cursor, VS Code or any MCP client and ask for live Turkish data:
exchange rates, Grand Bazaar gold prices, fuel prices by province, pharmacies on duty, prayer times, weather,
earthquakes, public holidays and more.

```
https://api.veriapi.pro/v1/mcp
```

- Transport: Streamable HTTP (stateless, JSON responses). Nothing to install or run locally.
- Auth: `X-API-Key: va_…` header (or `Authorization: Bearer va_…`, or `?anahtar=va_…` for clients without custom headers).
- `initialize`, `tools/list` and `ping` work without a key, so you can inspect the tools first.
- **Free plan:** 500 request units per day, no credit card → [veriapi.pro/panel](https://veriapi.pro/panel)
- Responses are Turkish JSON and include the data source.

Registry name: `io.github.veriapipro/veriapi` ([MCP Registry](https://registry.modelcontextprotocol.io)). Also listed on [Glama](https://glama.ai/mcp/connectors/io.github.veriapipro/veriapi).

## Setup

### Claude Code
```bash
claude mcp add --transport http --scope user veriapi https://api.veriapi.pro/v1/mcp \
  --header "X-API-Key: va_YOUR_KEY"
```

### Claude (web / desktop)
Settings → Connectors → Add custom connector → URL:
```
https://api.veriapi.pro/v1/mcp?anahtar=va_YOUR_KEY
```
The key is part of the URL here; do not share it. If it leaks, revoke it in the panel and create a new one.

### Cursor
`~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):
```json
{
  "mcpServers": {
    "veriapi": {
      "url": "https://api.veriapi.pro/v1/mcp",
      "headers": { "X-API-Key": "va_YOUR_KEY" }
    }
  }
}
```

### VS Code
`.vscode/mcp.json` — the key is asked once and stored as a secret:
```json
{
  "inputs": [
    { "type": "promptString", "id": "veriapi-key", "description": "VeriApi.Pro API key", "password": true }
  ],
  "servers": {
    "veriapi": {
      "type": "http",
      "url": "https://api.veriapi.pro/v1/mcp",
      "headers": { "X-API-Key": "${input:veriapi-key}" }
    }
  }
}
```

### Check without a key
```bash
curl -s -X POST https://api.veriapi.pro/v1/mcp -H 'content-type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

## Tools

| Tool | What it returns |
|---|---|
| `doviz`, `doviz_gecmis`, `cevir_doviz` | Exchange rates (Grand Bazaar live or central bank reference), 5-year history, currency converter |
| `altin`, `altin_gecmis` | Gold prices (gram, quarter, half, full, ounce…) and history |
| `piyasa_fiyat` | Live market prices: currencies, gold, silver, commodities, crypto |
| `akaryakit`, `akaryakit_turkiye`, `akaryakit_gecmis` | Fuel prices (gasoline, diesel, LPG) by province and distributor, national average, history |
| `eczane_nobetci` | Pharmacies on duty by province / district |
| `namaz` | Prayer times, iftar and sahur |
| `hava`, `hava_detay` | Weather: current, hourly and 7–16 day forecast |
| `deprem_son` | Recent earthquakes in and around Turkey |
| `takvim_tatiller`, `takvim_gun` | Public and religious holidays, working days, week numbers |
| `konumlar_iller`, `konumlar_iller_il_ilceler`, `konumlar_bul` | 81 provinces, districts, location lookup by name or coordinates |
| `ekonomi_enflasyon`, `mevzuat_asgari_ucret` | Inflation (CPI) and minimum wage |
| `vergi_maas`, `hesapla_kredi`, `hesapla_mevduat`, `hesapla_kidem` | Salary gross↔net, loan, deposit and severance calculators |
| `uc_ara` | Search all 100+ API endpoints (e.g. "şarj istasyonu", "posta kodu", "resmi gazete") |
| `uc_cagir` | Call any `GET /v1` endpoint found with `uc_ara` |

Some tools need a paid plan; each tool's description says so. Usage counts against the same daily quota as the REST API.

## Example prompts

- "İstanbul'da bugün nöbetçi eczaneler hangileri? Kadıköy olanları listele."
- "Gram altın ve dolar şu an kaç TL, dünden bu yana ne kadar değişti?"
- "Ankara'da benzin ve motorin fiyatlarını dağıtıcılara göre karşılaştır."
- "Son 24 saatte Türkiye'de 3'ten büyük deprem oldu mu?"
- "What are today's prayer times in Izmir?"

## Links

- Website: [veriapi.pro](https://veriapi.pro) · AI setup guide (Turkish): [veriapi.pro/yapay-zeka](https://veriapi.pro/yapay-zeka)
- Docs: [veriapi.pro/dokumantasyon](https://veriapi.pro/dokumantasyon) · OpenAPI: [api.veriapi.pro/v1/openapi.json](https://api.veriapi.pro/v1/openapi.json)
- SDKs: [PHP](https://github.com/veriapipro/veriapi-php) · [Python](https://github.com/veriapipro/veriapi-python) · [JavaScript](https://github.com/veriapipro/veriapi-js) · [Go](https://github.com/veriapipro/veriapi-go)

---

## Türkçe

VeriApi.Pro'nun barındırılan MCP sunucusu: `https://api.veriapi.pro/v1/mcp`. Claude, Cursor, VS Code ve diğer MCP
istemcilerine bağlayıp döviz, altın, akaryakıt, nöbetçi eczane, namaz vakti, hava durumu, deprem ve resmî tatil verisini
sohbetten sorabilirsiniz. Kurulum yok; anahtar `X-API-Key` başlığında. Ücretsiz paket günde 500 istek birimi.
Ayrıntılı kurulum ve hazır istemler: [veriapi.pro/yapay-zeka](https://veriapi.pro/yapay-zeka).

This repository holds documentation and registry metadata only; the server itself is hosted by VeriApi.Pro.
License: MIT (this repository).
