# Power Modes & Performance
This guide is supposed to show how the performance of the APU changes on various power levels and to help you pick the optimal power mode for your Strix Halo system.

All tests were run at fixed fans speed of 80% to make sure that cooling level is constant, CPU temp never exceeded 75 °C. For GPU becnhmarks please note that the GPU was in a passthrough mode to the Windows VM and only 12 CPU cores were passed as well, so there is at least 5% of a performance drop. In general, look at the percentage changes (relative to the base 55W result).

**Everything single-threaded is omitted here, since a single core could never use the full power limit even on the lowest 55W value.**

### CPU - PassMark
Details:
 - [PassMark PerformanceTest for Linux](https://www.passmark.com/products/pt_linux/download.php), version 11.0.1002
 - Average of 5 medium tests for CPU, to lower the influence of the initial power boost (`pt_linux_x64 -i 5 -d 2`)

| Metric | [55W](https://www.passmark.com/baselines/V11/display.php?id=509787657486) | [85W](https://www.passmark.com/baselines/V11/display.php?id=509787842837) | [120W](https://www.passmark.com/baselines/V11/display.php?id=509787902430) |
| ----------------------------------- | ------- | ------- | ------- |
| **CPU Mark**                        | **49 635** | **58 042 <sup style="color: #1fea00; font-size: 0.8em;">+16.9%</sup>** | **62 805 <sup style="color: #1fea00; font-size: 0.8em;">+26.5%</sup>** |
| Integer Math (MOps/s)               | 193 409 | 222 270 <sup style="color: #1fea00; font-size: 0.8em;">+14.9%</sup> | 233 483 <sup style="color: #1fea00; font-size: 0.8em;">+20.7%</sup> |
| Floating Point Math (MOps/s)        | 107 679 | 127 114 <sup style="color: #1fea00; font-size: 0.8em;">+18.0%</sup> | 138 255 <sup style="color: #1fea00; font-size: 0.8em;">+28.4%</sup> |
| Find Prime Numbers (MPrimes/s)      | 263 | 286 <sup style="color: #1fea00; font-size: 0.8em;">+8.7%</sup> | 302 <sup style="color: #1fea00; font-size: 0.8em;">+14.8%</sup> |
| Random String Sorting (kStrings/s)  | 71 735 | 88 253 <sup style="color: #1fea00; font-size: 0.8em;">+23.0%</sup> | 98 842 <sup style="color: #1fea00; font-size: 0.8em;">+37.8%</sup> |
| Data Encryption (MB/s)              | 33 582 | 40 310 <sup style="color: #1fea00; font-size: 0.8em;">+20.0%</sup> | 44 246 <sup style="color: #1fea00; font-size: 0.8em;">+31.8%</sup> |
| Data Compression (KB/s)             | 539 857 | 683 287 <sup style="color: #1fea00; font-size: 0.8em;">+26.6%</sup> | 777 048 <sup style="color: #1fea00; font-size: 0.8em;">+43.9%</sup> |
| Physics (Frames/s)                  | 4 802 | 5 477 <sup style="color: #1fea00; font-size: 0.8em;">+14.1%</sup> | 5 806 <sup style="color: #1fea00; font-size: 0.8em;">+20.9%</sup> |
| Extended Instructions (MMatrices/s) | 38 464 | 49 404 <sup style="color: #1fea00; font-size: 0.8em;">+28.4%</sup> | 58 761 <sup style="color: #1fea00; font-size: 0.8em;">+52.8%</sup> |

\* **All percentage values are relative to the base 55W result**

### CPU - Geekbench
:::info
# Notice
Geekbench does NOT test the system under full load, a lot of the tests don't even utilize all of the available cores, consider these results to be a reflection of an average daily lite usage with no substantial load
:::

Details:
 - [Geekbench for Linux](https://www.geekbench.com/download/), version 6.4.0
 - 1 test on every power mode with a 3 minute delay

| Metric | [55W](https://browser.geekbench.com/v6/cpu/12718643) | [85W](https://browser.geekbench.com/v6/cpu/12718729) | [120W](https://browser.geekbench.com/v6/cpu/12719008) |
| --------------------- | ------- | ------- | ------- |
| **Multi-Core Score**  | **17 328** | **17 544 <sup style="color: #1fea00; font-size: 0.8em;">+1.2%</sup>** | **17 516 <sup style="color: #1fea00; font-size: 0.8em;">+1.1%</sup>** |
| File Compression      | 11 184 | 12 063 <sup style="color: #1fea00; font-size: 0.8em;">+7.9%</sup> | 12 197 <sup style="color: #1fea00; font-size: 0.8em;">+9.1%</sup> |
| Navigation            | 13 578 | 13 050 <sup style="color: red; font-size: 0.8em;">-3.9%</sup> | 13 322 <sup style="color: red; font-size: 0.8em;">-1.9%</sup> |
| HTML5 Browser         | 16 030 | 17 040 <sup style="color: #1fea00; font-size: 0.8em;">+6.3%</sup> | 15 610 <sup style="color: red; font-size: 0.8em;">-2.6%</sup> |
| PDF Renderer          | 25 044 | 24 893 <sup style="color: red; font-size: 0.8em;">-0.6%</sup> | 26 726 <sup style="color: #1fea00; font-size: 0.8em;">+6.7%</sup> |
| Photo Library         | 25 989 | 27 260 <sup style="color: #1fea00; font-size: 0.8em;">+4.9%</sup> | 27 099 <sup style="color: #1fea00; font-size: 0.8em;">+4.3%</sup> |
| Clang                 | 42 312 | 44 101 <sup style="color: #1fea00; font-size: 0.8em;">+4.2%</sup> | 44 315 <sup style="color: #1fea00; font-size: 0.8em;">+4.7%</sup> |
| Text Processing       | 3 723 | 3 714 <sup style="color: red; font-size: 0.8em;">-0.2%</sup> | 3 667 <sup style="color: red; font-size: 0.8em;">-1.5%</sup> |
| Asset Compression     | 30 848 | 31 534 <sup style="color: #1fea00; font-size: 0.8em;">+2.2%</sup> | 32 421 <sup style="color: #1fea00; font-size: 0.8em;">+5.1%</sup> |
| Object Detection      | 13 032 | 13 075 <sup style="color: #1fea00; font-size: 0.8em;">+0.3%</sup> | 12 545 <sup style="color: red; font-size: 0.8em;">-3.7%</sup> |
| Background Blur       | 12 406 | 12 308 <sup style="color: red; font-size: 0.8em;">-0.8%</sup> | 12 207 <sup style="color: red; font-size: 0.8em;">-1.6%</sup> |
| Horizon Detection     | 20 156 | 19 947 <sup style="color: red; font-size: 0.8em;">-1.0%</sup> | 19 792 <sup style="color: red; font-size: 0.8em;">-1.8%</sup> |
| Object Remover        | 18 049 | 18 382 <sup style="color: #1fea00; font-size: 0.8em;">+1.8%</sup> | 18 796 <sup style="color: #1fea00; font-size: 0.8em;">+4.1%</sup> |
| HDR                   | 13 929 | 13 775 <sup style="color: red; font-size: 0.8em;">-1.1%</sup> | 13 791 <sup style="color: red; font-size: 0.8em;">-1.0%</sup> |
| Photo Filter          | 14 335 | 13 066 <sup style="color: red; font-size: 0.8em;">-8.9%</sup> | 13 849 <sup style="color: red; font-size: 0.8em;">-3.4%</sup> |
| Ray Tracer            | 38 300 | 40 547 <sup style="color: #1fea00; font-size: 0.8em;">+5.9%</sup> | 38 962 <sup style="color: #1fea00; font-size: 0.8em;">+1.7%</sup> |
| Structure from Motion | 22 188 | 22 210 <sup style="color: #1fea00; font-size: 0.8em;">+0.1%</sup> | 21 352 <sup style="color: red; font-size: 0.8em;">-3.8%</sup> |
\* **All percentage values are relative to the base 55W result**
Details:
 - sysbench - 1.0.20, `sysbench cpu run --threads=32 --time=30`
 - cpubench1a - 5.0, `cpubench1a -bench -nb 3 -duration 30`
 - 7-Zip - 16.02, `7z b`

### CPU - Various Benchmarks
| Benchmark (Metric)         | 55W | 85W | 120W |
| -------------------------- | --- | --- | ---- |
| sysbench (Events/s)        | 85 160 | 96 139 <sup style="color: #1fea00; font-size: 0.8em;">+12.9%</sup> | 99 218 <sup style="color: #1fea00; font-size: 0.8em;">+16.5%</sup> |
| cpubench1a (MT Score)      | 3 483 | 4 288 <sup style="color: #1fea00; font-size: 0.8em;">+23.1%</sup> | 4 790 <sup style="color: #1fea00; font-size: 0.8em;">+37.5%</sup> |
| 7-Zip (Compressing MIPS)   | 120 759 | 135 008 <sup style="color: #1fea00; font-size: 0.8em;">+11.8%</sup> | 140 471 <sup style="color: #1fea00; font-size: 0.8em;">+16.3%</sup> |
| 7-Zip (Decompressing MIPS) | 103 110 | 124 170 <sup style="color: #1fea00; font-size: 0.8em;">+20.4%</sup> | 135 859 <sup style="color: #1fea00; font-size: 0.8em;">+31.8%</sup> |


### GPU - Various Benchmarks
Details:
 - UNIGINE Superposition - DirectX, 1080p Extreme preset
 - 3DMARK Steel Nomad Light - Vulkan, 1080p
 - 3DMARK Time Spy - 1080p

| Benchmark                | 55W | 85W | 120W |
| ------------------------ | --- | --- | ---- |
| PassMark 2D Mark         | 621 | 706 <sup style="color: #1fea00; font-size: 0.8em;">+13.7%</sup> | 758 <sup style="color: #1fea00; font-size: 0.8em;">+22.1%</sup> |
| PassMark DirectX 10      | 59 | 65 <sup style="color: #1fea00; font-size: 0.8em;">+10.2%</sup> | 78 <sup style="color: #1fea00; font-size: 0.8em;">+32.2%</sup> |
| PassMark DirectX 11      | 112 | 134 <sup style="color: #1fea00; font-size: 0.8em;">+19.6%</sup> | 148 <sup style="color: #1fea00; font-size: 0.8em;">+32.1%</sup> |
| PassMark DirectX 12      | 63 | 67 <sup style="color: #1fea00; font-size: 0.8em;">+6.3%</sup> | 80 <sup style="color: #1fea00; font-size: 0.8em;">+27.0%</sup> |
| PassMark GPU Compute     | 6 229 | 6 777 <sup style="color: #1fea00; font-size: 0.8em;">+8.8%</sup> | 7 311 <sup style="color: #1fea00; font-size: 0.8em;">+17.4%</sup> |
| UNIGINE Superposition    | 5 247 | 6 103 <sup style="color: #1fea00; font-size: 0.8em;">+16.3%</sup> | 6 522 <sup style="color: #1fea00; font-size: 0.8em;">+24.3%</sup> |
| 3DMARK Steel Nomad Light | 8 494 | 9 883 <sup style="color: #1fea00; font-size: 0.8em;">+16.4%</sup> | 10 608 <sup style="color: #1fea00; font-size: 0.8em;">+24.9%</sup> |
| 3DMARK Time Spy          | 8 891 | 10 230 <sup style="color: #1fea00; font-size: 0.8em;">+15.1%</sup> | 10 710 <sup style="color: #1fea00; font-size: 0.8em;">+20.5%</sup> |
\* **All percentage values are relative to the base 55W result**

### GPU - Gaming
Details:
 - all games use pre-defined presets and built-in benchmarks
 - all values are average FPS
 - for Metro Exodus ray tracing was additionally set to `normal`
 - for GTA5 the value is an average FPS of 5 different benchmarks

| Game                                           | 55W | 85W | 120W |
| ---------------------------------------------- | --- | --- | ---- |
| Cyberpunk 2077 (1080p low)                     | 61 | 70 <sup style="color: #1fea00; font-size: 0.8em;">+14.8%</sup> | 73 <sup style="color: #1fea00; font-size: 0.8em;">+19.7%</sup> |
| Cyberpunk 2077 (1080p high)                    | 53 | 62 <sup style="color: #1fea00; font-size: 0.8em;">+17.0%</sup> | 66 <sup style="color: #1fea00; font-size: 0.8em;">+24.5%</sup> |
| The Talos Principle (1080p ultra)              | 102 | 113 <sup style="color: #1fea00; font-size: 0.8em;">+10.8%</sup> | 117 <sup style="color: #1fea00; font-size: 0.8em;">+14.7%</sup> |
| Metro Exodus Enhanced Edition (1080p ultra)    | 51 | 59 <sup style="color: #1fea00; font-size: 0.8em;">+15.7%</sup> | 63 <sup style="color: #1fea00; font-size: 0.8em;">+23.5%</sup> |
| Red Dead Redemption 2 (1080p high)             | 55 | 58 <sup style="color: #1fea00; font-size: 0.8em;">+5.5%</sup> | 66 <sup style="color: #1fea00; font-size: 0.8em;">+20.0%</sup> |
| Grand Theft Auto V Enchanced (1080p very high) | 105 | 118 <sup style="color: #1fea00; font-size: 0.8em;">+12.4%</sup> | 137 <sup style="color: #1fea00; font-size: 0.8em;">+30.5%</sup> |
| Black Myth: Wukong (1080p high)                | 71 | 78 <sup style="color: #1fea00; font-size: 0.8em;">+9.9%</sup> | 84 <sup style="color: #1fea00; font-size: 0.8em;">+18.3%</sup> |
\* **All percentage values are relative to the base 55W result**

### GPU - LLM
Details:
 - koboldcpp, Vulkan, full GPU offloading
 - Q5_K_M

| Model (Metric)                              | 55W | 85W | 120W |
| ------------------------------------------- | --- | --- | ---- |
| Gemma 3 27B (Prompt Processing, Tokens/s)   | 77 | 89 <sup style="color: #1fea00; font-size: 0.8em;">+15.6%</sup> | 95 <sup style="color: #1fea00; font-size: 0.8em;">+23.4%</sup> |
| Gemma 3 27B (Generation Speed, Tokens/s)    | 6 | 6 <sup style="font-size: 0.8em;">+0.0%</sup> | 6 <sup style="font-size: 0.8em;">+0.0%</sup> |
| Qwen3 30B A3B (Prompt Processing, Tokens/s) | 92 | 95 <sup style="color: #1fea00; font-size: 0.8em;">+3.3%</sup> | 95 <sup style="color: #1fea00; font-size: 0.8em;">+3.3%</sup> |
| Qwen3 30B A3B (Generation Speed, Tokens/s)  | 25 | 29 <sup style="color: #1fea00; font-size: 0.8em;">+16.0%</sup> | 29 <sup style="color: #1fea00; font-size: 0.8em;">+16.0%</sup> |
\* **All percentage values are relative to the base 55W result**

### Conclusions
Let's look on the average increase for every category compared to 55W mode:

| Category                 | 85W | 120W |
| ------------------------ | --- | ---- |
| CPU - PassMark           | <span style="color: #1fea00;">+19.0%</span> | <span style="color: #1fea00;">+30.8%</span> |
| CPU - Geekbench          | <span style="color: #1fea00;">+1.0% | <span style="color: #1fea00;">+0.9%</span> |
| CPU - Various Benchmarks | <span style="color: #1fea00;">+17.0% | <span style="color: #1fea00;">+25.5%</span> |
| GPU - Various Benchmarks | <span style="color: #1fea00;">+13.3% | <span style="color: #1fea00;">+25.0%</span> |
| GPU - Gaming             | <span style="color: #1fea00;">+12.3% | <span style="color: #1fea00;">+21.6%</span> |
| GPU - LLM                | <span style="color: #1fea00;">+8.7% | <span style="color: #1fea00;">+10.7%</span> |

A classic situation of diminishing returns, where **85W mode seems to be the sweet spot** and a good compromise between performance gain and power consumption.

The 55W mode is only suitable if you don't fully utilize your system resources (especially when you don't leverage all cores) or aren't concerned about performance loss. Geekbench results reflect that perfectly.

The 120W mode generally doesn't make much sense unless you aim to extract the absolute maximum from your system, with one exception - certain games respond more positively to this increase. My theory is that CPU-heavy games might slightly starve the GPU, so increasing the overall power budget alleviates this issue. Additionally, if you're just shy of reaching 60 fps at 1080p (a reasonable target for this system), this extra power might provide the necessary boost.

For LLMs, memory bandwidth (which APU's power limit doesn't affect) appears to play a more significant role than raw computing power, so again, exceeding 85W offers diminishing benefits.

In summary:
 - 55W for light loads (background services, office work, multimedia)
 - 120W for intensive gaming sessions
 - 85W for mixed workloads and everything else
