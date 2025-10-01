---
title: MCP IYP - Exploring Internet Topology with AI-Assisted Queries
summary: Using Model Context Protocol to analyze BGP AS-PATH topology and Internet infrastructure through the Internet Yellow Pages database, revealing the small-world nature of global network connectivity.

tags:
  - Internet Topology
  - BGP
  - Network Analysis
  - AI Tools
  - MCP
date: '2025-10-01T00:00:00Z'

# Optional external URL for project (replaces project detail page).
external_link: ''

image:
  caption: MCP IYP Analysis
  focal_point: Smart

# Slides (optional).
slides: ""
---

# From a Small Idea to a Massive Space to Explore

This morning I spent some time watching a new posted video from Veritasium about the degree of separation: [Something Strange Happens When You Trace How Connected We Are](https://youtu.be/CYlon2tvywA)

I wanted to explore a little bit. How can I use my new setup to interrogate a large data set of internet control plane data? I've been using [Model Context Protocol](https://modelcontextprotocol.io/docs/getting-started/intro) to connect with the [Internet Yellow Pages](https://iyp.iijlab.net/) Neo4j database publicly available.

Through Claude desktop with a new MCP Internet Yellow Pages Connector in place, here is an example of what I could do.

## Initial Prompt and Analysis

Here is the simple prompt that I used:

```
using MCP IYP
find the degree of separation between BGP AS in the AS-PATH
```

It took a couple of minutes to explore the database and extract a comprehensive summary of key findings.

## Key Findings

### 1. Hub ASes - The Internet's Core

**Top Peering Hubs (by PEERS_WITH connections):**
- **AS 6939 (Hurricane Electric)**: 34,773 peers - The most connected AS
- **AS 174 (Cogent)**: 27,726 peers
- **AS 3356 (Level 3/Lumen)**: 26,036 peers
- **AS 24482 (SG.GS)**: 23,169 peers

**Top Transit Providers (by customer count via DEPENDS_ON):**
- **AS 199524**: 111,702 customers
- **AS 20080**: 111,693 customers
- **AS 52320**: 110,925 customers
- **AS 6939 (Hurricane Electric)**: 52,330 customers

### 2. Regional Connectivity Patterns

#### Asia-Pacific
- **Excellent intra-regional connectivity**: Most major Asian ISPs are directly peered (1 degree separation)
- NTT (AS 2914), China Telecom (4134), China Unicom (4837), KDDI (2516), and Singtel (7473) all interconnect directly
- Only **1 exception**: China Unicom → Tencent requires 2 hops

#### Europe
- **Highly meshed network**: Most pairs at 1 degree separation
- Telia (1299), Deutsche Telekom (3320), Orange (5511), and Vodafone (1273) form tight peering fabric
- BT (2856) slightly less connected: 2 degrees to most others

#### Cross-Continental
- **Asia ↔ US**: Mostly 1 degree (NTT directly peers with Google/Cloudflare)
- **Europe ↔ US**: Mostly 1 degree
- **Asia ↔ Europe**: 1 degree (major carriers directly peered)

### 3. Separation Statistics

**Average Degrees of Separation from Major Hubs:**
| Hub AS | Network | Avg Separation | Min | Max |
|--------|---------|----------------|-----|-----|
| 1299 | Telia | **1.66** | 1 | 3 |
| 6939 | Hurricane Electric | **1.68** | 1 | 3 |
| 174 | Cogent | **1.71** | 1 | 3 |
| 3356 | Level 3 | **1.76** | 1 | 3 |

**Path Length Distribution (sample of AS pairs 1000-1050):**
- **34.5%** of pairs: 2 degrees separation
- **32.5%** of pairs: 3 degrees separation
- **5.5%** of pairs: 4 degrees separation

### 4. CDN-to-Content Provider Connectivity

| CDN | Content Provider | Separation | Via |
|-----|------------------|------------|-----|
| Cloudflare | Netflix | **1** | Direct |
| Cloudflare | Meta/Facebook | **2** | AS 7018 |
| Cloudflare | Apple | **2** | AS 6894 |
| Akamai | Netflix | **2** | AS 49605 |
| Fastly | All three | **2** | Various |

### 5. Key Network Topology Insights

**Small-World Properties:**
- The Internet exhibits strong "small-world" characteristics
- Most AS pairs are within **2-3 degrees** of separation
- Maximum observed separation in samples: **4 degrees**
- Average separation from tier-1 hubs: **~1.7 degrees**

**Critical Transit ASes:**
- AS 3356 (Level 3), AS 174 (Cogent), AS 3257 (GTT) frequently appear as middle hops
- These tier-1 providers act as "bridges" between smaller ASes

**Relationship Type Impact:**
- **PEERS_WITH** paths are usually shortest (direct connections)
- **DEPENDS_ON** (customer-provider) paths add minimal overhead
- **SIBLING_OF** relationships often provide alternative paths

### 6. Specific Interesting Cases

**Leaf Networks (Small ISPs):**
- Even stub ASes (60000-60010 range) are only **1-2 degrees** from major hubs
- Direct connection to one tier-1 provider gives access to entire Internet within 2-3 hops

**Maximum Separations Found:**
- Some obscure AS pairs reach **3-4 degrees**
- Paths typically traverse: Leaf → Regional ISP → Tier-1 → Target

---

## Practical Implications

1. **Network Resilience**: Multiple short paths exist between most ASes, providing redundancy
2. **Latency Optimization**: With most paths ≤3 hops, BGP routing is surprisingly efficient
3. **Peering Value**: Direct peering dramatically reduces path length (1 vs 2-3 degrees)
4. **Hub Dependency**: Hurricane Electric, Cogent, and Level 3 are critical infrastructure nodes
5. **CDN Efficiency**: Major CDNs are extremely well-connected (1-2 hops to content)

---

## MCP IYP Installation Guide

### What is MCP IYP?

MCP IYP (Internet Yellow Pages via Model Context Protocol) is an MCP server that provides access to the IYP Neo4j database. This allows Claude to query and interact with Internet topology data, including information about ASNs, prefixes, IXPs, and network relationships.

### Prerequisites

Before installing MCP IYP, ensure you have:

- **Claude Desktop App** installed on your system
- **Python 3** (usually pre-installed on macOS/Linux)
- **uvx** (Python package runner) - installed automatically with `uv`

### Installation Steps

#### 1. Install uv (Python Package Manager)

If you don't have `uv` installed, install it first:

**macOS/Linux:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Alternative via pip:**
```bash
pip install uv
```

#### 2. Configure Claude Desktop

You need to edit the Claude Desktop configuration file to add the IYP MCP server.

**Configuration File Location:**

- **macOS:** `~/.config/claude-desktop/claude_desktop_config.json`
- **Linux:** `~/.config/claude-desktop/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

#### 3. Add IYP Configuration

Open the configuration file in your text editor and add the `iyp-neo4j` server configuration:

```json
{
  "mcpServers": {
    "iyp-neo4j": {
      "command": "uvx",
      "args": ["mcp-neo4j-cypher@0.3.0"],
      "env": {
        "NEO4J_URI": "neo4j+s://iyp-bolt.iijlab.net:443",
        "NEO4J_USERNAME": "neo4j",
        "NEO4J_PASSWORD": "neo4j",
        "NEO4J_DATABASE": "neo4j",
        "NEO4J_NAMESPACE": "iyp"
      }
    }
  }
}
```

**If you already have other MCP servers configured**, add the `iyp-neo4j` entry inside the existing `mcpServers` object.

#### 4. Restart Claude Desktop

After saving the configuration file:

1. **Quit Claude Desktop completely** (not just close the window)
2. **Reopen Claude Desktop**

### Verification

To verify the installation worked:

1. Open a new conversation in Claude Desktop
2. Look for a small tools/hammer icon near the message input box
3. Click it to see available MCP servers - you should see "iyp-neo4j" listed
4. Try asking Claude a question like: "Can you show me the Neo4j schema for IYP?"

### Example Usage

After installation, you can ask Claude questions like:

- "What's the schema of the IYP database?"
- "Find all IXPs in Japan"
- "Show me ASNs that peer at TouIX"
- "What prefixes are announced by AS2497?"
- "Find the degree of separation between BGP AS in the AS-PATH"

---

## More Information

- **IYP Project**: https://github.com/InternetHealthReport/internet-yellow-pages
- **MCP Documentation**: https://modelcontextprotocol.io/
- **Neo4j Cypher Guide**: https://neo4j.com/docs/cypher-manual/
