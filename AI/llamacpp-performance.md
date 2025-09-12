# llama.cpp Performance

There are some tests that have been run on Strix Halo that can be useful to get a ballpark idea of performance:
- [lhl Strix Halo LLM Benchmark Results](https://github.com/lhl/strix-halo-testing/tree/main/llm-bench) - scripted tests from 2025-08 on a number of models, sweeping pp and tg from 1-4096 across a number of backends
- [kyuz0 AMD Ryzen AI MAX+ 395 "Strix Halo" — Llama.cpp Backend Performance Comparison](https://kyuz0.github.io/amd-strix-halo-toolboxes/) - automated pp512/tg128 tests across a number of backends; may be more up-to-date

## llama-bench Basics

It's important to note that both AMD, ROCm and Vulkan drivers/libs and llama.cpp are constantly being updated and that different models (and different context lengths!) will perform differently, so if you're looking for the best performance, you should run your own testing which backend and settings work best for your own use case.

For testing llama.cpp performance, it is best to use the included `llama-bench`. By default it generates a `pp512` and `tg128` number, running each test 5 times.

Here are the most important flags you should be aware of:

- `-ngl 999` - This specifies the number of layers of a model is loaded to the GPU and is by default set to `99` - there are some large models that are larger so you may want to set `999` in those cases. Even though Strix Halo has unified memory, you always want to load all your layers to "GPU" memory as it has full access to memory bandwidth (about 2X faster than CPU)
- `--mmap 0` - Memory mapping can allow lazy loading of a model and is enabled by default, but when you load large models to the GPU, it will make loading moderately slower for Vulkan, and catastrophically slower for ROCm. You should always set `--mmap 0` or `--no-mmap` (different flags for different commands, check `--help` for the right one for each command if necessary)
- `-fa 1` - while you may take a slight hit on small context performance depending on backend, you will almost always want to use [Flash Attention](https://towardsdatascience.com/understanding-flash-attention-writing-the-algorithm-from-scratch-in-triton-5609f0b143ea/) for long context and real world usage, so it's best to just test with this on unless you are sure you won't be using longer context.
- `-b`, `-ub` - These specify logical and physical max batch size and default to 2048 and 512 respectively. You can increase these to improve longer context performance, however `-b` raises reserved memory and `-ub` raises peak memory usage. You can get some gains in performance (diminishing returns) increasing these, but you also risk OOM errors 
- `-r` - if you are doing tests on ultra-long context, these can sometimes take an hour or even longer to run a single pass - for these tests setting repetition to 1 is advised: `-r 1` - if you want something more statistically valid or test under load, you could of course go the other way and set `-r 10` or higher
- `-p`, `-n`, `-pg`, `-d` - These control the various tests. `-p` (default 512) measures `pp` "prompt processing" speed or prefill - this is how many tokens are in your prior context/conversation and is typically compute limited. `-n` (default 128) tests your `tg` token generation - this is the speed at which new tokens generate after you've processed your prompt and is typically memory limited. `-pg` let's you measure a pp+tg together, and `-d` lets you specify a "depth" or number of tokens before you test - eg, if you specify `-p 0 -n 128 -d 10000` it will measure the speed that tokens generate after there is 10000 tokens of context - the longer the context length, the slower this will be.

## Long Context Length Testing

Most tests use the default llama-bench numbers (pp512/tg128) and these are good for broad comparisons, however as you can see [even from 1-4096 sweeps](https://github.com/lhl/strix-halo-testing/tree/main/llm-bench/llama-2-7b.Q4_0), different backends and settings have different performance characteristics as context extends.

For example, running tests on the AMDVLK vs RADV Vulkan backends, while RADV is about 4% on `tg128`, it's 16% slower for `pp512`. One some models the `pp512` performance gap is even bigger if you're [looking purely at pp512/tg128 numbers](https://kyuz0.github.io/amd-strix-halo-toolboxes/). This test takes ~5s to run and varies wildly for different model. For example, for Qwen 3 30B-A3B UD-Q4_K_XL RADV is actually a bit *faster* than AMDVLK:

```
❯ build/bin/llama-bench -fa 1 -r 1 --mmap 0 -m /models/gguf/Qwen3-30B-A3B-UD-Q4_K_XL.gguf 
❯ AMD_VULKAN_ICD=RADV build/bin/llama-bench -fa 1 -r 1 --mmap 0 -m /models/gguf/Qwen3-30B-A3B-UD-Q4_K_XL.gguf
```
| Backend       | pp512 (t/s)   | tg128 (t/s)   |
|---------------|---------------|---------------|
| Vulkan AMDVLK | 741.60| 81.79 |
| Vulkan RADV   | 755.14 | 85.11 |

However, running these tests at the end of its 128K context depth gives you an decidedly different picture. For long context, the RADV driver in this case scales **much better**:

```
❯ build/bin/llama-bench -fa 1 -r 1 --mmap 0 -m /models/gguf/Qwen3-30B-A3B-UD-Q4_K_XL.gguf -d 130560
❯ AMD_VULKAN_ICD=RADV build/bin/llama-bench -fa 1 -r 1 --mmap 0 -m /models/gguf/Qwen3-30B-A3B-UD-Q4_K_XL.gguf -d 130560
```

| Backend       | pp512 @ d130560 (t/s)   | tg128 @ d130560 (t/s)   |
|---------------|---------------|---------------|
| Vulkan AMDVLK | 10.75 | 3.51 |
| Vulkan RADV   | 17.24 | 12.54  |

These runs took about ~2-3h to run on Strix Halo. You can do this with the ROCm backend as well (I recommend using the rocWMMA compile, and testing with and without `ROCBLAS_USE_HIPBLASLT=1` to compare the rocBLAS vs hipBLASlt kernels, but I leave that as a multi-hour exercise for the reader. This is simply illustrative for the differences you might see. (If you're doing an overnight run, doing something like `-d 0,5000,10000,50000,100000` might give you a better idea of how things slow down as context grows. 

NOTE: `pp` or context processing is critically important if you are loading previous long conversations/large context (agentic flows) or adding many tokens (grounding via search, tools, file attachments, etc), however in single-user conversational usage, `llama-cli` has a `--prompt-cache-all` flag and `llama-server` allows you to submit `cache_prompt=True` with your request to be able to use previously cached context similar to vLLM's [automatic prefix caching](https://docs.vllm.ai/en/stable/features/automatic_prefix_caching.html) or SGLang's [radix caching](https://lmsys.org/blog/2024-01-17-sglang/).

## Bonus ROCm numbers
Built w/ rocWMMA (`-DGGML_HIP_ROCWMMA_FATTN=ON`)

```
❯ build/bin/llama-bench -fa 1 -r 1 --mmap 0 -m /models/gguf/Qwen3-30B-A3B-UD-Q4_K_XL.gguf
❯ ROCBLAS_USE_HIPBLASLT=1 build/bin/llama-bench -fa 1 -r 1 --mmap 0 -m /models/gguf/Qwen3-30B-A3B-UD-Q4_K_XL.gguf
```

| Backend       | pp512 (t/s)   | tg128 (t/s)   |
|---------------|---------------|---------------|
| ROCm  | 650.59 | 64.17 |
| ROCm hipBLASlt | 651.93 | 63.95 |

```
❯ build/bin/llama-bench -fa 1 -r 1 --mmap 0 -m /models/gguf/Qwen3-30B-A3B-UD-Q4_K_XL.gguf -d 130560
❯ ROCBLAS_USE_HIPBLASLT=1 build/bin/llama-bench -fa 1 -r 1 --mmap 0 -m /models/gguf/Qwen3-30B-A3B-UD-Q4_K_XL.gguf -d 130560
```

| Backend       | pp512 @ d130560 (t/s)   | tg128 @ d130560 (t/s)   |
|---------------|---------------|---------------|
| ROCm | 40.58 | 4.98 |
| ROCm hipBLASlt  | 40.35 | 4.97 |
