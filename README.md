# Classic23

A local chat interface inspired by the early ChatGPT UI, with streaming replies, saved conversations, model switching, dark and light themes, and model customization.

## Run

Install Node.js 22 or later and Ollama. Install the models you want in Ollama, start Ollama, then run:

```sh
node server.mjs
```

Open http://localhost:3210. No npm dependencies or build step are required. The server expects Ollama at http://127.0.0.1:11434.

The model panel discovers installed models from your local Ollama runtime. This repository does not include a personal model catalog, model weights, or custom SVG logo files. Models use a generic inline icon; you can add your own icons through Model controls → Customize.

The current server uses a Qwen default for chat titles; configure the default in server.mjs and dist/app.js for your installation. Review the chat and preferences storage paths in server.mjs before running on another machine.

The interface accepts local-network connections. Conversations are stored by connecting IP, and model compute is shared. Saved chats and customization preferences are local data and are not included in this repository.
