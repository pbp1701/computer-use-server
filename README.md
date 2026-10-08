# computer-use-server

Kaggle free-GPU notebooks that run a computer-use vision model and expose it as an OpenAI-compatible API for Open Interpreter on your PC.

## Notebooks

- `kaggle_computer_use_server.ipynb` — Qwen2.5-VL-7B-Instruct (general vision model, ~31% OSWorld)
- `kaggle_ui_tars_server.ipynb` — UI-TARS-1.5-7B (purpose-built computer-use model, ~42% OSWorld, better clicking)

## Setup (either notebook)

1. Open the notebook on Kaggle
2. Settings -> Accelerator -> GPU T4 x2 -> Save
3. Run cells 1-4 (model loads, server starts on port 8000)
4. Run cell 5 in a separate terminal to open the cloudflared tunnel
5. Paste the printed public URL into Open Interpreter on your PC

## Limits

- ~30 GPU hours per week, resets weekly
- Session dies after ~60 min idle; tunnel URL changes each session
- Phone-verified Kaggle account required for GPU access
