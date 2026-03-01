# 🍌 Nano Banana MCP Server

A custom MCP server for image generation via the Gemini API (Nano Banana 2), built for local integration with Opencode/Antigravity.

## Features

- 🎨 **nanobanana_generate** — Text-to-image generation with styles, aspect ratios, resolutions
- ✏️ **nanobanana_edit** — Edit existing images with natural language
- 🎯 **nanobanana_icon** — Generate app icons, favicons, UI elements in multiple sizes
- 📊 **nanobanana_diagram** — Technical diagrams (flowchart, architecture, database, sequence, mindmap)

## Models

| Model | ID | Best for |
|-------|-----|----------|
| **Nano Banana 2** (default) | `gemini-3.1-flash-image-preview` | Speed, high-volume use |
| **Nano Banana Pro** | `gemini-3-pro-image-preview` | Professional asset production |

## Prerequisites

- Node.js 18+
- `GEMINI_API_KEY` environment variable set

## Setup

```bash
cd ~/.gemini/antigravity/mcp-servers/nanobanana-mcp-server
npm install
npm run build
```

## MCP Configuration

Add to your MCP config (e.g., `.opencode/mcp.json` or VS Code settings):

```json
{
  "mcpServers": {
    "nanobanana": {
      "command": "node",
      "args": ["/Users/recarnot/.gemini/antigravity/mcp-servers/nanobanana-mcp-server/dist/index.js"],
      "env": {
        "GEMINI_API_KEY": "${GEMINI_API_KEY}"
      }
    }
  }
}
```

## Usage Examples

### Generate an image
```
nanobanana_generate(prompt: "A watercolor fox in a snowy forest", aspect_ratio: "16:9", resolution: "2K")
```

### Edit an image
```
nanobanana_edit(image_path: "photo.jpg", prompt: "Add sunglasses to the person")
```

### Create app icons
```
nanobanana_icon(prompt: "coffee cup logo", type: "app-icon", sizes: [64, 128, 256, 512])
```

### Generate a diagram
```
nanobanana_diagram(prompt: "microservices architecture", type: "architecture", style: "professional", complexity: "detailed")
```
