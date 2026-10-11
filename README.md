# computer-use-server

Kaggle free-GPU notebooks that run a computer-use vision model and expose it as an OpenAI-compatible API for Open Interpreter on your PC.

Primary notebook: `kaggle_ui_tars_server.ipynb` (UI-TARS-1.5-7B, ~42% OSWorld, purpose-built for clicking/gui).
Legacy notebook: `kaggle_computer_use_server.ipynb` (Qwen2.5-VL-7B, general vision, ~31% OSWorld).

## Two servers, one API

The UI-TARS server exposes:

| Endpoint | Purpose |
| --- | --- |
| `POST /v1/chat/completions` | OpenAI-style chat. Accepts plain text **and** `image_url` parts (base64 data URLs) so it can receive screenshots from the local MCP `ai_vision` tool. |
| `GET /health` | `{"status": "ok", "model": MODEL_ID}` |
| `GET /prompt` | Read the system prompt. |
| `POST /prompt` | Replace the system prompt (no restart needed). |
| `GET /stats` | Uptime + model info. |
| `POST /shutdown` | Ends the Kaggle kernel so the GPU meter stops. |

## Setup

1. Open `kaggle_ui_tars_server.ipynb` on Kaggle.
2. Settings → Accelerator → **GPU T4 x2** → Save.
3. Run cells 1–4 (model loads, FastAPI starts on port 8000, Gradio UI opens on port 7860).
4. Run cell 5 in a separate terminal to open the cloudflared tunnel.
5. Paste the public URL into Open Interpreter on your PC.

### Vision screenshots

The local MCP `ai_vision` tool POSTs screenshots to `/v1/chat/completions` as OpenAI-style `image_url` parts:

```json
{
  "model": "ui-tars",
  "messages": [
    {
      "role": "user",
      "content": [
        {"type": "text", "text": "What do you see?"},
        {"type": "image_url", "image_url": {"url": "data:image/png;base64,<base64>"}}
      ]
    }
  ]
}
```

## Local testing (no Kaggle)

You can run the same server + Gradio UI on your own PC if you already have the model locally.
See `local_run.md` (optional) — or run cells 3–4 in a Jupyter kernel and put your local model on the GPU.

## Limits

- ~30 GPU hours per week, resets weekly on Kaggle.
- Session dies after ~60 min idle; the tunnel URL changes each session.
- Phone-verified Kaggle account required for GPU access.

