[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/ferdousbhai-wsb-analyst-mcp-badge.png)](https://mseep.ai/app/ferdousbhai-wsb-analyst-mcp)

# WSB Analyst MCP Server

[![Trust Score](https://archestra.ai/mcp-catalog/api/badge/quality/ferdousbhai/wsb-analyst-mcp)](https://archestra.ai/mcp-catalog/ferdousbhai__wsb-analyst-mcp)
[![smithery badge](https://smithery.ai/badge/@ferdousbhai/wsb-analyst-mcp)](https://smithery.ai/server/@ferdousbhai/wsb-analyst-mcp)

A Model Context Protocol (MCP) server that provides real-time WallStreetBets data for analysis with Claude or other LLM clients.

<a href="https://glama.ai/mcp/servers/@ferdousbhai/wsb-analyst-mcp">
  <img width="380" height="200" src="https://glama.ai/mcp/servers/@ferdousbhai/wsb-analyst-mcp/badge" alt="WSB Analyst Server MCP server" />
</a>

## Features

- **Fetch WallStreetBets Posts**: Filter posts by score, comment count, and content type
- **Detailed Post Analysis**: Extract comments, links, and metadata from posts
- **External Link Collection**: Gather links being shared in WSB discussions
- **Analysis Templates**: Ready-to-use prompt templates for market analysis
- **Progress Reporting**: Real-time progress updates during data collection
- **MCP Integration**: Seamless integration with Claude Desktop and other MCP clients

## Hosted deployment

A hosted deployment is available on [Fronteir AI](https://fronteir.ai/mcp/ferdousbhai-wsb-analyst-mcp).

## Requirements

- Python 3.12 or higher
- Reddit API credentials
- [Claude Desktop](https://claude.ai/download) or another MCP client

## Installation

### Installing via Smithery

To install WSB Analyst for Claude Desktop automatically via [Smithery](https://smithery.ai/server/@ferdousbhai/wsb-analyst-mcp):

```bash
npx -y @smithery/cli install @ferdousbhai/wsb-analyst-mcp --client claude
```

Clone this repository or download the source files:

```bash
git clone https://github.com/ferdousbhai/wsb-analyst-mcp
cd wsb-analyst-mcp
```

Create a virtual environment and install dependencies:

```bash
# Using uv (recommended)
uv sync
```

## Setting Up Reddit API Credentials

To use this server, you need to create a Reddit application to get API credentials:

1. Log in to your Reddit account
2. Navigate to [https://www.reddit.com/prefs/apps](https://www.reddit.com/prefs/apps)
3. Scroll down and click "create another app..." or "create an app..."
4. Fill in the following details:
   - **name**: WSB Analyst MCP (or any name you prefer)
   - **app type**: select "script"
   - **description**: Optional description of your application
   - **about url**: Can be left blank
   - **redirect uri**: Use `http://localhost:8000` (any valid URL works as we don't use OAuth)
5. Click "create app"
6. After creation, note down:
   - **client_id**: The string under the app name (appears right under "personal use script")
   - **client_secret**: The string labeled "secret"

## Configuration for Claude Desktop

Open Claude Desktop's configuration file:

- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

Add the following configuration (adjust paths as needed):

```json
{
  "mcpServers": {
    "wsb-analyst": {
      "command": "uvx",
      "args": [
        "run",
        "wsb-analyst"
      ],
      "env": {
        "REDDIT_CLIENT_ID": "your_client_id_here",
        "REDDIT_CLIENT_SECRET": "your_client_secret_here"
      }
    }
  }
}
```

Restart Claude Desktop

## Using with Claude Desktop

Once configured, you can interact with the WSB Analyst server through Claude:

1. Open Claude Desktop
2. You should see a hammer icon in the bottom right corner of the message input box
3. Click the hammer to see available tools
4. Access prompt templates via slash commands (e.g., `/analyze_wsb_market`)

Example queries:

- "What are the top trending stocks on WallStreetBets today?"
- "Analyze recent WallStreetBets posts and tell me about potential market opportunities"
- "What external resources are WSB users sharing about AMD stock?"

## Available Tools

### `find_top_posts`

Fetch and filter WSB posts based on criteria.

Parameters:

- `min_score` (default: 100): Minimum score (upvotes) required
- `min_comments` (default: 10): Minimum number of comments required
- `limit` (default: 10): Maximum number of posts to return
- `excluded_flairs` (default: `["Meme", "Shitpost", "Gain", "Loss"]`): List of post flairs to exclude.

### `fetch_post_details`

Fetch detailed information about a specific WSB post including top comments.

Parameters:

- `post_id`: Reddit post ID

### `fetch_batch_post_details`

Fetches details for multiple posts efficiently.

### `fetch_detailed_wsb_posts`

Fetch and filter WSB posts, then get detailed information including top comments and links for each.

### `get_external_links`

Collects all external links from top posts.

### `get_trending_tickers`

Identifies and returns a list of stock tickers frequently mentioned or discussed in recent top WSB posts.

## Prompt Templates

### `/analyze_wsb_market`

Provides a template prompt to guide an LLM in performing a comprehensive market analysis using the available tools (`fetch_detailed_wsb_posts`, `get_external_links`). It instructs the LLM on the structure and focus of the analysis.

### `/find_market_movers`

Creates a prompt focused on what's moving specific stocks or the overall market. This prompt guides the LLM to use tools like `find_top_posts` and `fetch_post_details` or `fetch_batch_post_details`.

## Integrating with Firecrawl MCP Server

For enhanced analysis capabilities, especially when dealing with external links found in WSB posts, you can integrate this server with the [Firecrawl MCP Server](https://github.com/mendableai/firecrawl-mcp-server). This allows your LLM agent to not only identify links shared on WSB but also scrape and analyze the content of those linked pages.

## License

MIT