# Media Generator — Images & Audio

Generates and edits images and audio: pictures from a description, background removal and upscaling, music, speech, voice, sound effects, memes, floor plans, and room layouts. Generation uses account credits.

| | |
|---|---|
| **Registry name** | `ai.saifs/media-generator-images-videos` |
| **Endpoint** | `https://mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-media-generator": {
      "type": "http",
      "url": "https://mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-media-generator https://mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What you send | What comes back |
|---|---|---|
| `generate_image` | A description, up to 500 characters, and a category: Simple Generation, Painting, Sculpture, Photography, Portrait, Landscape, Abstract, Fantasy, Anime, or Digital Art | An image |
| `edit_image_with_text` | An image (JPG or PNG) and the change you want | The edited image |
| `remove_background` | An image | The same subject with the background removed |
| `upscale_image` | An image and the upscale factor. JPEG, JPG, PNG, or WEBP, up to 500×500 or 3MB | A sharper, higher-resolution image |
| Text Image Generator | A description that includes the words to draw, such as a birthday card or a sale banner | An image with that text in the picture |
| Meme Generator | A meme prompt | An image with the caption |
| Cloth Changer | A person photo (full body or upper body), a clothing image (flat-lay or on a model), and a body part: upper, lower, or full for dresses | The person wearing that garment |
| Floor Plan | Plot dimensions and the rooms you need | A floor-plan image from that description |
| Room Designer | A photo of the room and a photo of the product | The room shown with that product in it |
| `music_generator` | A description up to 500 characters, one or more styles, and a length from 10 to 120 seconds | A music clip |
| Vocal Remover | An audio track | Vocals and instrumental separated |
| `text_to_speech` | The text, a language, and a voice | Spoken audio |
| Voice Cloner | A sample of speech you have the right to use, about 15–30 seconds and clear, plus the text to speak | That voice saying the text |
| Sound Effects | A description of the effect, and a video to attach it to | The effect added to that video |
| `get_music_categories` | Nothing | The music styles |
| `get_languages` | Nothing | The speech languages |
| `search` | A query | Matching tools on this server |
| `fetch` | One tool name | That tool’s details |
| `health` | Nothing | A health check |

A floor plan from a description is a picture to discuss. It is not a construction drawing. A cloned voice is the sample you uploaded.

## More detail

What each tool takes, why the model should call it, and what a floor plan is for are in [docs/README.md](docs/README.md).

## Example prompts

- “Photorealistic sunset over the ocean.”
- “Remove the background from this product photo.”
- “Compose 30 seconds of ambient electronic music.”
- “Read this product line in Hindi.”
- “A birthday card that says Happy Birthday, with colorful balloons.”
- “A 30 by 40 plot with two bedrooms, a kitchen, and one bathroom.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
