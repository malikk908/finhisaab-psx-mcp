# FinHisaab PSX MCP Server

**FinHisaab MCP** connects compatible AI assistants and developer tools to factual Pakistan Stock Exchange (PSX) research. It is a hosted, read-only [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) service for exploring Pakistani stocks, market activity, mutual funds, company announcements, and related economic data.

- **Product and setup information:** [finhisaab.com/mcp](https://finhisaab.com/mcp)
- **Website:** [finhisaab.com](https://finhisaab.com/)
- **Service:** hosted remote MCP over Streamable HTTP
- **Access:** authenticated FinHisaab MCP credential
- **Purpose of this repository:** product and developer information; it is not the MCP server source code or an installable SDK

## What FinHisaab MCP does

FinHisaab MCP gives an AI client a structured way to request bounded market facts and research from FinHisaab. The MCP server performs supported lookups, screening, calculations, and data normalization; the AI client decides which tools to call and explains the returned facts.

It is designed for questions about PSX-listed companies and the Pakistan market, such as:

- Find a company from its name or ticker and retrieve selected research sections.
- Screen stocks using explicit filters on supported metrics.
- Review company announcements and, when needed, read bounded source-document extracts.
- Compare bounded shareholder return windows, including a separate retained-cash dividend result.
- Inspect market summaries, sector activity, investor flows, and index or stock technical snapshots.
- Discover and research mutual funds, disclosed holdings, and selected Pakistan macroeconomic data.
- With separate account consent, retrieve read-only data from the authenticated user's own FinHisaab portfolios.

Results are factual research, not personalized investment advice or a buy, sell, or hold recommendation. The service does not place trades.

## Connect

Start at [finhisaab.com/mcp](https://finhisaab.com/mcp) for the current connection instructions and endpoint details. FinHisaab MCP is a remote service: a user does not install or run the FinHisaab backend, Node.js, MongoDB, or Redis to connect an AI client.

At a high level:

1. Use an MCP-compatible client that supports remote Streamable HTTP servers and a custom bearer credential or HTTP header.
2. Sign in to FinHisaab and create an MCP API key from the official MCP page.
3. Add the endpoint shown by FinHisaab and store the key in the client's credential settings.
4. Ask the client to discover the available tools, then try `search_stocks` with a company name or ticker.

MCP keys are credentials. Store them securely, do not paste them into ordinary prompts or commit them to source control, and revoke them if exposed. Client-specific setup options vary; follow the live official instructions rather than assuming every client uses the same configuration format.

## Tools and research areas

The full catalog currently provides the following tools. Focused catalog endpoints are available for market, stocks, funds, and portfolio workflows; use the endpoint and tool list shown in the official setup information.

| Area | Tools |
| --- | --- |
| Stock discovery and research | `search_stocks`, `research_stocks`, `screen_stocks`, `calculate_stock_returns`, `research_stock_timeseries`, `list_metrics` |
| Stock and index technicals | `analyze_stock_technicals`, `analyze_index_technicals`, `scan_stock_technicals` |
| Announcements and market | `research_announcements`, `fetch_announcement_document`, `get_market_update`, `research_investor_flows` |
| Sectors and macro data | `research_sectors`, `research_sector_operations`, `research_sbp_easy_data`, `research_inflation` |
| Mutual funds | `search_mutual_funds`, `research_mutual_funds`, `screen_mutual_funds`, `research_mutual_fund_holdings` |
| Private portfolios (opt-in) | `list_portfolios`, `research_portfolio`, `research_portfolio_position` |

Tool availability can evolve. MCP clients should discover the server's current tools rather than rely on a hard-coded tool count.

## Developer notes

- **Protocol:** remote MCP over Streamable HTTP, using JSON-RPC messages through the MCP SDK.
- **Endpoint:** use the current hosted endpoint presented at [finhisaab.com/mcp](https://finhisaab.com/mcp). The website page is the setup and product-information page; use the server URL it provides in the MCP client.
- **Authentication:** a FinHisaab-managed MCP API key, sent using a supported bearer-token or custom-header mechanism. Do not embed credentials in public configuration examples.
- **Request model:** stateless MCP requests; tool schemas validate inputs and responses use structured content.
- **Response semantics:** data is bounded and may include timestamps, units, period basis, methodology references, warnings, partial-result indicators, and explicit reasons for unavailable values.
- **Metric discovery:** use `list_metrics` to find supported metric IDs and whether they can be researched, screened, sorted, or selected.
- **Reliability controls:** endpoints and tools impose request, query-cost, pagination, time, and response-size limits. Some data is cached or reflects the latest completed ingestion.
- **Portfolio privacy:** portfolio tools require authentication, the user's explicit MCP portfolio-access setting, and ownership of the requested portfolio.
- **Source documents:** announcement-document extraction is bounded and restricted to approved public hosts.

### Example research flow

1. Call `search_stocks` to resolve a company name or symbol.
2. Use `research_stocks` for selected company research sections, or `screen_stocks` for explicit filters.
3. Call `list_metrics` first when you need the exact metric ID accepted by research or screening.
4. Check response timestamps, methodology, warnings, coverage, and null reasons before summarizing findings.

## Scope and limitations

FinHisaab MCP is read-only. It does not execute trades, modify a user's account, or expose another user's private portfolio. It is not an unrestricted database interface and does not promise that every FinHisaab website field is available through MCP. Tool descriptions and responses are the source of truth for current capabilities and limitations.

Technical indicators and Buy/Sell labels, where present, summarize documented indicator calculations; they are not investment recommendations. Historical disclosure snapshots, estimated values, source coverage, and freshness should be interpreted according to the methodology exposed with the relevant tool or resource.

## About this repository

This repository is an informational landing page for developers and people discovering the FinHisaab MCP server through GitHub. It contains no runnable server implementation, client library, credentials, or private market data. For product access and the latest connection instructions, visit [finhisaab.com/mcp](https://finhisaab.com/mcp).

**Topics:** `finhisaab` `mcp` `model-context-protocol` `psx` `pakistan-stock-exchange` `pakistan-stock-market` `ai-tools` `financial-data` `stock-research` `mutual-funds`
