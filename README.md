

# AI-Voice-Clone-with-Qwen3-TTS
**Zero-watermark, creator-friendly voice cloning via Google Colab using Qwen3-TTS**

Preset voices and voice cloning from your own audio — runs entirely on Colab's GPU. No Alibaba cloud account, no API key, no per-character billing. Model weights pull from HuggingFace, inference stays local.

Built around [Qwen3-TTS](https://github.com/QwenLM/Qwen3-TTS) by Alibaba/Qwen — **Apache 2.0**.

---

## Why Qwen3-TTS?

| Concern | Qwen3-TTS | Chatterbox | Index-TTS2 |
|---|---|---|---|
| License | Apache 2.0 ✅ | MIT ✅ | Commercial license required ⚠️ |
| Watermark | None ✅ | Perth watermark ❌ | None ✅ |
| YouTube Safe | Yes ✅ | Potential flagging ❌ | Unclear ⚠️ |
| Commercial Use | Yes ✅ | Yes ✅ | Separate license needed ⚠️ |
| Voice Cloning | 3-second reference ✅ | Yes ✅ | Yes ✅ |
| Runs Locally | Yes ✅ | Yes ✅ | Yes ✅ |

Chatterbox embeds an imperceptible neural watermark (Perth) into every generated audio file. Not DRM, but detectable by AI content detection tools — a real concern for YouTube creators. Qwen3-TTS generates clean audio with no embedded markers.

---

## Models

All weights are on HuggingFace. The `qwen-tts` package downloads them automatically on first load.

| Model | Size | Purpose |
|---|---|---|
| Qwen3-TTS-12Hz-0.6B-CustomVoice | 2.52 GB | 9 preset voices with style control |
| Qwen3-TTS-12Hz-0.6B-Base | 2.52 GB | Voice cloning from your audio |
| Qwen3-TTS-12Hz-1.7B-CustomVoice | 4.54 GB | Preset voices, higher quality |
| Qwen3-TTS-12Hz-1.7B-Base | 4.54 GB | Voice cloning, higher quality |
| Qwen3-TTS-12Hz-1.7B-VoiceDesign | 4.54 GB | Create voices from text descriptions |

All fit in a Colab T4's 16 GB VRAM. Start with 0.6B, step up to 1.7B if you want better quality.

---

## Notebooks

| Notebook | What it does |
|---|---|
| `qwen3_tts_basic.ipynb` | Preset voices, style instructions, batch generation |
| `qwen3_tts_clone.ipynb` | Upload your audio, clone your voice, batch segments |

### Quick Start

1. Open a notebook in Google Colab
2. **Runtime → Change runtime type → GPU (T4)**
3. Run cells top to bottom
4. Edit the text/speaker/audio variables in the cells marked with `--- Edit ---`

---

## Voice Cloning Workflow

1. Record 3-20 seconds of clean speech (mono, ≥24 kHz, no background noise)
2. Upload to the clone notebook
3. Type the transcription of your clip into `ref_text` — the model uses both audio and text together for alignment
4. Generate. Download. Done.

No voice sample ever hits an external server.

---

## Supported Languages

Chinese, English, Japanese, Korean, German, French, Russian, Portuguese, Spanish, Italian

---

## Credits

- **Qwen3-TTS** by [Qwen / Alibaba Cloud](https://github.com/QwenLM/Qwen3-TTS) — Apache 2.0
- **Notebooks** by [artcore-c](https://github.com/artcore-c) — Apache 2.0

---

## License

**Apache 2.0** — both the upstream model and this repo. Commercial use permitted.

---

## YouTube

Tutorial coming soon — [artcore-c](https://youtube.com/@artcore-c)