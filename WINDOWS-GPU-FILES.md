# Windows GPU files for llamafile 0.10.7

This is the handoff list for building Windows GPU backends on a Windows
machine, then copying the finished DLLs to Linux for packaging into the
universal llamafile executable.

Build the DLLs from the same llamafile/llama.cpp source and patch state as the
Linux backends: llama.cpp v0.9.0, commit `cd959bae99ff4bed22bef2604f45241c09bbb8c8`.
Do not mix DLLs from an older llamafile or llama.cpp build.

## Files to copy from Windows

Copy the DLLs that were built for x64 Windows into a directory on Linux, for
example `~/code/unofficial-llamafile/windows-gpu/`.

| File | Needed for | Windows build command |
| --- | --- | --- |
| `ggml-cuda.dll` | NVIDIA CUDA | `llamafile\cuda_parallel.bat --clean --minimize-size` |
| `ggml-vulkan.dll` | Vulkan GPUs, including NVIDIA and AMD | `llamafile\vulkan.bat --clean` |
| `ggml-rocm.dll` | AMD ROCm (optional; separate from Vulkan) | `llamafile\rocm_parallel.bat --clean` |

The current Linux `llamafile-0.10.7` already contains `ggml-cuda.so` and
`ggml-vulkan.so`. The first two DLLs add Windows support for those backends.
Include `ggml-rocm.dll` only if AMD ROCm support is wanted; the current
`llamafile/release.sh` does not bundle ROCm, so it must be added explicitly
when packaging.

Also copy a small build record alongside the DLLs:

- SHA-256 checksum for every DLL (for example, `sha256sum *.dll`)
- Build log for each DLL
- CUDA, Vulkan SDK, or HIP SDK version used
- Confirmation that the build used this v0.9.0 source and patch set

Do not copy `.obj`, `.lib`, `.pdb`, compiler binaries, or SDK directories for
Linux packaging. They are not needed to embed the DLLs.

## Windows build machine requirements

The repository's documented Windows build setup uses:

- Windows 11 x64
- Visual Studio 2022 Build Tools with the C++ toolchain
- MSYS2 with `git`, `patch`, `unzip`, `wget`, and `make` for `make setup`
- CUDA Toolkit 12.9.1 for `ggml-cuda.dll`
- Vulkan SDK 1.4.341.1 for `ggml-vulkan.dll`
- AMD HIP SDK 7.1.1 for `ggml-rocm.dll` (optional)

Run `make setup` in the matching source checkout before running the backend
scripts. The CUDA script defaults to TinyBLAS; avoid `--cublas` unless the
Windows target will also have the matching cuBLAS runtime installed.

## Can these files be copied to Linux?

Yes. The finished Windows DLLs can be copied to Linux and embedded there with
`zipalign`; Linux does not need the Windows compiler or SDKs to package them.
The DLLs are stored as Windows-specific entries and are only loaded when the
universal executable runs on Windows. The current scripts do not build Windows
DLLs on Linux, so compile them on Windows first.

On the Windows target, the relevant GPU drivers/runtime still need to be
installed. The CUDA TinyBLAS DLL depends on the NVIDIA driver-provided
`nvcuda.dll`; Vulkan depends on `vulkan-1.dll`. ROCm uses AMD HIP runtime DLLs
such as `amdhip64.dll`, `hipblas.dll`, and `rocblas.dll`. These are runtime
dependencies, not backend DLLs to embed in the llamafile.

## Package on Linux

Copy the Windows DLLs into `windows-gpu/`, then rebuild the full executable
from the thin binary so the Linux `.so` files and Windows `.dll` files are all
present exactly once:

```sh
cd ~/code/unofficial-llamafile
mkdir -p windows-gpu
cp llamafile-0.10.7-thin llamafile-0.10.7
~/code/llamafile/o//third_party/zipalign/zipalign -j0 llamafile-0.10.7 \
  /tmp/opencode/gpulibs/ggml-cuda.so \
  /tmp/opencode/gpulibs/ggml-vulkan.so \
  windows-gpu/ggml-cuda.dll \
  windows-gpu/ggml-vulkan.dll
```

Append `windows-gpu/ggml-rocm.dll` to the command if ROCm support is included.
Then regenerate the full executable's checksum. This packages the DLLs but
does not validate them; run a Windows GPU smoke test on a Windows machine.
