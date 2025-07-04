# Power Modes & Performance

:::warning
# Warning
This article is a work in progress!
:::

### Intro
This guide is supposed to show how the performance of the APU changes on various power levels and to help you pick the optimal power mode for your Strix Halo system.

All tests were run at fixed fans speed of 80% to make sure that cooling level is constant, CPU temp never exceeded 75 °C. For GPU becnhmarks please note that the GPU was in a passthrough mode to the Windows VM and only 12 CPU cores were passed as well, so there is at least 5% of a performance drop. In general, look at the percentage changes.

**Everything single-threaded is omitted here, since a single core could never use the full power limit even if it's 55W.**

### CPU - PassMark
Details:
 - [PassMark PerformanceTest for Linux](https://www.passmark.com/products/pt_linux/download.php), version 11.0.1002
 - Average of 5 medium tests for CPU, to lower the influence of the initial power boost (`./pt_linux_x64 -i 5 -d 2`)

| Metric | [55W](https://www.passmark.com/baselines/V11/display.php?id=509787657486) | [85W](https://www.passmark.com/baselines/V11/display.php?id=509787842837) | [120W](https://www.passmark.com/baselines/V11/display.php?id=509787902430) |
| ----------------------------------- | ------- | ------- | ------- |
| **CPU Mark**                        | **49635** | **58042<sup style="color: #1fea00; font-size: 0.8em;">+16.9%</sup>** | **62805<sup style="color: #1fea00; font-size: 0.8em;">+8.2%</sup>** |
| Integer Math (MOps/s)               | 193409 | 222270<sup style="color: #1fea00; font-size: 0.8em;">+14.9%</sup> | 233483<sup style="color: #1fea00; font-size: 0.8em;">+5.0%</sup> |
| Floating Point Math (MOps/s)        | 107679 | 127114<sup style="color: #1fea00; font-size: 0.8em;">+18.0%</sup> | 138255<sup style="color: #1fea00; font-size: 0.8em;">+8.8%</sup> |
| Find Prime Numbers (MPrimes/s)      | 263 | 286<sup style="color: #1fea00; font-size: 0.8em;">+8.7%</sup> | 302<sup style="color: #1fea00; font-size: 0.8em;">+5.6%</sup>   |
| Random String Sorting (kStrings/s)  | 71735  | 88253<sup style="color: #1fea00; font-size: 0.8em;">+23.0%</sup> | 98842<sup style="color: #1fea00; font-size: 0.8em;">+12.0%</sup> |
| Data Encryption (MB/s)              | 33582 | 40310<sup style="color: #1fea00; font-size: 0.8em;">+20.0%</sup> | 44246<sup style="color: #1fea00; font-size: 0.8em;">+9.8%</sup> |
| Data Compression (KB/s)             | 539857 | 683287<sup style="color: #1fea00; font-size: 0.8em;">+26.6%</sup> | 777048<sup style="color: #1fea00; font-size: 0.8em;">+13.7%</sup> |
| Physics (Frames/s)                  | 4802 | 5477<sup style="color: #1fea00; font-size: 0.8em;">+14.0%</sup> | 5806<sup style="color: #1fea00; font-size: 0.8em;">+6.0%</sup> |
| Extended Instructions (MMatrices/s) | 38464 | 49404<sup style="color: #1fea00; font-size: 0.8em;">+28.4%</sup> | 58761<sup style="color: #1fea00; font-size: 0.8em;">+18.9%</sup> |

### CPU - Geekbench
:::info
# Notice
Geekbench does NOT test the system under full load, a lot of the tests don't even utilize all of the available cores, consider these results to be a reflection of an average daily lite usage with no extreme load
:::

Details:
 - [Geekbench for Linux](https://www.geekbench.com/download/), version 6.4.0
 - 1 test on every power mode with a 3 minute delay

| Metric | [55W](https://browser.geekbench.com/v6/cpu/12718643) | [85W](https://browser.geekbench.com/v6/cpu/12718729) | [120W](https://browser.geekbench.com/v6/cpu/12719008) |
| --------------------- | ------- | ------- | ------- |
| **Multi-Core Score**  | **17328** | **17544<sup style="color: #1fea00; font-size: 0.8em;">+1.2%</sup>** | **17516<sup style="color: red; font-size: 0.8em;">-0.2%</sup>** |
| File Compression      | 11184 | 12063<sup style="color: #1fea00; font-size: 0.8em;">+7.9%</sup> | 12197<sup style="color: #1fea00; font-size: 0.8em;">+1.1%</sup> |
| Navigation            | 13578 | 13050<sup style="color: red; font-size: 0.8em;">-3.9%</sup> | 13322<sup style="color: #1fea00; font-size: 0.8em;">+2.1%</sup> |
| HTML5 Browser         | 16030 | 17040<sup style="color: #1fea00; font-size: 0.8em;">+6.3%</sup> | 15610<sup style="color: red; font-size: 0.8em;">-8.4%</sup> |
| PDF Renderer          | 25044 | 24893<sup style="color: red; font-size: 0.8em;">-0.6%</sup> | 26726<sup style="color: #1fea00; font-size: 0.8em;">+7.4%</sup> |
| Photo Library         | 25989 | 27260<sup style="color: #1fea00; font-size: 0.8em;">+4.9%</sup> | 27099<sup style="color: red; font-size: 0.8em;">-0.6%</sup> |
| Clang                 | 42312 | 44101<sup style="color: #1fea00; font-size: 0.8em;">+4.2%</sup> | 44315<sup style="color: #1fea00; font-size: 0.8em;">+0.5%</sup> |
| Text Processing       | 3723 | 3714<sup style="color: red; font-size: 0.8em;">-0.2%</sup> | 3667<sup style="color: red; font-size: 0.8em;">-1.3%</sup> |
| Asset Compression     | 30848 | 31534<sup style="color: #1fea00; font-size: 0.8em;">+2.2%</sup> | 32421<sup style="color: #1fea00; font-size: 0.8em;">+2.8%</sup> |
| Object Detection      | 13032 | 13075<sup style="color: #1fea00; font-size: 0.8em;">+0.3%</sup> | 12545<sup style="color: red; font-size: 0.8em;">-4.1%</sup> |
| Background Blur       | 12406 | 12308<sup style="color: red; font-size: 0.8em;">-0.8%</sup> | 12207<sup style="color: red; font-size: 0.8em;">-0.8%</sup> |
| Horizon Detection     | 20156 | 19947<sup style="color: red; font-size: 0.8em;">-1.0%</sup> | 19792<sup style="color: red; font-size: 0.8em;">-0.8%</sup> |
| Object Remover        | 18049 | 18382<sup style="color: #1fea00; font-size: 0.8em;">+1.8%</sup> | 18796<sup style="color: #1fea00; font-size: 0.8em;">+2.3%</sup> |
| HDR                   | 13929 | 13775<sup style="color: red; font-size: 0.8em;">-1.1%</sup> | 13791<sup style="color: #1fea00; font-size: 0.8em;">+0.1%</sup> |
| Photo Filter          | 14335 | 13066<sup style="color: red; font-size: 0.8em;">-8.9%</sup> | 13849<sup style="color: #1fea00; font-size: 0.8em;">+6.0%</sup> |
| Ray Tracer            | 38300 | 40547<sup style="color: #1fea00; font-size: 0.8em;">+5.9%</sup> | 38962<sup style="color: red; font-size: 0.8em;">-3.9%</sup> |
| Structure from Motion | 22188 | 22210<sup style="color: #1fea00; font-size: 0.8em;">+0.1%</sup> | 21352<sup style="color: red; font-size: 0.8em;">-3.9%</sup> |

### GPU - Various Benchmarks
Details:
 - UNIGINE Superposition was run with the DirectX 1080p Extreme preset.
 - 3DMARK Steel Nomad Light was run in Vulkan.

| Benchmark                | 55W | 85W | 120W |
| ------------------------ | --- | --- | ---- |
| PassMark 2D Mark         | 621 | 706<sup style="color: #1fea00; font-size: 0.8em;">+13.7%</sup> | 758<sup style="color: #1fea00; font-size: 0.8em;">+7.4%</sup> |
| PassMark DirectX 10      | 59 | 65<sup style="color: #1fea00; font-size: 0.8em;">+10.2%</sup> | 78<sup style="color: #1fea00; font-size: 0.8em;">+20.0%</sup> |
| PassMark DirectX 11      | 112 | 134<sup style="color: #1fea00; font-size: 0.8em;">+19.6%</sup> | 148<sup style="color: #1fea00; font-size: 0.8em;">+10.4%</sup> |
| PassMark DirectX 12      | 63 | 67<sup style="color: #1fea00; font-size: 0.8em;">+6.3%</sup> | 80<sup style="color: #1fea00; font-size: 0.8em;">+19.4%</sup> |
| PassMark GPU Compute     | 6229 | 6777<sup style="color: #1fea00; font-size: 0.8em;">+8.8%</sup> | 7311<sup style="color: #1fea00; font-size: 0.8em;">+7.9%</sup> |
| UNIGINE Superposition    | 5247 | 6103<sup style="color: #1fea00; font-size: 0.8em;">+16.3%</sup> | 6522<sup style="color: #1fea00; font-size: 0.8em;">+6.9%</sup> |
| 3DMARK Steel Nomad Light | 8494 | 9883<sup style="color: #1fea00; font-size: 0.8em;">+16.4%</sup> | 10608<sup style="color: #1fea00; font-size: 0.8em;">+7.3%</sup> |
| 3DMARK Time Spy          | 8891 | 10230<sup style="color: #1fea00; font-size: 0.8em;">+15.1%</sup> | 10710<sup style="color: #1fea00; font-size: 0.8em;">+4.7%</sup> |

### GPU - Gaming
`WIP`

### Conslusions
**For CPU** we have a classic situation of diminishing returns, where **85W mode seems to be the sweet spot**. Going higher doesn't make much sense unless you want to squeeze the absolute maximum out of your system, and going lower is only suitable if you don't use your system resources fully (especially when you don't utilize all of the cores) or don't care about performance loss.

**For GPU** the situation is a bit different, seems that it can utilise power more effectively, but more testing is needed, I'm on it.
