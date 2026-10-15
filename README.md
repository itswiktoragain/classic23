# Classic23

Open http://127.0.0.1:3210/ or choose **Classic23** in your applications menu.

The local interface has separate browser routes: `/chat` for the conversation, `/models` for model controls, and `/about` for the local-app information panel. The root route remains a chat landing page.

A local interface inspired by the early ChatGPT page: welcome cards, Qwen-generated chat titles, saved conversations, alternating message rows, dark and light themes, streaming responses, stop/regenerate, copy, local feedback, rename/delete and JSON export. Icons are inline SVGs. The provided Qwen SVG uses a mask to punch the white regions through the black shape; the original file is untouched.

## Portable model environment

Classic23 bundles no models. Install Ollama, pull whichever local models you want, and start the included Node server. The Model controls panel discovers every installed Ollama model automatically. Uncatalogued models receive a generic mark until you assign an SVG through **Model controls → Customize**. Custom icons and the Arena judge preference are saved outside the project in `~/.local/share/qwen-classic/preferences.json`.

Arena sends prompts to the installed models, then asks the selected private judge model to choose the most accurate answer. Change that judge from the same Customize panel.

## Installed model

`qwen:7b-chat-q4_0` — original Qwen architecture, 7.72B parameters, Q4_0 quantization, approximately 4.5 GB. This is the first-generation 2023 Qwen-7B-Chat family, not the newer Qwen 1.5 default. The exact August 2023 launch checkpoint has not been verified for these packaged weights.

Source: https://ollama.com/library/qwen:7b-chat-q4_0
Model layer SHA256: 1076c950e4af194c3f7a13aefb551346ce0ade8bc492994c8bcc608dacf53fa8

The runtime uses your NVIDIA GPU automatically. Replies are capped at 1,024 tokens and the working context is 4,096 tokens. Very long conversations may lose their oldest context. Feedback buttons record your preference locally; they do not train the model.

## Privacy and storage

The model runtime binds only to 127.0.0.1; the interface accepts local-network connections so one computer can share its compute. Each conversation has a UUID and is saved as its own file under `/home/wiktor/.local/share/qwen-classic/chats/`. Each connecting IP receives its own history directory, while every IP uses the same local model runtime. Clearing site data or using a browser-storage extension does not remove chats. Deleting or clearing conversations in the app removes that IP's corresponding local files.

Application source is in this folder. Runtime and model files are in `/home/wiktor/.local/share/qwen-classic/`. The two user services start at sign-in:

```sh
systemctl --user status qwen-classic-model qwen-classic-ui
systemctl --user restart qwen-classic-model qwen-classic-ui
```

Stop and disable automatic startup:

```sh
systemctl --user disable --now qwen-classic-ui qwen-classic-model
```

Keep this source folder in place: the installed interface service points to `server.mjs` here. No npm dependencies or build step are needed.

## Model controls

The sidebar model controls list every model installed in the local Ollama runtime. Selecting a model applies to the current conversation and becomes the default for new chats. Earlier replies retain their original model label. Model switching is disabled while a reply is generating.

**Arena** is a local ensemble mode. It asks every installed catalog model for a response in pairs, then asks Qwen privately to select the candidate it judges most accurate. The selected model's original answer, icon, and label are shown in the chat; Qwen's judging text is never displayed. Arena remains selected for later turns. Arena prioritizes factual accuracy rather than response agreement, confidence, or writing style. Candidates that time out, loop, or exceed the response limit are stopped and excluded. It takes longer than a normal reply, and the result remains a model judgment rather than independent verification of facts.

Additional historical models:

- Vicuna 7B, 2023: `vicuna:7b`. An early ShareGPT-trained conversation assistant, released two years before this interface's newest models and selected as a much stronger replacement for DialoGPT.
- Mistral 7B Instruct v0.1, September 2023: `mistral:v0.1`. Initial instruction-tuned release.
- Gemma 2B Instruct, February 2024: `gemma:2b-instruct-q4_0`. Original release, distinct from `2b-instruct-v1.1-q4_0` (verified different model-layer hashes).
- GPT-2, 2019: `hf.co/tensorblock/gpt2-xl-GGUF:Q3_K_M`. The original GPT-2 XL family checkpoint in a compact GGUF conversion, adapted to the chat interface with a short question-and-answer completion prompt, repetition controls, and a small local guard for greetings and obvious forum-text continuations. It remains a base completion model rather than an instruction-tuned assistant.
- StableLM Zephyr 3B, 2023: `hf.co/TheBloke/stablelm-zephyr-3b-GGUF:Q4_K_M`. An early compact Stability AI chat model.
- Falcon 7B Instruct, May 2023: `falcon:7b-instruct-q4_0`. The original instruction-tuned Falcon release.

Sources: https://ollama.com/library/vicuna:7b · https://ollama.com/library/mistral:v0.1 · https://ollama.com/library/gemma:2b-instruct-q4_0

Model metadata and logo mappings are in `models.json`. The entries use supplied SVG marks recolored white on transparent backgrounds; Vicuna uses a white local animal mark, Gemma the Google G, Mistral the cleaned pixel emblem, GPT-2 the supplied OpenAI mark, StableLM the supplied Stability AI mark, and Falcon the supplied bird mark. Original source logo files are unchanged. All chat models run locally. Model generation dates refer to original releases, not the dates on which their quantized packages were created.

Validation: all three additional historical models returned successful responses through the local chat backend. Browser checks confirmed switching, remembered selection, the four-model panel, and rendered SVG logos.
