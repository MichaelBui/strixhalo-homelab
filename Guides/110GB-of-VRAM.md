# 110GB of VRAM

:::info
# Notice
This is possible on Linux only and wasn't yet tested by me. Sources:
 - https://community.frame.work/t/framework-laptop-13-ryzen-300-configuring-graphics-memory/65389/16
 - https://blog.hjc.im/strix-halo-local-llm.html
:::

### Module Options
`/etc/modprobe.d/vram.conf`:
```bash
options amdgpu gttsize=110000
options ttm pages_limit=26856000
options ttm page_pool_size=26856000
```

Note that **110GB is NOT a hard limit**, you can allocate more, as long as there is enough memory for the OS to work.

### Performance Concerns
There were reports that when IOMMU is active and the VRAM is being set on the OS level, the memory latency becomes too high. To prevent this, either disable IOMMU in the BIOS or set `amd_iommu=off` it kernel boot params.

### Relevant Pages
 - [[Guides/AI-Capabilities]]
