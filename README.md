Unofficial llamafile builds, re-bundled against specific llama.cpp releases.

I'm only using this for embedding, and wanted it to work with embeddinggemma2, and want my code to automatically download it, so I've just made small test releases for myself. If it helps you feel free to also use it :)

Binaries are published as [GitHub Releases](https://github.com/MarkyXP/unofficial-llamafile/releases), one per llama.cpp version. Each binary has a matching `.sha256` file for verification (`sha256sum -c <file>.sha256`).

## llama.cpp 0.9.0

| File | Notes |
|------|-------|
| [llamafile-0.10.7-llamacpp-0.9.0](https://github.com/MarkyXP/unofficial-llamafile/releases/download/llamacpp-0.9.0/llamafile-0.10.7-llamacpp-0.9.0) | Full (CPU + bundled CUDA DSO) |
| [llamafile-0.10.7-llamacpp-0.9.0-thin](https://github.com/MarkyXP/unofficial-llamafile/releases/download/llamacpp-0.9.0/llamafile-0.10.7-llamacpp-0.9.0-thin) | CPU only |

## llama.cpp 1.0.0

| File | Notes |
|------|-------|
| [llamafile-0.10.7-llamacpp-1.0.0](https://github.com/MarkyXP/unofficial-llamafile/releases/download/llamacpp-1.0.0/llamafile-0.10.7-llamacpp-1.0.0) | Full (CPU + bundled CUDA DSO, sm_75/80/86/89/90/120f) |
| [llamafile-0.10.7-llamacpp-1.0.0-thin](https://github.com/MarkyXP/unofficial-llamafile/releases/download/llamacpp-1.0.0/llamafile-0.10.7-llamacpp-1.0.0-thin) | CPU only |
| [llamafile-tts-0.10.7-llamacpp-1.0.0](https://github.com/MarkyXP/unofficial-llamafile/releases/download/llamacpp-1.0.0/llamafile-tts-0.10.7-llamacpp-1.0.0) | TTS (text-to-speech), CPU + bundled CUDA DSO |
| [llamafile-tts-0.10.7-llamacpp-1.0.0-thin](https://github.com/MarkyXP/unofficial-llamafile/releases/download/llamacpp-1.0.0/llamafile-tts-0.10.7-llamacpp-1.0.0-thin) | TTS (text-to-speech), CPU only |
