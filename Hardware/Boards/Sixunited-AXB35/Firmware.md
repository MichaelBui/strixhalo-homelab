# Sixunited AXB35 Firmware

### BIOS & EC upgrade

:::info
If you happen to have BIOS or EC firmware versions not presented on this page and would like to share, please **[contact me directly](https://d7.wtf/contact)** or **[join our Discord](https://discord.gg/pnPRyucNrG)**
:::

:::danger
**DO NOT FLASH ANY FIRMWARE IF YOUR SYSTEM IS STABLE AND FUNCTIONING PROPERLY**, only consider a BIOS update if you have specific technical requirements or are experiencing any issues
:::

All devices sent after the middle of May seem to have BIOS version of at least 1.04. The only officially supported upgrade way is using Windows, follow the included instructions. Something like [Hiren's BootCD](https://www.hirensbootcd.org) could be used as a more convenient option. **You're supposed to update EC firmware first and BIOS the last.**

All AXB35 firmwares should be compatible across AXB35-based devices, allowing you to flash any OEM BIOS regardless of the original manufacturer. However, **proceed at your own risk** as this practice is not officially supported. Technically it shouldn't even be possible to flash incompatible firmware because it should have a different ROM ID. Currently, **we have several confirmed successful cases of flashing GMK BIOS onto a FEVM and Bosgame PCs without any issues**.

A debug mode can be toggled by pressing **ALT+F5**. Enabling the mode adds a second "Advanced" tab that contains a lot of additional options, as well as some additional settings in the existing tabs. However, some of them are glitchy or don't work at all. In some cases changing these can even lead to your system not booting unless you erase your settings by removing the battery. All in all, it's not recommended to mess with the debug mode without a particular reason.

> [!CAUTION]
> After initiating the BIOS upgrade, your PC will reboot and the process will continue in a special interface.  
> ‼️ **DO NOT turn off your computer or interrupt power**  
> ‼️ **DO NOT press any keys during this process**  
> ‼️ **BE PATIENT, the screen may remain blank for several minutes**  


### GMKtec EVO-X2

BIOS and EC firmware are no longer available [on the GMKtec website](https://www.gmktec.com/pages/drivers-and-software), but you can download previously saved copies here.

**BIOS**

| Version  | Official Changelog | Notes    | Download |
| -------- | ------------------ | -------- | -------- |
| **1.11 20251017** | N/A | Suddenly appeared on the GMK's Google Drive. Community found changes:<br>1. <span style="color:red">It's no longer possible to use the `O` key in the BIOS and on the boot level</span> (yes, you read that right).<br>2. Minimum VRAM allocation is now 1GB, not 512MB. | [AXB35-02_GMK_SW1.11_20251017.zip](./AXB35-02_GMK_SW1.11_20251017.zip) |
| **1.05 20250729** | N/A | Suddenly appeared on the GMK's Google Drive. GMK's support said this is a final version of the previous **20250716** fix. | [AXB35-02_GMK_SW1.05_20250729.zip](./AXB35-02_GMK_SW1.05_20250729.zip) |
| **1.05 20250716** | Solve the problem of SD card reverse locking | Suddenly appeared on the GMK's Google Drive. | [AXB35-02_GMK_SW1.05_20250716.zip](./AXB35-02_GMK_SW1.05_20250716.zip) |
| **1.05 20250606** | N/A | According to Sixunited this one contains many changes from upstream and the rest is confidential. I personally noticed more conservative voltages and slightly improved temperature because of that. | [AXB35-02_GMK_SW1.05_20250606.zip](./AXB35-02_GMK_SW1.05_20250606.zip) |
| **1.04** | 1. Add virtualization<br>2. The first startup item is displayed as USB<br>3. The Core Performance Boost function is hidden<br>4. Add GFX Configuration in the BIOS to adjust the video memory<br>5. The BIOS adds a fan adjustment menu<br>6. Configure the BIOS at 64G/128G for automatic compatibility recognition | - | [AXB35-02_GMK_SW1.04_20250514.zip](./AXB35-02_GMK_SW1.04_20250514.zip) |


**EC Firmware**

| Version  | Official Changelog | Notes | Download |
| -------- | ------------------ | ----- | -------- |
| **1.06** | N/A | Seems to have slightly quieter fan curves | [EC-AXB35-02-1.06.zip](./EC-AXB35-02-1.06.zip) |
| **1.04** | N/A | First version that supports manual fan control | [EC-AXB35-02-1.04.zip](./EC-AXB35-02-1.04.zip) |


### Bosgame M5

There is no BIOS available for download (yet) at the Bosgame PC [support website](https://www.bosgamepc.com/pages/support).

| Version  | Official Changelog | Notes | Download |
| -------- | ------------------ | ----- | -------- |
| **1.07 20250912** | N/A | This BIOS was dumped from a unit bought in October. | [AXB35-02_BOSGAME_SW1.07_20250912.rom](./AXB35-02_BOSGAME_SW1.07_20250912.rom) |

### Relevant Pages
 - [[Hardware/Boards/Sixunited-AXB35]]
 - [[Hardware/PCs/GMKtec-EVO-X2]]
