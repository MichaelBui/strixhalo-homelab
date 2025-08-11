# AI Capabilities Overview

> [!WARNING]
> This guide is a work in progress. Also, there has been ongoing progress with software quality and performance so please be aware that some of this information may become rapidly out of date.

## Intro
Strix Halo can be a capable local LLM inferencing platform. With up to 128GiB of shared system memory (LPDDR5x-8000 on a 256-bit bus) and a theoretical bandwidth limit of 256GiB/s (double most PC desktop and APU platforms) it is well suited for running quantized medium sized (~30B) dense models as well as large 100B+ parameter sparse Mixture-of-Experts (MoE) models.

There have been some wild claims made on Strix Halo's AI capabilities, so it's important to put this in context. 256GiB/s of MBW is still much lower than most mid-range dGPUs. As a point of reference, a 3060 Ti has 448 GiB/s of MBW. Also, the Strix Halo GPU uses an RDNA3.5 architecture (gfx1151), which for AI is sub-optimal architecturally. For compute and memory bandwidth, you can think of the Strix Halo GPU like a [Radeon RX 7600 XT](https://www.techpowerup.com/gpu-specs/radeon-rx-7600-xt.c4190), but with up to 100GiB+ of VRAM.

Vulkan works well for Strix Halo on Windows and Linux (both Mesa RADV and AMDVLK), but as of August 2025, its ROCm support is still immature and incomplete, and you may find running other AI/ML tasks (training, image/video generation) slow or impossible. 

If you are doing more than running common desktop inferencing software (llama.cpp, etc), then you will want to do some careful research.

### GPU Compute
For the 40CU Radeon 8060S at a max clock of 2.9GHz, the 395 Strix Halo should have a peak of 59.4 FP16/BF16 TFLOPS:
```
512 ops/clock/CU * 40 CU * 2.9e9 clock / 1e12 = 59.392 FP16 TFLOPS
```

For more information on how RDNA3 does its tensor math, see this article on  [WMMA on RDNA3](https://gpuopen.com/learn/wmma_on_rdna3/).

| Type       | RDNA3              | RDNA4/sparse           |
|------------|--------------------|------------------------|
| FP16/BF16  | 512 ops/cycle      | 1024/2048 ops/cycle    |
| FP8        | N/A                | 2048/4096 ops/cycle    |
| INT8       | 512 ops/cycle      | 2048/4096 ops/cycle    |
| INT4       | 1024 ops/cycle     | 4096/8192 ops/cycle    |

In practice, as of 2025-08, Strix Halo's tested performance is substantially lower what should be theoretically achievable. You can track performance progress via these issues:
- https://github.com/ROCm/ROCm/issues/4748
- https://github.com/ROCm/ROCm/issues/4499

### Comparison to other available options
There are not that many options for those looking for 96GB+ of VRAM, annd among its competition, Strix Halo is well priced:

| Spec                      | AMD Ryzen AI Max Plus 395     | Apple Mac Studio M4 Max         | NVIDIA DGX Spark                           | NVIDIA RTX PRO 6000 |
|---------------------------|------------------------------|----------------------------------|---------------------------------------------|---------------------|
| **Release Date**          | Spring 2025                   | March 12, 2025                   | August 2025                                 | May 2025           |
| **Price**                 | $2,000                        | $3,499                           | $3,000+                                     | $8,200              |
| **Power (Max W)**         | 120                           | 300+                             | ??                                          | 600                 |
| **CPU**                   | 16x Zen5 5.1 GHz           | 16x M4 4.5 GHz                 | 10x Arm Cortex-X925, 10x Arm Cortex-A725  |                     |
| **GPU**                   | RDNA 3.5                      | M4                               | Blackwell                                   | Blackwell           |
| **GPU Cores**             | 40                            | 40                               | 192                                         | 752                 |
| **GPU Clock**             | 2.9 GHz                       | 1.8 GHz                          | 2.5 GHz                                     | 2.6 GHz             |
| **Memory**                | 128GB LPDDR5X-8000             | 128GB LPDDR5X-8533                | 128GB LPDDR5X-8533                        | 96GB GDDR7 ECC      |
| **Memory Bandwidth**      | 256 GB/s                       | 546 GB/s                          | 273 GB/s                                    | 1792 GB/s           |
| **FP16 TFLOPS**           | 59.39                         | 34.08                            | 62.5                                        | 251.90              |
| **FP8 TFLOPS**            |                               |                                  | 125                                         | 503.79              |
| **INT8 TOPS (GPU)**       | 59.39 (same as FP16)           | 34.08 (same as FP16)              | 250                                         | 1007.58             |
| **INT4 TOPS (GPU)**       | 118.78                        |                                  | 500                                         | 2015.16             |
| **INT8 TOPS (NPU)**       | 50                            | 38                               |                                             |                     |
| **Storage**               | 2-3x NVMe                     | 512GB (non-upgradable) + 3x TB5 (120 Gbps) | NVMe                                       |                     |

An NVIDIA RTX PRO 6000 (Blackwell) dGPU is included as a point of comparison - if you are willing to pay (significantly) more, you can get much better performance, but of course there are many other options if you are open to a bigger form factor, power envelope, or price point (usually all three).

## Setup

### Memory Limits
For the Strix Halo GPU, memory is either GART, which is a fixed reserved aperture set exclusively in the BIOS, and GTT, which is a dynamically allocable memory amount of memory. In Windows, this should be automatic (but is limited to 96GB). In Linux, it can be set via boot configuration up to the point of system instability.

As long are your software supports using GTT, for AI purposes, you are probably best off setting GART to the minimum (eg, 512MB) and then allocating automatically via GTT. In Linux, you can create a conf in your `/etc/modprobe.d/` (like `/etc/modprobe.d/amdgpu_llm_optimized.conf`):

```
# Use up to 120GB GTT with 60GB pre-reserved
options amdgpu gttsize=120000
options ttm pages_limit=31457280
options ttm page_pool_size=15728640
```

`amdgpu.gttsize` is an [officially deprecated](https://www.mail-archive.com/amd-gfx@lists.freedesktop.org/msg117333.html) parameter that may be referenced by some software, so it's best to still set it to match, but GTT allocation in Linux is now actually handled by the Translation Table Maps (TTM) memory management subsystem.
- `pages_limit` sets the maximum number or 4KiB pages that can be used for GPU memory.
- `page_pool_size` pre-caches/allocates the memory for usage by the GPU. (This will not be available for your system). In theory you could set this to 0, but if you are looking for the maximum performance (minimizing fragmentation), then you can set it to match the `pages_limit` size.

You can increase the limits as high as you want, but you'll want to make sure you reserve enough memory for your OS/system.

If you are setting `page_pool_size` lower than the `pages_limit` you may want to try increasing, eg `amdgpu.vm_fragment_size=8` (4=64K default, 9=2M) to allocate in bigger chunks.

### ROCm
The latest release version of ROCm (6.4.3 as of this writing) has preliminary rocBLAS support for Strix Halo gfx1151, but it is much slower and buggier (and doesn't have hipBLASlt kernels). For the most up-to-date support, currently it's recommended to use the latest gfx1151 [TheRock/ROCm "nightly" release](https://github.com/ROCm/TheRock/blob/main/RELEASES.md). These can be found at [https://therock-nightly-tarball.s3.amazonaws.com/](https://therock-nightly-tarball.s3.amazonaws.com/) (find the filename) or you can use the helper scripts described in the [Releases page]((https://github.com/ROCm/TheRock/blob/main/RELEASES.md)).

### Performance Tips
- If you are not using VFIO or any type of GPU passthrough, you should set `amd_iommu=off` in your kernel options for ~6% faster memory reads (actuall impact on llama.cpp tg performance tends to be smaller, about <2%. Note that when tested, `iommu=pt` does not give any speed benefit.
- You can improve further improve performance with [tuned](https://tuned-project.org/) and switching to the `accelerator-performance` profile:
  - Raises raw Vulkan memory bandwidth performance by about 3%
  - Seems to have minimal effect on `tg` but improves llama.cpp `pp512` performance by 5-8% (!!!)
```
paru -S tuned
sudo systemctl enable --now tuned
tuned-adm list
# - accelerator-performance     - Throughput performance based tuning with disabled higher latency STOP states
sudo tuned-adm profile accelerator-performance
tuned-adm active
# Current active profile: accelerator-performance
```
- If you are loading large models (more than half your RAM) on llama.cpp , yous hould be sure to disable mmap as it's marginally bad for Vulkan model loading performance, but catastrophically bad for ROCm (large models can take hours to load).  You should use the appropriate command option (differs based on specific llama.cpp binary) to disable mmap in general for Strix Halo.
- Again for llama.cpp, be sure to use `–ngl 99` (or 999 if >99 layers) to load all layers into the GPU-addressable space. While its technically "shared" memory, in practice, CPU memory bandwidth is only half of the GPU mbw due to Strix Halo's memory architecture (I’ve heard due to GMI link design, but the practical part is you’re going to have much lower CPU vs GPU mbw).


## LLMs
The recommended way to run LLMs is with [llama.cpp](https://github.com/ggml-org/llama.cpp) or one of the apps that leverage it like [LM Studio](https://lmstudio.ai/) with the Vulkan backend.

AMD has also been sponsoring the rapidly developing [Lemonade Server](https://lemonade-server.ai/) - an easy-to-install, all-in-one package that leverages the latest builds of software (like llama.cpp) and even provides NPU/hybrid inferencing on Windows. If you're new or unsure on what to run, you might want to check that out first.

One of our community members, kyuz0, also maintains a [AMD Strix Halo Llama.cpp Toolboxes](https://github.com/kyuz0/amd-strix-halo-toolboxes) which has Docker builds of all llama.cpp backends.

Performance has been improving and several of our community members have been running extensive LLM inferencing benchmarks on many models:
- https://kyuz0.github.io/amd-strix-halo-toolboxes/ - an interactive viewer of standard pp512/tg128 results
- https://github.com/lhl/strix-halo-testing/tree/main/llm-bench - graphs and sweeps of pp/tg from 1-4096 for a variety of architectures

### llama.cpp
The easiest way to get llama.cpp to work is with the Vulkan backend. This reliable and relatively performant on both Windows and Linux and you can either [build it yourself](https://github.com/ggml-org/llama.cpp/blob/master/docs/build.md#vulkan) or simply [download the latest release](https://github.com/ggml-org/llama.cpp/releases) for your OS.
- For Vulkan on Linux, you should install both the AMDVLK and Mesa RADV drivers. When both are installed AMDVLK will be the default Vulkan driver, which is generally fine as it's `pp` can be up to 2X faster than Mesa RADV. You can set `AMD_VULKAN_ICD=RADV` to switch to the RADV to compare. The latter tends to have slightly higher `tg` speed, hold up better in long context, and be slightly more reliable, so you should test both and see which works better for you.

If you want to use the ROCm backend (the rocWMMA FA implementation in particular can offer huge (eg 2X) `pp` performance advantages vs Vulkan), the easiest way is to use kyuz0's [AMD Strix Halo Llama.cpp Toolboxes](https://github.com/kyuz0/amd-strix-halo-toolboxes) container builds or Lemonade's [llamacpp-rocm](https://github.com/lemonade-sdk/llamacpp-rocm) builds. If you're looking to build your own, please refer to: [[Guides/AI-Capabilities/llamacpp-with-ROCm]]

If you are using the llama.cpp ROCm backend, you may want to also try to use the hipBLASlt kernel with the `ROCBLAS_USE_HIPBLASLT=1` environment variable as it is sometimes faster than the default rocBLAS kernels.


TODO: add a list of some models that work well with pp512/tg128, memory usage, model architecture, weight sizes?


### Additional Resources
 - Deep dive into LLM usage on Strix Halo: https://llm-tracker.info/_TOORG/Strix-Halo
 - Newbie Linux inference guide: https://github.com/renaudrenaud/local_inference
 - Ready to use Docker containers: https://github.com/kyuz0/amd-strix-halo-toolboxes

## Image/Video Generation
For Windows you can give AMUSE a try. It's probably the easiest way to get started quickly: https://www.amuse-ai.com/

Here are some instructions for getting ComfyUI up and running on Windows: https://www.reddit.com/r/StableDiffusion/comments/1lmt44b/running_rocmaccelerated_comfyui_on_strix_halo_rx/

Note, while TheRock has [PyTorch nightlies](https://github.com/ROCm/TheRock/blob/main/RELEASES.md#installing-pytorch-python-packages) available for Strix Halo gfx1151, they *do not* currently have AOTriton or FA and may not run very well.
