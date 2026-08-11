# Armasourcing MCP Server

Hire vetted Filipino virtual assistants from inside any AI assistant.

This is a remote [Model Context Protocol](https://modelcontextprotocol.io) server that lets
Claude, ChatGPT, Cursor, or any MCP client check the depth of Armasourcing's vetted talent pool,
explain how hiring and engagement actually work, and hand the user a link to book a free
discovery call.

No install, no API key, no npm package. It is a hosted Streamable HTTP endpoint.

```
https://chatgpt.armasourcing.com/mcp
```

## Tools

| Tool | Read-only | What it does |
| --- | :---: | --- |
| `search_talent_pool` | yes | Checks how much depth the pool has for a given role, skills and timezone. Returns aggregate coverage, typical experience and the most common skills and tools. |
| `list_supported_roles` | yes | The roles Armasourcing actually places, so an assistant does not invent one. |
| `get_hiring_process` | yes | The real steps and timelines from first call to someone working. |
| `get_engagement_terms` | yes | Commercial terms, guarantees, replacement cover and what is included. |
| `get_booking_link` | no | Returns the URL to book a free 15 minute discovery call, plus what to have ready. |

## What this server deliberately does not return

Two things are withheld by design, and any assistant using these tools is instructed not to fill
the gap by guessing:

- **No prices.** Rates are quoted per role on the call. The tools carry no amounts, and
  `get_engagement_terms` explicitly tells the model to refuse to estimate rather than invent a
  figure.
- **No candidate names or profiles.** `search_talent_pool` reports pool *depth*, never
  individuals. Real applicants are not enumerable through this server. Candidate details are
  shared with a client after a discovery call, not before one.

That makes it safe to expose publicly, and it means answers you get through it are honest about
what is and is not known.

## Connect

### Claude

Settings → Connectors → Add custom connector, then paste the URL above. Requires a paid plan.

### ChatGPT

Settings → Connectors → Add, then paste the URL above.

### Cursor

`~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "armasourcing": {
      "url": "https://chatgpt.armasourcing.com/mcp"
    }
  }
}
```

### VS Code

`.vscode/mcp.json`:

```json
{
  "servers": {
    "armasourcing": {
      "type": "http",
      "url": "https://chatgpt.armasourcing.com/mcp"
    }
  }
}
```

### Anything else

Any client that speaks MCP over Streamable HTTP. There is no OAuth and no key, so a URL is all
you need.

```bash
curl -s -X POST https://chatgpt.armasourcing.com/mcp \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

## Example prompts

Once connected, these each exercise a different tool:

- *"Is there real depth in the Armasourcing pool for a Shopify and customer support VA who can cover US Eastern hours?"*
- *"What roles does Armasourcing actually place?"*
- *"How does hiring through Armasourcing work, and how long until someone is working?"*
- *"What are the guarantees and what happens if the hire does not work out?"*
- *"I'm ready to talk to someone. How do I book a call?"*

## Protocol

- Transport: Streamable HTTP. `POST` returns JSON. `GET` returns 405, there is no SSE stream.
- Protocol version: `2025-06-18`, with `2025-03-26` and `2024-11-05` accepted.
- Stateless. No session header required.
- Every tool carries `readOnlyHint` and `destructiveHint` annotations.

## About

[Armasourcing](https://armasourcing.com) places vetted Filipino virtual assistants and managed
contact center teams with businesses in the US, UK, Australia and New Zealand.

- [Privacy policy](https://armasourcing.com/privacy/)
- [Terms and conditions](https://armasourcing.com/terms-and-conditions/)
- Support: hello@armasourcing.com

## License

MIT. See [LICENSE](LICENSE).
