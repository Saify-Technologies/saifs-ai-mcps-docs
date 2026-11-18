# Media Generator — Images & Audio

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/media-generator-images-videos`. You generate and edit images and audio: a picture from a description, a background removed, music, or a line spoken aloud.

People search for this as an image generator, a background remover, an image upscaler, text to speech, or a music generator. The short install page is the [server README](../README.md). This page is what each tool takes, why a model should call the tool, and what a floor plan is for.

| | |
|---|---|
| **Registry name** | `ai.saifs/media-generator-images-videos` |
| **Endpoint** | `https://mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

You describe a picture, a change, a piece of music, or a line to speak. The server returns the file. Generation uses account credits.

Images cover a new picture, an edit, a background removal, an upscale, words drawn inside the picture, a meme, a garment on a person, a floor plan, and a room with a product placed in it. Audio covers music, a split of vocals and instrumental, speech, a cloned voice, and a sound effect attached to a video you already have.

Where a call has a tool id, that id is the one in the table. The other rows are named for the job they do.

## Why the model should call the tool

A language model can describe a sunset. It cannot hand back the pixels, the WAV, or a background that is actually gone. The tool does that work and returns the file. The description you send is the brief. The category, the length, the language, and the voice are part of the brief, so two calls with the same inputs are the same request.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. For a picture, send the description.

```json
{
  "prompt": "Photorealistic sunset over the ocean",
  "category": "Photography"
}
```

Image descriptions and music descriptions stop at 500 characters. Image categories are Simple Generation, Painting, Sculpture, Photography, Portrait, Landscape, Abstract, Fantasy, Anime, and Digital Art.

Music takes one or more styles and a length from 10 to 120 seconds. Speech takes the text, a language from `get_languages`, and a voice. `get_music_categories` lists the styles.

### Pictures

`generate_image` returns an image from the description and the category. `edit_image_with_text` takes a JPG or PNG and the change you describe. `remove_background` returns the subject with the background gone.

`upscale_image` takes the image and the factor you choose. Accepted files are JPEG, JPG, PNG, or WEBP, up to 500×500 or 3MB.

Text Image Generator draws the words you put in the description, such as a birthday card or a sale banner. Meme Generator takes a meme prompt and returns the captioned image.

Cloth Changer takes a person photo (full body or upper body), a clothing image (a flat-lay or a shot on a model), and a body part: upper, lower, or full for a dress.

AI Floor Plan takes plot dimensions and the rooms you need. The result is a picture to discuss. It is not a construction drawing. AI Room Designer takes a photo of the room and a photo of the product, and shows that product in the room.

### Audio

`music_generator` returns a clip from the description, the styles, and the length. Vocal Remover takes an audio track and returns the vocals and the instrumental separately.

`text_to_speech` returns spoken audio for the text, language, and voice you send. AI Voice Cloner takes a sample you have the right to use, about 15–30 seconds of clear speech, and the text to speak. The voice in the file is that sample.

Sound Effects takes a description of the effect and a video you already have. The effect is added to that video.

### Prompts

- “Photorealistic sunset over the ocean.”
- “Remove the background from this product photo.”
- “Compose 30 seconds of ambient electronic music.”
- “Read this product line in Hindi.”
- “A birthday card that says Happy Birthday, with colorful balloons.”
- “A 30 by 40 plot with two bedrooms, a kitchen, and one bathroom.”

## Tools

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `generate_image` | A description up to 500 characters, and a category | An image | You want words rendered inside the picture. Use Text Image Generator |
| `edit_image_with_text` | A JPG or PNG, and the change | The edited image | You only want the background gone. Use `remove_background` |
| `remove_background` | An image | The subject with no background | You want a new scene. That is `generate_image` or an edit |
| `upscale_image` | An image within 500×500 or 3MB (JPEG, JPG, PNG, WEBP), and a factor | A sharper, higher-resolution image | The file is over that limit. Resize it first |
| Text Image Generator | A description that includes the words to draw | An image with that text | You want a caption added to a photo you already have. That is Meme Generator |
| Meme Generator | A meme prompt | A captioned image | You want a clean product render with no caption |
| Cloth Changer | A person photo, a clothing image, and upper, lower, or full body | The person in that garment | You do not have a photo of the person and of the garment |
| AI Floor Plan | Plot dimensions and the rooms | A floor-plan picture | You need a drawing a builder can stamp. This is a picture from the description |
| AI Room Designer | A room photo and a product photo | The room with that product in it | You only have a text description and no photos |
| `music_generator` | A description up to 500 characters, styles, and 10–120 seconds | A music clip | You want an existing track split into vocals and instrumental. Use Vocal Remover |
| Vocal Remover | An audio track | Vocals and instrumental, separated | You want new music. Use `music_generator` |
| `text_to_speech` | Text, a language, and a voice | Spoken audio | You want a specific person’s voice. Use AI Voice Cloner with a sample you have the right to use |
| AI Voice Cloner | A 15–30 second sample you have the right to use, and the text | That voice saying the text | You do not have the right to use the sample |
| Sound Effects | A description, and a video you already have | The effect on that video | You want a new video created. This call adds a sound to a video you already have |
| `get_music_categories` | Nothing | The style list | You already know the style. Send it to `music_generator` |
| `get_languages` | Nothing | The speech languages | You are generating an image |
| `search` | A query | Matching tools | You already know the tool id |
| `fetch` | One tool name | That tool’s details | You want the file itself |
| `health` | Nothing | A health check | You want a picture or a clip |

## How to read a result

- **The file is the result.** The description you sent is the brief that produced it. Generation uses account credits.
- **An upscale stays inside the file limit.** Over 500×500 or over 3MB, or a type other than JPEG, JPG, PNG, or WEBP, is rejected.
- **Music length is the length you set,** between 10 and 120 seconds.
- **A floor plan is a sketch from the sizes and rooms you typed.** Take it into a conversation. Do not treat it as a site drawing.
- **A cloned voice matches the sample.** Use a recording you have the right to use.

## Data and what this will not do

You send the prompt, the image, the audio sample, or the video the effect attaches to. Each user signs in with OAuth. Credits are on the account. A voice sample is used to speak the text you send. Send a sample you have the right to use.

A floor plan is a picture from the sizes and rooms you typed. A voice file uses the sample you uploaded.

Privacy policy: https://saifs.ai/privacy_policy

## Support

- Install and client setup: [server README](../README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
