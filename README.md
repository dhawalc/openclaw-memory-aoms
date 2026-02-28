🌐 **Live Demo:** [dhawalc.github.io/openclaw-memory-aoms](https://dhawalc.github.io/openclaw-memory-aoms/)

# openclaw-memory-aoms

**AOMS HTTP memory backend plugin for OpenClaw**

Connects OpenClaw to [AOMS (Always-On Memory Service)](https://github.com/yourusername/aoms) via HTTP, providing persistent tiered memory with semantic search, procedural learning, and episodic recall.

## Features

- 🔍 **Semantic search** - Query memory with natural language
- 💾 **Tiered storage** - Episodic, semantic, procedural, working memory
- ⚡ **Fast HTTP API** - Local-first, sub-100ms queries
- 🔒 **Optional auth** - API key support via environment variables
- 📊 **Weight tuning** - Adjust memory importance dynamically

## Installation

```bash
npm install openclaw-memory-aoms
```

## Configuration

Add to your OpenClaw config (YAML or JSON):

### YAML (`~/.openclaw/config.yml`)

```yaml
plugins:
  - id: openclaw-memory-aoms
    config:
      baseUrl: http://localhost:9100
      timeoutMs: 10000
      apiKey: ${AOMS_API_KEY}  # Optional, reads from environment
```

### JSON (`~/.openclaw/config.json`)

```json
{
  "plugins": [
    {
      "id": "openclaw-memory-aoms",
      "config": {
        "baseUrl": "http://localhost:9100",
        "timeoutMs": 10000,
        "apiKey": "${AOMS_API_KEY}"
      }
    }
  ]
}
```

## Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `baseUrl` | string | `http://localhost:9100` | AOMS HTTP API endpoint |
| `timeoutMs` | number | `10000` | Request timeout (500-120000ms) |
| `apiKey` | string | - | Optional bearer token (supports `${ENV_VAR}` syntax) |

## Tools Provided

### `memory_search`

Search memory entries across all tiers.

**Parameters:**
- `query` (string, required) - Search query text
- `maxResults` (number, optional) - Maximum results to return
- `minScore` (number, optional) - Minimum relevance score threshold

**Example:**
```typescript
// In your agent/skill code
const results = await tools.memory_search({
  query: "What did we decide about the API design?",
  maxResults: 5,
  minScore: 0.7
});
```

### `memory_write`

Write a new memory entry to a specific tier.

**Parameters:**
- `tier` (string, required) - Memory tier: `episodic`, `semantic`, `procedural`, `working`
- `text` (string, required) - Memory content
- `metadata` (object, optional) - Additional structured data

**Example:**
```typescript
await tools.memory_write({
  tier: "episodic",
  text: "User prefers dark mode and compact UI layouts",
  metadata: {
    category: "preferences",
    confidence: 0.95
  }
});
```

### `memory_weight`

Adjust the importance/weight of an existing memory entry.

**Parameters:**
- `id` (string, required) - Memory entry ID
- `weight` (number, required) - New weight value (typically -1 to 1)
- `tier` (string, optional) - Tier hint for faster lookup

**Example:**
```typescript
await tools.memory_weight({
  id: "mem_abc123",
  weight: 0.9  // Boost importance
});
```

## Usage with AOMS

This plugin requires a running AOMS instance. Quick start:

```bash
# Clone AOMS
git clone https://github.com/yourusername/aoms.git
cd aoms

# Install dependencies
pip install -r requirements.txt

# Start AOMS HTTP server
python -m aoms.server --port 9100
```

See [AOMS documentation](https://github.com/yourusername/aoms) for advanced setup (embeddings, vector stores, multi-tier config).

## Development

```bash
# Clone this repo
git clone https://github.com/yourusername/openclaw-memory-aoms.git
cd openclaw-memory-aoms

# Install dependencies
npm install

# Build
npm run build

# Test locally (link into OpenClaw)
npm link
cd ~/.openclaw
npm link openclaw-memory-aoms
```

## Architecture

```
OpenClaw Agent
      ↓
  Plugin Tools (memory_search, memory_write, memory_weight)
      ↓
  HTTP POST to AOMS API
      ↓
  AOMS Server (4-tier cortex, vector search, procedural learning)
      ↓
  ChromaDB / JSON storage
```

## License

MIT

## Links

- **AOMS**: https://github.com/yourusername/aoms
- **OpenClaw**: https://openclaw.ai
- **Plugin Docs**: https://docs.openclaw.ai/plugin
- **Issues**: https://github.com/yourusername/openclaw-memory-aoms/issues

## Contributing

PRs welcome! Please ensure:
- TypeScript compiles without errors
- No `any` types (use TypeBox schemas)
- Follow existing code style
- Update README if adding features

---

**Made with 🧠 for autonomous AI agents**
