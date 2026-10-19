# QimenMind Qimen Dunjia MCP Server (奇门遁甲排盘 MCP)

A free, hosted [Model Context Protocol](https://modelcontextprotocol.io) server that casts **Qimen Dunjia (奇门遁甲)** charts for AI assistants. It is run by [**QimenMind**](https://www.qimenmind.com/en), a free online Qimen Dunjia calculator and San Yuan Qimen (三元奇门) AI reading tool.

- **Endpoint:** `https://www.qimenmind.com/api/mcp`
- **Transport:** Streamable HTTP. No install, no API key.
- **Registry name:** `io.github.xlw-pro/qimenmind` ([Official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=qimenmind))

This repository holds the connection docs and the `server.json` registry metadata. The server itself runs on qimenmind.com.

## Tools

| Tool | What it does |
|------|--------------|
| `qimen_chart` | Casts a chart for a local clock date and time (`year`, `month`, `day`, `hour`, optional `minute`; years 1900–2200). |
| `qimen_chart_now` | Casts the chart for the current moment in an IANA timezone (`timezone`, default `Asia/Shanghai`). |

Each chart returns the four pillars, Yin/Yang Dun and Ju number, Zhi Fu (值符) and Zhi Shi (值使), and all nine palaces with heaven and earth stems, star, door, deity, 81-pattern name, and Void (空亡) and Horse (驿马) markers. Charts use the time-based Chai Bu (拆补) method.

Both tools are read-only and purely computational. The server does not call any AI model.

## Connect

### Claude Code

```bash
claude mcp add --transport http qimenmind https://www.qimenmind.com/api/mcp
```

### Claude Desktop, Cursor, Windsurf and other clients

```json
{
  "mcpServers": {
    "qimenmind": {
      "type": "streamable-http",
      "url": "https://www.qimenmind.com/api/mcp"
    }
  }
}
```

For clients that only support stdio, use [`mcp-remote`](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "qimenmind": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://www.qimenmind.com/api/mcp"]
    }
  }
}
```

### Quick test

```bash
curl -s -X POST https://www.qimenmind.com/api/mcp \
  -H 'content-type: application/json' \
  -H 'accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

## Example prompts

- "Cast a Qimen Dunjia chart for 2026-10-20 15:00 Beijing time."
- "现在起一个奇门遁甲局，看看值符值使落在哪个宫。"
- "Which palace holds the Open Door (开门) right now in New York?"

## Limits

Requests are rate limited to 30 per minute per IP. Please cache charts you reuse.

## Interpreting charts

The MCP returns the chart only. For a full AI reading of the same chart, with follow-up questions, use the [QimenMind Qimen Dunjia calculator](https://www.qimenmind.com/en). The site also offers current-hour charts, an hour-by-hour direction table and Qimen destiny (终身局) charts, in English, 简体中文, 繁體中文, 日本語 and Español.

## Links

- Website: [qimenmind.com](https://www.qimenmind.com/en)
- AI context: [llms.txt](https://www.qimenmind.com/llms.txt)

## License

The documentation and metadata in this repository are released under the MIT License.
