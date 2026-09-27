# Quickstart & Per-Client Guides

## Universal (any MCP client)

1. Create an API key at [app.kmingai.com](https://app.kmingai.com) (Settings → Developer).
2. Add remote MCP server: URL `https://mcp.kmingai.com/mcp`, header `Authorization: Bearer <key>`.
3. Verify: ask a question that requires your data — answers include sample sizes.

Raw config for clients that accept `mcp.json`:

```json
{
  "mcpServers": {
    "kongming": {
      "type": "streamableHttp",
      "url": "https://mcp.kmingai.com/mcp",
      "headers": { "Authorization": "Bearer ${KONGMING_API_KEY}" },
      "timeout": 30000
    }
  }
}
```

## Doubao Work (豆包工作)

Desktop app → 「技能 · 连接器 · 伙伴」→ New custom connector → paste URL + `Authorization` header.
Full illustrated guide: https://kongming-doubao.jetr.within-7.com

## Tencent WorkBuddy

Two paths: connector marketplace (search KongMing-AI) after listing, or direct `mcp.json` editing (client 5.6.0+).
Full guide: https://kongming-doubao.jetr.within-7.com (Doubao-flavored; WorkBuddy-specific page coming)

## Error codes

| Code | Meaning |
|---|---|
| -32001 | Invalid/expired/revoked key (401) |
| -32002 | Not in grant / parameter whitelist violation |
| -32007 | Tool not yet open (e.g. media_estimate_price) |
| -32008 | Per-key usage cap reached *(reserved, ships with billing window)* |
| -32009 | Insufficient credits *(reserved, ships with billing window)* |

## Support

www.kmingai.com · console: app.kmingai.com
