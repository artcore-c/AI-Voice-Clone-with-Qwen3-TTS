# AI-Voice-Clone-with-Qwen3-TTS
**Voice cloning and text-to-speech via Google Colab using Qwen3-TTS**

Preset voices and voice cloning from your own audio. Runs entirely on Colab's GPU — model weights pull from HuggingFace, inference stays local. No external accounts or API keys required.

Built around [Qwen3-TTS](https://github.com/QwenLM/Qwen3-TTS) by Alibaba/Qwen — **Apache 2.0**.

---

## Features

- **Voice Cloning** — clone from as little as 3 seconds of reference audio
- **9 Preset Voices** — multilingual, with instruction-based style and emotion control
- **Voice Design** — generate entirely new voices from a text description (1.7B only)
- **10 Languages** — Chinese, English, Japanese, Korean, German, French, Russian, Portuguese, Spanish, Italian
- **Streaming capable** — 97ms end-to-end synthesis latency
- **No watermarks** — clean audio output

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

All fit in a Colab T4's 16 GB VRAM. Start with 0.6B, step up to 1.7B for better quality.

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
4. Edit the text/speaker/audio variables in the cells marked `--- Edit ---`

---

## Voice Cloning Workflow

1. Record 3-20 seconds of clean speech (mono, ≥24 kHz, no background noise)
2. Upload to the clone notebook
3. Type the transcription of your clip into `ref_text` — the model uses both audio and text together for alignment
4. Generate. Download. Done.

No audio ever hits an external server.

---

## Credits

- **Qwen3-TTS** by [Qwen / Alibaba Cloud](https://github.com/QwenLM/Qwen3-TTS) — Apache 2.0
- **Notebooks** by [artcore-c](https://github.com/artcore-c) — Apache 2.0

---

## License

**Apache 2.0**