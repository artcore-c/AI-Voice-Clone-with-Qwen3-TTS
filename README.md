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

## How It Works

Qwen3-TTS uses a flow-matching architecture to generate speech directly from text. The process differs depending on whether you're using a preset voice or cloning:

**Preset voices** use the CustomVoice model. You provide text and a speaker name, and optionally a style instruction (e.g. "speak with calm authority"). The model generates speech matching that speaker's characteristics and your style direction.

**Voice cloning** uses the Base model. You provide a reference audio clip and its transcription. The model extracts acoustic features — pitch, tone, cadence, speaking style — and encodes them into a speaker embedding. That embedding is then used to synthesize new text in your voice. The transcription (`ref_text`) gives the model a phonetic alignment target, which improves accuracy over audio alone.

**Voice design** (1.7B only) generates a brand new speaker from a text description. No reference audio needed — just describe the voice characteristics you want.

---

## Why Google Colab?

Running TTS inference on CPU is slow — GPU acceleration is essential for practical use. Colab's free T4 GPU (16 GB VRAM) is enough to run all Qwen3-TTS model sizes without any local hardware. No Python installation needed locally. Everything runs in the notebook.

---

## Intended Use Cases

- Consistent narration across multiple recording sessions
- Editing specific audio segments without re-recording the whole piece
- Voiceovers when recording conditions aren't ideal
- Generating placeholder audio for video editing workflows
- Exploring preset voices and style control for creative projects

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

## Requirements

- Google account (for Colab and Google Drive)
- Google Colab with T4 GPU runtime (free plan, subject to availability)
- For voice cloning: 3–20 seconds of clean reference audio in WAV format
- No local Python installation needed

---

## Preparing Your Audio

For voice cloning, clean audio matters more than length. Aim for clear speech with minimal background noise.

**Recommended recording setup:**

- USB audio interface (e.g. Arturia MiniFuse 2)
- Condenser or shotgun microphone (e.g. Audio-Technica AT875R)
- Quiet environment

**Acceptable minimum:**

- Smartphone in a quiet room
- USB microphone with cardioid pattern
- Laptop mic in a very quiet space (quality will be lower)

**Target specs:** mono, 24 kHz or higher, 16-bit or 24-bit. WAV format preferred.

### Converting Audio to WAV

**macOS (built-in):**

```bash
afconvert -f WAVE -d LEI16 input.m4a output.wav
```

**macOS / Linux / Windows (ffmpeg):**

Install ffmpeg first:

```bash
# macOS with MacPorts
sudo port install ffmpeg

# Ubuntu / Debian
sudo apt-get install ffmpeg

# Windows (Chocolatey)
choco install ffmpeg
```

Convert:

```bash
ffmpeg -i input.m4a -ar 24000 output.wav
```

Supported input formats: .m4a, .mp3, .mp4, .mov, and most audio/video containers.

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
4. Edit the text, speaker, or audio variables in the cells marked `--- Edit ---`

---

## Voice Cloning Workflow

1. Record 3–20 seconds of clean speech (see audio prep above)
2. Upload to the clone notebook
3. Type the transcription of your clip into `ref_text` — the model uses both audio and text together for phonetic alignment
4. Generate. Download. Done.

No audio ever hits an external server.

---

## ⚠️ GPU Usage Limits

Colab's free plan has session time limits (up to 12 hours depending on availability and usage patterns). If the runtime disconnects mid-generation, the notebook can be resumed — model download is cached within the session.

If free-tier GPU access is unavailable, wait 12+ hours or consider Colab Pro ($9.99/month) for increased compute availability.

---

## Credits

- **Qwen3-TTS** by [Qwen / Alibaba Cloud](https://github.com/QwenLM/Qwen3-TTS) — Apache 2.0
- **Notebooks** by [artcore-c](https://github.com/artcore-c) — Apache 2.0

---

## License

**Apache 2.0**