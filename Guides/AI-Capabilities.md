# AI Capabilities

> [!WARNING]
> This guide is a work in progress, the information might be incomplete or plain wrong.

### Intro
It's pretty much as promised - there's a lot of memory, you can run a lot of stuff with it. Still, in my opinion, it doesn't make sense to allocate more than 64G to the gpu for these purposes, because even though you'll have enough memory to put big models into, the memory bandwidth would be a huge bottleneck, making most stuff unusable for realtime applications.

One really cool thing is that you can just keep several models in the memory to call instantly when you need them and still be able to play some really demanding games at the same time.

### LLMs
Context processing is very slow due to memory speed limitations, adding more than 4k context at once is painful (if you're using models in chat mode, adding context gradually, then it's fine of course), using FlashAttention and KV cache quantization is highly recommended. BLAS batch size of 512 seems to be optimal.

MoE models work very well, hopefully we'll see some 50-70B ones in the future, they could be the real sweet spot for this hardware.

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

#### Additional Resources
 - Deep dive into LLM usage on Strix Halo: https://llm-tracker.info/_TOORG/Strix-Halo
 - Newbie Linux inference guide: https://github.com/renaudrenaud/local_inference
 - Ready to use Docker containers: https://github.com/kyuz0/amd-strix-halo-toolboxes

### Image/Video Generation
Didn't play with it too much yet, but looks like here the memory bandwidth and GPU performance limitations strike the most. With SDXL you can generate an image every 4-5 seconds, but going to something like Flux will lead to wait times of several minutes.

### Relevant Pages
 - [[Guides/110GB-of-VRAM]]
