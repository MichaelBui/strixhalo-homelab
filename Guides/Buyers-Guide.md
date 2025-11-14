# Strix Halo Buyer's Guide

This guide is written based on my own experience and experiences of other people, as well as [the data from the owner's questionnaire](https://docs.google.com/forms/d/e/1FAIpQLSdckAFgHfRF7pt8-HXf9xCFCqkbg8uo9ZHBhGg78lDmcVtj-A/viewanalytics). It's in no way complete and is a subject of further improvements.


## Do I Even Need It?

#### AI Stuff

Strix Halo APUs are mainly considered to be a cheap way to run big LLMs and they indeed can do it fairly well, but not without some caveats. Most limitations come from memory bandwidth constraints. MoE (Mixture of Experts) models work much faster than dense ones, and while there are more and more MoE models coming out, you might still end up needing to use a dense model with severely lacking performance. 

The same goes for context size - **prompt processing and text generation speeds take a major hit as context grows**. Depending on model size and architecture, going over 64k, 32k, and in some cases even 16k of context becomes painful. Big context sizes could still work fairly well if the context grows gradually (like when you're using the model in chat mode), but don't expect miracles with massive document processing.

As an example, here are some charts illustrating the drop of speed with the growth of context size:  
![](./strix-halo-pp-tg-ctx.png)

**General rule: look at benchmarks with the specific model and context size you're interested in.** If you can't find them, ask in [our Discord](https://discord.gg/pnPRyucNrG) for someone to test it for you. This is important - don't give in to hype only to be disappointed later.

More on the AI topic - **image and video generation is just slow**. Again, depending on your use cases this might or might not be a problem, but be aware that the typical experience would probably be "set some tasks in the evening, come back to check results in the morning".

But the real elephant in the room is that AMD software support is lacking as always. It's been nearly a year since the platform was introduced, yet there are still problems with stability (especially on ROCm) and performance. The NPUs are still mostly unused, the SDKs are filled with bugs, and AMD's level of involvement is mediocre at best. The situation has been improving slightly over the last several months, but the whole ecosystem is mostly driven by community effort. The overall experience is indeed worse than what you could theoretically get with NVIDIA products.

#### Non-AI Stuff

What about other, non-AI related tasks? Well, it's great! If we're talking about the top-of-the-line AI Max+ 395, in terms of raw CPU performance it could even be compared to the 9950X3D (being roughly just 10-20% slower), which is really impressive. The iGPU is also the best one available on the market so far. Depending on the power limit, the 8060S performance lies somewhere between desktop versions of RTX 3060 and 4060, leaning closer to the latter and in some cases even beating it.

All of that, along with a huge amount of RAM, also makes the platform very interesting for running VMs and acting as a home server. However, note that [[GPU passthrough|Guides/VM-iGPU-Passthrough]] on consumer AMD cards is very lacking and plagued with problems.

#### All at Once

With that said, my personal opinion about the best use case is basically a kind of "jack of all trades" system. You get full x86 compatibility and something that can do anything and everything, albeit not at the highest possible levels and while being kinda pricey (especially with prices rising since November 2025).


## Choosing the Right Configuration

There are four main types of Strix Halo-based devices: mini PCs, AIOs, laptops, and handhelds. Laptops and handhelds are typically power and cooling limited, so their performance is lacking as well. However, the difference between 55W and 120W power limits isn't that massive ([[5-35% depending on the task|Guides/Power-Modes-and-Performance]]). This guide is mostly centered on mini PCs since this seems to be the most popular option anyway.

There are several different models of Strix Halo APUs available:  
![Strix Halo APU Specs](./strix-halo-specs.png)

Each APU comes with 32, 64, 96, or 128GB of onboard RAM that can be shared with the iGPU statically or dynamically. I'd personally recommend sticking to the best available option, which is the AI Max+ 395, but of course it's up to you and your preferred use case. Note that some manufacturers don't even provide options with lower-tier APUs.

The amount of RAM is another consideration. If you don't care about AI stuff, 32GB could be just enough, otherwise 64GB or higher is the way to go. **Important note**: None of the Strix Halo systems are upgradeable, so picking the beefiest option for both the APU and RAM amount is reasonable - and this is what most people do.


## What Mini PC to Buy

**Chinese OEMs (GMKtec, Bosgame, FEVM, Corsair, NIMO, etc.)**: About 90% of Chinese PCs are built on the same [[platform made by Sixunited|Hardware/Boards/Sixunited-AXB35]]. It's nothing to write home about, but performance-wise (which I think is the most important thing) it will be roughly the same as anything else, so it's a valid option if you want to go the cheapest route. Things to look out for: import taxes, warranty, support (or the lack thereof).

Based on the survey data, **[[GMKtec EVO-X2|Hardware/PCs/GMKtec-EVO-X2]]** and **[[Bosgame M5|Hardware/PCs/Bosgame-M5]]** seem to be the most popular Chinese options, with most buyers being satisfied with their purchases. GMKtec appears slightly more reliable, while some Bosgame units had minor issues like stuck power buttons.

**[[Corsair AI Workstation 300|Hardware/PCs/Corsair-AI-Workstation-300]]** is also a decent option. It's a stock Sixunited's PC, but brand reputation likely force them to have a better quality control and support. 

**[[Beelink GTR9Pro|Hardware/PCs/Beelink-GTR9-Pro]]** is a very interesting system in Mac Mini style which is unfortunately plagued by major stability problems which no one was able to solve for over 3 months already. Avoid unless something changes on that front.

**[[Minisforum MS-S1 MAX|Hardware/PCs/Minisforum-MS-S1-MAX]]** seems decent and also has a PCIe slot for expandability, along with two 10G Ethernets. Early feedback suggests good build quality and premium feel.

**[[HP Z2 Mini G1a|Hardware/PCs/HP-Z2-Mini-G1a]]** is still the only option from big manufacturers. They claim there's an ECC memory option, but I've never seen anyone with actual ECC modules (only ECC-link). It boosts up to 160W (although going beyond 120W leads to very questionable gains) and is noisier than Sixunited's cooling, which is hard to imagine. Pricing is all over the place, if someone could find it for an adequate price (sub $2500), then it could be recommended because with HP you at least get some level of support. Also features the PRO version of the APU, with the only real difference being DASH support.

**[[Framework Desktop|Hardware/PCs/Framework-Desktop]]** is the crown jewel of them all - pretty good in all possible measurements, except for size. It can be recommended as a guaranteed plug-and-play device with decent support and a huge community. However, it's also a bit pricey and availability sucks. Some users experienced longer wait times (over a month), and there were occasional PSU fan noise issues.

You can also check out [the spreadsheet that aims to compare all possible options](https://discord.com/channels/@me/1427396731070320732/1427401860322562231) (could be a bit outdated).

## Size and Noise

To evaluate the case sizes, there is [a great infographic created by RexYuan](https://gist.github.com/RexYuan/3fc27edcd12475e496eb20946f8c8485):  
![Strix Halo PC Sizes](./strix-halo-pc-sizes.png)

Most mini PCs are considered noisy in general. The typical cooling solution for almost every PC except for the Framework's consists of two high-RPM blowers that create enough pressure to push sufficient air through the heatsink. On the full 120W power limit, it's almost guaranteed to be noisy no matter what you do. If you have a chance to put the PC in a distant location and only work with it remotely (SSH, RDP, streaming), this might be ideal.


## DIY Way

It is possible to buy just the board and build your own PC with it, but you'll basically be limited to the [[Framework's one|Hardware/Boards/Framework-Desktop-Mainboard]]. There are other options from [[Sixunited|Hardware/Boards/Sixunited-STHT1]] and [[MeeGoPad|Hardware/Boards/MeeGoPad-100AM]], but they seem to be hard to get and no owners were encountered in the wild just yet. If anything could be concluded from the quality of descriptions and brand reputation - avoid.


## Country-Specific Information

**European Union**: Most buyers don't have to pay anything extra, since many Chinese manufacturers now ship from EU warehouses (particularly Germany) to avoid import duties. Delivery times are typically 1-2 weeks.

**United States**: Some products are available through local retailers like Microcenter. Framework and Corsair seem popular for those wanting better support.

**Canada**: Pricing tends to be slightly higher due to exchange rates and taxes.

**Asia-Pacific**: Pricing varies significantly by country. In Japan, domestic online shopping sites often offer better deals than direct manufacturer purchases. South Korea has notably high pricing (over $2200 equivalent) due to import duties and local retailer markups, also for some bizarre reason PCs could lack WI-Fi modules. 

**China**: Obviously the cheapest option if you're local, no import taxes, no delays.


## Alternatives

For AI workloads, there are two obvious alternatives: **NVIDIA DGX Spark** and **Apple Silicon Macs**. Both would be pricier with the same amount of RAM, both are tied to their own ecosystems (not x86), but both could be more performant for AI use cases too. If AI is your main requirement and you don't care about x86 compatibility, I highly recommend researching these options before committing to anything.

For gaming and general tasks, you should be able to build a more performant system for less money, although it will likely take more space and draw more power. A traditional desktop with a mid to high-end dedicated GPU will almost always outperform Strix Halo in gaming scenarios.


## Conclusions

Think before you buy! Consider available options, read reviews, check out benchmarks, and ask current owners about their experiences. Figuring it all out might be overwhelming, but there's really no way around it.

The questionnaire data shows that most buyers (about 85%) are satisfied with their purchases, but satisfaction varies significantly by use case and expectations. Those focused purely on AI workloads tend to be less satisfied due to software ecosystem limitations, while those using these systems as general-purpose machines tend to be happier.

**Key takeaways:**
- Don't buy based on hype alone - verify performance with your specific use cases
- Chinese OEMs offer the best value but with limited support
- Framework/HP could cost more but provide better support and build quality  
- Factor in import duties and taxes for your specific location, read seller conditions
- Be prepared for noise and consider remote usage if possible
- Remember that nothing except for storage is upgradeable - buy the specs you need from day one

The Strix Halo platform is genuinely impressive from a technical standpoint, but it's not a magic bullet. Set realistic expectations and you'll likely be happy with what it can do.

Good luck!
