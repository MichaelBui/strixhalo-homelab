# Clustering

The up to 128GB of memory provided by Strix Halo systems are a lot of memory, but sometimes you want more 😈. Luckily with **Llama.cpp** there is an option to utilize multiple systems and take advantage of their memory to run inference with even larger models.

## Networking

All Strix Halo systems have two USB4 ports that are also Thunderbolt 3 compatible (40GBit/s).
If you connect Strix Halo systems using a Thunderbolt 3 cable, you should see a thunderbolt-net connection in NetworkManager rightaway. This will provide around 9 GBit/s of bandwidth.

There doesn't seem to be routing with thunderbolt-net, so with two ports you can connect three Strix Halo systems using for now. You can of course try to use other means to cluster more systems, like the Ethernet ports.

### Cabling

You need a cable that can handle 40 GBit/s. The cheapest cables that are known to work are "_UGOURD Thunderbolt 40gbps_" that you can usually get for less than $5 each at 0.3m length from AliExpress. Good luck!

Note that the [[Bosgame M5|Hardware/PCs/Bosgame_M5]] has one USB4 port on the front and one on the back.

### Bonding

Bonding is using multiple network interfaces to get higher throughput, kind of like RAID for harddisks. There is a pending [kernel patch](https://lore.kernel.org/netdev/20251215121109.4042218-1-mika.westerberg@linux.intel.com/T/#t) that will allow bonding with thunderbolt-net. Until then it won't work.

## Llama.cpp with RPC

The Llama.cpp RPC architecture is [explained in the documentation](https://github.com/ggml-org/llama.cpp/blob/master/tools/rpc/README.md). The version of Llama.cpp provided by **kyuz0**s toolboxes are compiled with this option enabled. 

Run `rpc-server` (part of llama.cpp) on all but one PC. It will make available your iGPU to the master PC. Use the `-c` option to make it cache the LLM data on the local disk (in the directory `~/.cache/llama.cpp/rpc/`). This will speed things up considerably on subsequent invocations of the same LLM.

On the **master** PC, you start `llama-server` with the `--rpc` option and provide it with the addresses of the other PCs. If you use thunderbolt networking, make sure to give the addresses of the thunderbolt interfaces.

### For example

- Master PC with thunderbolt0 interface and IPv4 address 192.168.230.1

- Second PC with thunderbolt0 interface and IPv4 address 192.168.230.2

This second PC is running "`rpc-server -c`"

On the master PC start `llama-server` as usual, but add the parameter `--rpc 192.168.230.2:50052`. If you have three PCs, add the third PC with a comma as separator: `--rpc 192.168.230.2:50052,192.168.230.3:50052`. 
Voilà! 

## vLLM

vLLM is also capable of utilizing GPUs across multiple PCs. You have to setup a Ray cluster. If you get it to work, **please** document it here.

## Community

For further discussion join the [#beyond128g](https://discord.com/channels/1384139280020148365/1455307501472976979) discord channel.
