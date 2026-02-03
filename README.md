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

Qwen3-TTS uses a discrete speech-token language-model architecture and supports a dual-track hybrid streaming mode for low-latency synthesis. The process differs depending on whether you're using a preset voice or cloning:

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

| Model                           | Size    | Purpose                              |
| ------------------------------- | ------- | ------------------------------------ |
| Qwen3-TTS-12Hz-0.6B-CustomVoice | 2.52 GB | 9 preset voices with style control   |
| Qwen3-TTS-12Hz-0.6B-Base        | 2.52 GB | Voice cloning from your audio        |
| Qwen3-TTS-12Hz-1.7B-CustomVoice | 4.54 GB | Preset voices, higher quality        |
| Qwen3-TTS-12Hz-1.7B-Base        | 4.54 GB | Voice cloning, higher quality        |
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

#### See **Notes:** section below for Hardware Recommendations

___

## 🎬 Video Guide

[![AI Voice Clone with Colab + Qwen3-TTS (Free)](https://img.youtube.com/vi/CgDs8WL5YSE/maxresdefault.jpg)](https://youtu.be/CgDs8WL5YSE)

This repository was created as a companion to the YouTube video covering:
- **Qwen3-TTS** setup with Google Colab

___

## 🚀 Quick Start

1. Open Google Colab:  
   [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)
2. Enable GPU: **Runtime → Change runtime type → GPU (T4)**
3. Follow the **Notebook Build Guide** below and run the cells in order  
   *(first run will download model weights)*
4. Upload a short reference audio clip (3–20 seconds) for voice cloning
5. Enter the transcription of your clip and the text you want generated
6. Generate and download your cloned voice!

---

## Notebook Build Guide (Google Colab)

This project does not provide a prebuilt Colab notebook.

Instead, we document the exact steps and cell order used to build a working notebook so readers can reproduce, adapt, and extend the workflow themselves. This avoids breakage from Colab environment changes and keeps the process transparent.

Create a new blank notebook in Google Colab and follow the steps below.

---

### Cell 1 — Enable GPU Runtime

In Google Colab:
- Runtime → Change runtime type
- Set Hardware accelerator to GPU (T4)

Then verify CUDA is available:

```python
import torch
print("CUDA available:", torch.cuda.is_available())
print("GPU:", torch.cuda.get_device_name(0) if torch.cuda.is_available() else "None")
```

### Cell 2 — Install System Dependencies

These are required for audio loading, conversion, and saving:

```python
!apt-get update -qq
!apt-get install -y -qq ffmpeg sox libsndfile1
```

### Cell 3 — Install Qwen3-TTS

Install the official Python package. Model weights are downloaded automatically on first use.

```python
!pip install -U qwen-tts
```

###Cell 4 — Load the Model (Preset Voices)

Start with the 0.6B CustomVoice model for faster iteration. You can switch to 1.7B later for higher quality.

```python
import torch
import soundfile as sf
from qwen_tts import Qwen3TTSModel

model = Qwen3TTSModel.from_pretrained(
    "Qwen/Qwen3-TTS-12Hz-0.6B-CustomVoice",
    device_map="cuda:0",
    dtype=torch.float16,
)

print("Model loaded")
print("Supported speakers:", model.get_supported_speakers())
print("Supported languages:", model.get_supported_languages())
```

### Cell 5 — Generate Speech (Single Segment)

Edit the text and speaker as desired.

```python
from IPython.display import Audio, display

text = "This is a test of Qwen3-TTS running on Google Colab."
speaker = model.get_supported_speakers()[0]

wavs, sr = model.generate_custom_voice(
    text=text,
    language="English",
    speaker=speaker,
)

output_path = "qwen3_tts_test.wav"
sf.write(output_path, wavs[0], sr)

display(Audio(output_path))
print(f"Saved: {output_path}")
```

### Cell 6 — Batch Generation (Recommended for Long Narration)

Generate long scripts in segments for better control and stability.

```python
segments = [
    "This is the first segment of narration.",
    "This is the second segment, generated separately.",
    "This approach works well for long-form content."
]

for i, segment in enumerate(segments):
    wavs, sr = model.generate_custom_voice(
        text=segment,
        language="English",
        speaker=speaker,
    )
    path = f"segment_{i:02d}.wav"
    sf.write(path, wavs[0], sr)
    print(f"Saved: {path}")
```

### Cell 7 — Download Generated Audio

```python
from google.colab import files
files.download("qwen3_tts_test.wav")
```
(Repeat for any batch segments you want to download.)

### Switching to Higher Quality (Optional)

Once the pipeline works, switch to the 1.7B models by changing the model name:

```python
"Qwen/Qwen3-TTS-12Hz-1.7B-CustomVoice"
```

Generation will be slower but more natural and closer to real speech.

---

## Voice Cloning Workflow

1. Record 3–20 seconds of clean speech (see audio prep above)
2. Upload to the clone notebook
3. Type the transcription of your clip into `ref_text` — the model uses both audio and text together for phonetic alignment
4. Generate. Download. Done.

No audio ever hits an external server.

---

## Best Results

- **GPU recommended:** Runs best on an NVIDIA GPU (Colab T4 works well). CPU is not practical for voice cloning.  ￼
- **Preset voices:** Preset speakers are strongest in their native accents/languages—try a few for best English results.  ￼
- **Long narration:** Generate long scripts in segments (e.g., 30–90 seconds) for smoother iteration.

> **Note:** While our development platform is Apple Silicon, (MPS) is experimental; this guide focuses on Colab for consistency.

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
