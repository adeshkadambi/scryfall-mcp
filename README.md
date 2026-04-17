# Scryfall MCP Server

A [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) server for interacting with the [Scryfall](https://scryfall.com/docs/api) API. It provides tools to look up Magic: The Gathering card details, card rulings, and price information.

![Scryfall MCP Server](img/sorcerer.jpg)

## Features

- **search_cards**  
  Perform a text-based search on Scryfall using a full query string (e.g. `t:goblin pow=2 o:haste`). Returns a list of matching cards with basic fields (name, set, collector number, ID).
- **get_card_by_id**  
  Retrieve a card by its Scryfall UUID. Returns key fields: name, mana cost, type line, oracle text, power, toughness, color identity, game changer status, and Commander legality.
- **get_card_by_name**  
  Retrieve a card by its exact English name. Returns the same filtered fields as `get_card_by_id`.
- **random_card**  
  Get a random card from the entire Scryfall database. Returns the full card data in JSON.
- **get_rulings**  
  Retrieve official rulings for a card by its Scryfall ID or Oracle ID. Each ruling includes a `published_at` date and a `comment` explaining the ruling.
- **get_prices_by_id**  
  Retrieve current pricing information (USD, USD foil, EUR, TIX, and more) for a given card by Scryfall ID.
- **get_prices_by_name**  
  Retrieve current pricing information (USD, USD foil, EUR, TIX, and more) for a given card by exact name.
- **get_collection**  
  Retrieve a batch of up to 75 cards in a single request by providing an array of identifiers. Each identifier can be a Scryfall `id`, card `name`, `name` + `set`, `collector_number` + `set`, `mtgo_id`, `multiverse_id`, `oracle_id`, or `illustration_id`. Returns the matched cards with filtered fields and a `not_found` list for any unresolved identifiers.

## Usage

The server can be run in two modes:

1. Standard stdio mode (default)
2. Server-Sent Events (SSE) mode with HTTP endpoints

### Using NPX

If you have Node.js installed locally:

```bash
# Stdio mode
npx scryfall-mcp-server

# SSE mode
npx scryfall-mcp-server --sse
```

### Connecting to the Server

#### Stdio Mode

Your application or environment (like Claude Desktop) can communicate directly via stdio with the server.

#### SSE Mode

When running in SSE mode (with `--sse`), you can connect using the MCP CLI:

```bash
npx @wong2/mcp-cli --sse http://localhost:3000/sse
```

The server will be available at:

- SSE endpoint: `http://localhost:3000/sse`
- Message endpoint: `http://localhost:3000/messages`

### Integration in claude_desktop_config.json

Example snippet for stdio mode:

```json
{
  "mcpServers": {
    "scryfall": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "mcp/scryfall"]
    }
  }
}
```

Or with npx:

```json
{
  "mcpServers": {
    "scryfall": {
      "command": "npx",
      "args": ["scryfall-mcp-server"]
    }
  }
}
```

### Building from Docker

```bash
docker build -t mcp/scryfall .
```

Then you can run in stdio mode:

```bash
docker run -i --rm mcp/scryfall
```

Or in SSE mode:

```bash
docker run -i --rm -p 3000:3000 mcp/scryfall --sse
```

## License

Licensed under the MIT License.
