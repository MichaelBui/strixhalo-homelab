# Sixunited AXB35 Firmware

::: info
If you happen to have BIOS or EC firmware versions not presented on this page and would like to share, please **[contact me directly](https://d7.wtf/contact)** or **[join our Discord](https://discord.gg/pnPRyucNrG)**
:::

### BIOS & EC upgrade
All devices sent after the middle of May seem to have BIOS version of at least 1.04. The only officially supported upgrade way is using Windows, follow the included instructions. Something like [Hiren's BootCD](https://www.hirensbootcd.org) could be used as a more convenient option. You're supposed to update EC firware first and BIOS the last.

> [!CAUTION]
> After initiating the BIOS upgrade, your PC will reboot and the process will continue in a special interface.  
> ‼️ **DO NOT turn off your computer or interrupt power**  
> ‼️ **DO NOT press any keys during this process**  
> ‼️ **BE PATIENT, the screen may remain blank for several minutes**  

### GMKtec EVO-X2

BIOS and EC firmware are available [on the GMKtec website](https://www.gmktec.com/pages/drivers-and-software), but since they update it rarely and put them on Google Drive, you can download them here as well.

**BIOS**

| Version  | Official Changelog | Notes    | Download |
| -------- | ------------------ | -------- | -------- |
| **1.05 20250716** | Solve the problem of SD card reverse locking | Suddenly appeared on the GMK's Google Drive. | [AXB35-02_GMK_SW1.05_20250716.zip](./AXB35-02_GMK_SW1.05_20250716.zip) |
| **1.05 20250606** | N/A | According to Sixunited this one contains many changes from upstream and the rest is confidential. I personally noticed more conservative voltages and slightly improved temperature because of that. | [AXB35-02_GMK_SW1.05_20250606.zip](./AXB35-02_GMK_SW1.05_20250606.zip) |
| **1.04** | 1. Add virtualization<br>2. The first startup item is displayed as USB<br>3. The Core Performance Boost function is hidden<br>4. Add GFX Configuration in the BIOS to adjust the video memory<br>5. The BIOS adds a fan adjustment menu<br>6. Configure the BIOS at 64G/128G for automatic compatibility recognition | - | [AXB35-02_GMK_SW1.04_20250514.zip](./AXB35-02_GMK_SW1.04_20250514.zip) |

**EC Firmware**

| Version  | Official Changelog | Notes | Download |
| -------- | ------------------ | ----- | -------- |
| **1.06** | N/A | Seems to have slightly quieter fan curves | [EC-AXB35-02-1.06.zip](./EC-AXB35-02-1.06.zip) |
| **1.04** | N/A | First version that supports manual fan control | [EC-AXB35-02-1.04.zip](./EC-AXB35-02-1.04.zip) |

### Relevant Pages
 - [[Hardware/Boards/Sixunited-AXB35]]
 - [[Hardware/PCs/GMKtec-EVO-X2]]
