# AI Capabilities

> [!WARNING]
> This guide is a work in progress, the information might be incomplete or plain wrong.

## Intro
Strix Halo can be a capable local LLM inferencing platform. With up to 128GiB of shared system memory (LPDDR5x-8000 on a 256-bit bus), it has a theoretical limit of 256GiB/s, double most PC desktop and APU platforms.

That being said, it's important to put this in context. 256GiB/s is still much lower than most mid-range dGPUs (eg, as a point of reference, a 3060 Ti has 448 GiB/s). Also, the Strix Halo GPU is an RDNA3.5 architecture (gfx1151), which for AI is pretty sub-optimal architecturally. It's ROCm support is still also far under-tuned vs other RDNA3 platforms like the 7900 series (gfx1100). For compute and memory bandwidth, you can think of the Strix Halo GPU like a [Radeon RX 7600 XT](https://www.techpowerup.com/gpu-specs/radeon-rx-7600-xt.c4190), but with up to 128GiB of VRAM.

Due to limited memory-bandwidth and compute, unless you're very patient, for real-time inferencing, Strix Halo is best for quantized versions of large Mixture-of-Expert (MoE) LLMs that have fewer activations or for having multiple models loaded or models loaded while doing other (non-GPU) tasks.

The software support is also another issue. Strix Halo's Vulkan works well on Windows and Linux (Mesa RADV and AMDVLK), but its ROCm support is still immature and incomplete. If you are doing more than running common desktop inferencing software (llama.cpp, etc), then you will want to do some careful research. 

### GPU Compute
For the 40CU Radeon 8060S at a max clock of 2.9GHz, the 395 Strix Halo should have a peak of 59.4 FP16/BF16 TFLOPS:
```
512 ops/clock/CU * 40 CU * 2.9e9 clock / 1e12 = 59.392 FP16 TFLOPS
```

Note that this theoretical max involves perfect utilization of the [dual issue capabilities of the WGPs](https://chipsandcheese.com/p/microbenchmarking-amds-rdna-3-graphics-architecture) which can be [hard in practice](https://cprimozic.net/notes/posts/machine-learning-benchmarks-on-the-7900-xtx/#tinygrad-rdna3-matrix-multiplication-benchmark). The best way to achieve efficient AI acceleration on RDNA3 is [via WMMA](https://gpuopen.com/learn/wmma_on_rdna3/).

| Type       | RDNA3              | RDNA4/sparse           |
|------------|--------------------|------------------------|
| FP16/BF16  | 512 ops/cycle      | 1024/2048 ops/cycle    |
| FP8        | N/A                | 2048/4096 ops/cycle    |
| INT8       | 512 ops/cycle      | 2048/4096 ops/cycle    |
| INT4       | 1024 ops/cycle     | 4096/8192 ops/cycle    |

In practice, as of 2025-08, Strix Halo's performance is substantially lower. It may be worth tracking these issues:
- https://github.com/ROCm/ROCm/issues/4748
- https://github.com/ROCm/ROCm/issues/4499

## LLMs
The recommended way to run LLMs is with [llama.cpp](https://github.com/ggml-org/llama.cpp) or one of the apps that leverage it like [LM Studio](https://lmstudio.ai/) with the Vulkan backend.

AMD has also been sponsoring the rapidly developing [Lemonade Server](https://lemonade-server.ai/) - an easy-to-install, all-in-one package that leverages the latest builds of software (like llama.cpp) and even provides NPU/hybrid inferencing on Windows. If you're new or unsure on what to run, you might wan to check that out first.

One of our community members, kyuz0, also maintains a [AMD Strix Halo Llama.cpp Toolboxes](https://github.com/kyuz0/amd-strix-halo-toolboxes) which has Docker builds of all llama.cpp backends.

Performance has been improving and several of our community members have been running extensive LLM inferencing benchmarks on many models:
- https://kyuz0.github.io/amd-strix-halo-toolboxes/ - an interactive viewer of standard pp512/tg128 results
- https://github.com/lhl/strix-halo-testing/tree/main/llm-bench - graphs and sweeps of pp/tg from 1-4096 for a variety of architectures

TODO: add a list of some models that work well with pp512/tg128, memory usage, model architecture, weight sizes?


Some test results (KoboldCPP 1.96.2, Vulkan, full GPU offloading, [example config file](./gemma-3-27b.kcpps)) **on Windows**:

| Model                 | Quantization | Prompt Processing | Generation Speed |
| --------------------- | ------------ | ----------------- | ---------------- |
| Llama 3.3 70B         | Q4_K_M       | 50.9 t/s          | 4.2 t/s          |
| Gemma 3 27B           | Q5_K_M       | 94.4 t/s          | 6.5 t/s          | 
| Qwen3 30B A3B         | Q5_K_M       | **284.1** t/s     | **30.0** t/s     | 
| GLM 4 9B              | Q5_K_M       | 275.0 t/s         | 15.9 t/s         |

And **on Linux**:

| Model                 | Quantization | Prompt Processing | Generation Speed |
| --------------------- | ------------ | ----------------- | ---------------- |
| Llama 3.3 70B         | Q4_K_M       | 50.3 t/s          | 4.4 t/s          |
| Gemma 3 27B           | Q5_K_M       | 127.0 t/s         | 7.2 t/s          | 
| Qwen3 30B A3B         | Q5_K_M       | 263.0 t/s         | **36.0** t/s     | 
| GLM 4 9B              | Q5_K_M       | **316.6** t/s     | 18.9 t/s         |

All tests were run 3 times and the best result was picked. Generation speed is pretty stable, prompt processing speed fluctuates a bit (5-10%).

### Additional Resources
 - Deep dive into LLM usage on Strix Halo: https://llm-tracker.info/_TOORG/Strix-Halo
 - Newbie Linux inference guide: https://github.com/renaudrenaud/local_inference
 - Ready to use Docker containers: https://github.com/kyuz0/amd-strix-halo-toolboxes

## Image/Video Generation
Didn't play with it too much yet, but looks like here the memory bandwidth and GPU performance limitations strike the most. With SDXL you can generate an image every 4-5 seconds, but going to something like Flux will lead to wait times of several minutes.

### Relevant Pages
 - [[Guides/110GB-of-VRAM]]
