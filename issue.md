I have not been able to get `llamafile --gpu amd` working with an `AMD Radeon RX 6900 XT` on `linux`. The relevant line of the log seems to be:

```
llamafile: /usr/src/debug/hip-runtime-amd/clr-rocm-5.7.1/rocclr/os/os_posix.cpp:310: static void amd::Os::currentStackInfo(unsigned char**, size_t*): Assertion `Os::currentStackPtr() >= *base - *size && Os::currentStackPtr() < *base && "just checking"' failed.
```

- [`full log`](https://github.com/johnshaughnessy/llamafile-mc/blob/memory-cache/log/llama2.log)

I found similar bug reports in other projects, so I suspect this is **NOT** a `llamafile` bug:

- [Issue from `JuliaGPU`](https://github.com/JuliaGPU/AMDGPU.jl/issues/549)
- [Issue from `ROCm-OpenCL-Runtime`](https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime/issues/158)
- [Issue from `gentoo` forums](https://bugs.gentoo.org/895286)
- [Issue from `ROCm`](https://github.com/ROCm/ROCm/issues/1376)
- [An older issue from `ROCm`](https://github.com/ROCm/ROCm/issues/887)

Instead, it seems that `ROCm` is not supported on my graphics card:

```
  Name:                    gfx1030
  Marketing Name:          AMD Radeon RX 6900 XT
```

- [`full rocminfo`](https://github.com/johnshaughnessy/llamafile-mc/blob/memory-cache/log/rocminfo.log)

Searching the `AMD` docs, I found:

- [`ROCm™ Software 6.0.0` does **not** list `6900` series cards in their **linux** support matrix.](https://rocm.docs.amd.com/projects/install-on-linux/en/latest/reference/system-requirements.html)
- [`ROCm™ Software 6.0.0` **does** list `6900` series cards in their **windows** support matrix.](https://rocm.docs.amd.com/projects/install-on-windows/en/latest/reference/system-requirements.html)

I tried messing with the environment variable `HSA_OVERRIDE_GFX_VERSION` because I had seen that in some other issue reports, but did not have any luck.

In case it's helpful, I kept a [log](https://github.com/johnshaughnessy/llamafile-mc/blob/memory-cache/MemoryCache.md) the steps I took when setting things up.

To summarize, [I installed ROCm for Arch Linux](https://github.com/rocm-arch/rocm-arch), but it seems that my graphics card (`Radeon RX 6900 XT`) is not supported by `ROCm`, so I cannot use the `--gpu amd` flag with `llamafile`.

If this is correct, then it is not a bug with `llamafile`. Still, I wanted to file this issue:

- to ask if this seems correct,
- to ask if there's anything else worth trying before giving up on my card,
- to save anyone else the trouble of figuring this out,
- to offer to make a note in the `Gotchas` section of the `README.md`.
