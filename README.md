# AI-Voice-Clone-with-Qwen3-TTS

Free voice cloning for creators using **Qwen3-TTS** on Google Colab.  
Clone your voice from as little as **3–20 seconds of audio** for consistent narration and voiceovers.  
Complete guide to build your own notebook. **Apache 2.0 licensed.**

---
## Overview

**Qwen3-TTS** is a multilingual text-to-speech system with high-quality **zero-shot voice cloning** capabilities.  
It uses a discrete speech-token language-model architecture combined with a flow-matching decoder to generate natural speech that preserves speaker identity, cadence, and tone from a short reference clip.

Unlike many creator-facing TTS systems, Qwen3-TTS is fully open-source (Apache 2.0), produces **unwatermarked audio**, and does not require external APIs or paid inference services.

This repository focuses specifically on **voice cloning workflows for creators**, even though Qwen3-TTS also supports preset voices and text-designed speakers.

> Note: [Qwen3-TTS](https://github.com/QwenLM/Qwen3-TTS) is a recent release, so this project is perhaps a bit more experimental than our prior work and the foundation we built with [AI-Voice-Clone-with-Coqui-XTTS-v2](https://github.com/artcore-c/AI-Voice-Clone-with-Coqui-XTTS-v2/), and serves as an extension of our continued exploration with Colab-based voice cloning notebooks.

---
## How It Works

### Voice Cloning Process

Qwen3-TTS uses a discrete speech-token language-model architecture and supports a dual-track hybrid streaming mode for low-latency synthesis. The process differs depending on whether you're using a preset voice or cloning:

**Preset voices** use the CustomVoice model. You provide text and a speaker name, and optionally a style instruction (e.g. "speak with calm authority"). The model generates speech matching that speaker's characteristics and your style direction.

**Voice cloning** uses the Base model. You provide a reference audio clip and its transcription. The model extracts acoustic features — pitch, tone, cadence, speaking style — and encodes them into a speaker embedding. That embedding is then used to synthesize new text in your voice. The transcription (`ref_text`) gives the model a phonetic alignment target, which improves accuracy over audio alone.

- **Audio Analysis** — The model analyzes your reference audio to extract acoustic features such as pitch, tone, cadence, and speaking style.
- **Speaker Encoding** — These features are encoded into a speaker embedding that represents your vocal identity.
- **Text-to-Speech Generation** — Given new text, the model synthesizes speech that matches the learned speaker characteristics.
- **Waveform Synthesis** — The generated speech tokens are decoded into a high-quality waveform suitable for narration and post-production.

Providing the transcription of your reference clip (`ref_text`) improves phonetic alignment and cloning accuracy, especially with very short samples.

**Voice design** (1.7B only) generates a brand new speaker from a text description. No reference audio needed — just describe the voice characteristics you want.

---
## Technical Stack

- **Model:** Qwen3-TTS (0.6B and 1.7B variants)
- **Framework:** PyTorch
- **Inference:** Runs on Google Colab’s free T4 GPU (16 GB VRAM)
- **Sample Rate:** 24 kHz output
- **Languages:** Chinese, English, Japanese, Korean, German, French, Russian, Portuguese, Spanish, Italian

---
## Why Google Colab?

Qwen3-TTS benefits from GPU-accelerated inference, which supports practical voice cloning workflows and faster iteration.

Google Colab provides free access to GPU-accelerated computing well-suited for running Qwen3-TTS models without local setup. A single T4 GPU is sufficient to run all Qwen3-TTS models used in this guide without any local hardware, paid cloud services, or complex setup. No Python installation needed locally. Everything runs in the notebook.

All inference runs **inside your Colab session** — no audio is uploaded to third-party APIs.

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

- Google account (for Google Colab and Google Drive)
- For voice cloning: 3–20 seconds of clean reference audio in WAV
  - Best results: clear speech, minimal background noise
  - Mix of scripted and natural speaking recommended
- Google Colab with T4 GPU runtime (available with free plan but subject to usage limits)
- No Python installation needed locally (runs in Colab)

## Prerequisites

### 🎤Audio File

- .wav or .mp3 sample audio file uploaded to your Google Drive
- 3-20 seconds in length
- 16-bit or 24-bit, 44.1kHz or 48kHz sample rate recommended

#### Converting Audio to WAV

**macOS (built-in tool):**

```zsh
# afconvert comes pre-installed on macOS
afconvert -f WAVE -d LEI16 input.m4a output.wav
```

**Mac/Linux/Windows (use ffmpeg):**

Install ffmpeg first:

```zsh
# macOS with MacPorts
sudo port install ffmpeg

# Ubuntu/Debian
sudo apt-get install ffmpeg

# Windows (with Chocolatey)
choco install ffmpeg
```

Convert audio:

```zsh
ffmpeg -i input.m4a -ar 24000 output.wav
```

Supported input formats: .m4a, .mp3, .mp4, .mov, and most audio/video formats.

#### See **Notes:** section below for Hardware Recommendations

---
## 🎬 Video Guide

[![AI Voice Clone with Colab + Qwen3-TTS (Free)](https://img.youtube.com/vi/CgDs8WL5YSE/maxresdefault.jpg)](https://youtu.be/CgDs8WL5YSE)

This repository was created as a companion to the YouTube video covering:

- **Qwen3-TTS** setup with Google Colab

---
## 🚀 Quick Start

1. Open the Colab notebook: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)
2. Enable GPU: **Runtime → Change runtime type → GPU (T4)**
3. For **Preset/Custom Voice** run cells 1 - 7 in order  
   _(first run will download model weights)_
4. For **Voice Cloning** run cells 8 - 12 in order
5. Upload a short reference audio clip (3–20 seconds) for voice cloning
6. Enter the transcription of your clip and the text you want generated
7. Generate and download your cloned voice!

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

### Cell 4 — Load the Model (Preset Voices)

Start with the 0.6B CustomVoice model for faster iteration. You can switch to 1.7B later for higher quality.

```python
import torch
import soundfile as sf
from qwen_tts import Qwen3TTSModel

model = Qwen3TTSModel.from_pretrained(
    "Qwen/Qwen3-TTS-12Hz-0.6B-CustomVoice",
    device_map="cuda:0",
    dtype=torch.bfloat16,
)

print("Model loaded")
print("Supported speakers:", model.get_supported_speakers())
print("Supported languages:", model.get_supported_languages())
```

> **Note:** You'll see two warnings after this cell runs — `flash-attn not installed` and `HF_TOKEN` not set. Both are expected on Colab's free T4 GPU and can be ignored. FlashAttention 2 is recommended by upstream but requires Ampere-class hardware (A100 or newer) — T4 doesn't support it. The model runs correctly on the manual PyTorch fallback.

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
## Voice Cloning

### Cell 8 — Mount Google Drive

Upload your reference audio to Drive first, then mount it here.

```python
from google.colab import drive
drive.mount('/content/drive')
```

### Cell 9 — Load the Base Model (Voice Cloning)
Swaps out CustomVoice for Base. If you already ran Cells 4–7, restart the runtime first to free VRAM.

```python
import torch
import soundfile as sf
from qwen_tts import Qwen3TTSModel

model = Qwen3TTSModel.from_pretrained(
    "Qwen/Qwen3-TTS-12Hz-0.6B-Base",
    device_map="cuda:0",
    dtype=torch.bfloat16,
)

print("Base model loaded — ready for voice cloning")
```

### Cell 10 — Clone Your Voice (Single Segment)
Edit the three variables: your audio path, its transcription, and the text you want generated.

```python
from IPython.display import Audio, display

# --- Edit these ---
ref_audio_path = "/content/drive/MyDrive/your_reference.wav"
ref_text = "This is exactly what was said in the reference clip."
target_text = "This is the new text you want generated in your voice."
# -----------------

wavs, sr = model.generate_voice_clone(
    text=target_text,
    language="English",
    ref_audio=ref_audio_path,
    ref_text=ref_text,
)

output_path = "cloned_voice.wav"
sf.write(output_path, wavs[0], sr)

display(Audio(output_path))
print(f"Saved: {output_path}")
```

### Cell 11 — Batch Cloning with Cached Prompt (Recommended)
For multiple segments, cache the speaker embedding first. This avoids re-extracting from your reference audio on every iteration — significant speedup for longer scripts.

```python
# Cache the speaker embedding once
prompt = model.create_voice_clone_prompt(
    ref_audio=ref_audio_path,
    ref_text=ref_text,
)

segments = [
    "This is the first segment of your narration.",
    "This is the second segment, same voice, no re-upload.",
    "Batch cloning keeps things consistent across the whole script."
]

for i, segment in enumerate(segments):
    wavs, sr = model.generate_voice_clone(
        text=segment,
        language="English",
        voice_clone_prompt=prompt,
    )
    path = f"clone_segment_{i:02d}.wav"
    sf.write(path, wavs[0], sr)
    print(f"Saved: {path}")
```

### Cell 12 — Download Cloned Audio

```python
from google.colab import files
files.download("cloned_voice.wav")
```

(Repeat for any batch segments.)

### Fallback — No Transcription Available
If you can't transcribe the reference clip, this mode still works but quality may drop:

```python
wavs, sr = model.generate_voice_clone(
    text=target_text,
    language="English",
    ref_audio=ref_audio_path,
    x_vector_only_mode=True,
)
```

---
## Best Results

- **GPU recommended:** Best results are achieved on Colab’s T4-class GPUs.
- **Preset voices:** Preset speakers are strongest in their native accents/languages—try a few for best English results. ￼
- **Long narration:** Generate long scripts in segments (e.g., 30–90 seconds) for smoother iteration.

> **Note:** While our local development platform has been upgraded to Apple Silicon, (MPS) is experimental; this guide focuses on Colab for consistency.

---
## Notes:

### Recording Equipment (Minimum Recommended)

**Recommended for best results while recording audio samples:**

- USB audio interface (_we used an Arturia MiniFuse 2_)
- Condenser or shotgun microphone (_we used an Audio-Technica AT875R_)
- Quiet recording environment

**Acceptable minimum:**

- Smartphone (_eg. iPhone 8+_) in a quiet room
- USB microphone with cardioid pattern
- Desktop/Laptop built-in mic in very quiet environment (quality will be lower)

**Background noise:**

- More important than mic quality. Record in a quiet space.

---
## PyTorch & GPU Notes

This workflow uses PyTorch for model loading and inference. When running on Google Colab, PyTorch automatically detects and uses the available GPU runtime.

No manual CUDA configuration or version pinning is required for the steps described in this guide. Model weights and dependencies are resolved automatically by the `qwen-tts` package and the Colab environment.

If a GPU runtime is unavailable, inference may fall back to CPU, but GPU-backed runtimes are recommended for practical voice cloning workflows.

---
## License

**Apache 2.0**

## Acknowledgements

This project builds upon:

- **[Qwen3-TTS](https://github.com/QwenLM/Qwen3-TTS)** - Open-source models and framework
- **[Google Colab](https://colab.research.google.com)** - Free GPU infrastructure
- **[PyTorch](https://pytorch.org)** - Deep learning framework

We're grateful to the open-source community for making voice cloning accessible to all creators.

## ⚠️ GPU Usage Limits

**Colab Free Plan Limitations:**

- In the free version of Colab notebooks can run for at most 12 hours, depending on availability and usage patterns.
- Colab Pro and Pay As You Go offer increased compute availability based on your compute unit balance.
- If unavailable, wait 12+ hours or consider Colab Pro ($9.99/month) for increased access.

## Support

Questions? Check the video tutorial or open an issue!
