# API and MCP

Point your own AI agent at live flight inventory. Plain language in, real fares out.

Search only: there is no booking endpoint. Build the search into your agent and hand the traveller to [flyteai.io](https://www.flyteai.io) to book. Full reference: [flyteai.io/api-docs](https://www.flyteai.io/api-docs).

## Getting a key

Keys are issued on request at [flyteai.io/request-access](https://www.flyteai.io/request-access). A key looks like `flyte_sk_...` and is shown once; only a hash is stored.

## REST

```bash
curl -X POST https://api.flyteai.io/api/process \
  -H "x-api-key: flyte_sk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "message": "ORD to JFK on December 15th",
    "conversation_id": "your-own-id",
    "sender_id": "your-app"
  }'
```

Reuse `conversation_id` to refine a search ("make it business class"). Read `metadata.flightOffer` for the structured flight; the prose in `response` is for people and its wording changes.

## MCP

One read-only tool, `search_flights`, over JSON-RPC at `https://api.flyteai.io/mcp`.

```bash
curl -X POST https://api.flyteai.io/mcp \
  -H "x-api-key: flyte_sk_your_key_here" \
  -H "Authorization: Bearer flyte_sk_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 2,
    "method": "tools/call",
    "params": {
      "name": "search_flights",
      "arguments": { "query": "ORD to JFK on December 15th", "conversation_id": "my-agent-session-1" }
    }
  }'
```
