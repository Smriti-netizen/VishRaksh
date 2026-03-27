# Vish-Rakshak (VishRaksh)

Snakebite clinical decision support: **PaliGemma 2** describes the wound from an image; **MedGemma** produces patient- or doctor-oriented clinical guidance for the Indian context.

> **Disclaimer:** This is a research and decision-support prototype. It is **not** a medical device, diagnostic tool, or substitute for emergency care. Always seek immediate professional medical attention for snakebites.

---

## What it does

- **Patient mode:** Plain-language urgency, first steps, what to avoid, warning signs.
- **Doctor mode:** Risk level, envenomation type, anti-venom considerations, labs, monitoring (aligned with WHO-style guidance in prompts).

## Architecture

```
Image → PaliGemma 2 (vision) → Wound description → MedGemma 4B (text) → Clinical report
```

| Component | Role |
|-----------|------|
| **PaliGemma 2 Mix 3B** (448) | Keras Hub / TensorFlow; image description |
| **MedGemma 4B IT** | Hugging Face Transformers; chat-based reasoning |
| **Gradio** | Web UI and optional public link (`share=True`) |

## Repository contents

| File | Description |
|------|-------------|
| `vishrashk.ipynb` | Main Kaggle notebook (model load + inference + optional Gradio cells) |
| `medium_article.md` | Long-form write-up for blog / Medium |
| `README.md` | This file |

## Requirements (Kaggle)

- **GPU:** T4 (or similar) recommended; both models are large.
- **Internet:** On for first-time downloads and Hugging Face access.
- **Hugging Face token:** Required for `google/medgemma-4b-it`.  
  - Add Kaggle secret (e.g. `VishRakshk2`) and use `UserSecretsClient()` as in the notebook.

## Kaggle setup

1. Create / open a notebook from `vishrashk.ipynb`.
2. Enable **GPU** and **Internet** in notebook settings.
3. Add your Hugging Face token under **Secrets** (name must match what your code expects).
4. Run cells **in order** from the top (Keras config + restart if your notebook requires it, then imports, PaliGemma, MedGemma, pipeline).

## Gradio UI (optional)

After the notebook loads both models successfully:

1. Add a cell:

   ```python
   !pip install -q gradio
   ```

2. Add the Gradio cell that defines `analyze_for_gradio`, `gr.Blocks`, and `demo.launch(share=True)`.

3. The notebook prints a **public URL** (e.g. `https://*.gradio.live`). Share it only with people you trust; the link expires (see Gradio’s current policy).

**Markdown output:** Use `gr.Markdown` for the clinical panel so MedGemma’s `**bold**` and lists render correctly instead of showing raw asterisks.

## Local / offline use

Running the full stack locally needs a strong GPU and enough VRAM for both models; the notebook is optimized for **Kaggle**. If you deploy elsewhere, you will need to adjust paths, secrets, and hardware (e.g. quantization).

## Limitations

- Session and link lifetime depend on Kaggle and Gradio, not your code.
- Model quality depends on image quality and prompt design.
- Not validated for regulatory or clinical use.

## Contributing

Issues and PRs are welcome for documentation, UI tweaks, or safer deployment patterns.

## References

- [PaliGemma](https://ai.google.dev/gemma/docs/paligemma)
- [MedGemma](https://ai.google.dev/gemma/docs/medgemma)
- [Gradio](https://www.gradio.app/)
- [Kaggle Notebooks](https://www.kaggle.com/docs/notebooks)

---

**Vish-Rakshak** — *poison protector* — built for awareness and research in rural India snakebite response.

