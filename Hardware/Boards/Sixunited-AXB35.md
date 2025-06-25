# Sixunited AXB35
One of the first Strix Halo boards, used in many Chinese PCs along with a cooling system. First revisions go back to at least October of 2024 (marked `SU_AXB35_FB11`).

The board supports 3 different power limits - 55W (100W burst), 85W (120W burst), and 120W (140W burst).

Known PCs:
 - [[GMKtec EVO-X2|Hardware/PCs/GMKtec-EVO-X2]] (`SU_AXB35-02_FP11`)
 - [[Bosgame M5|Hardware/PCs/Bosgame-M5]]
 - FEVM FX-EX9/FA-EX9
 - [[Peladn YO1|Hardware/PCs/Peladn-YO1]]

### Cooling
The cooling system is decent enough for this form factor, but could suffer from inadequate thermal interface conductivity. So if you plan to use a full 120W power limit, switching to PTM7950 or similar products is highly recommended. You'll need a 30x30 mm piece of 0.2-0.25 mm thickness.

Here's what you could expect from a correctly working system with PTM7950:  
![](./axb35-ptm7950-cooling.png)

### Facts
 - EC: ITE IT5570E-128
 - Heatsink mount: 75x75 mm
 - RAM thermal pads thickness: 0.5 mm

### Firmware
See [[Firmware|Hardware/Boards/Sixunited-AXB35/Firmware]] page.

### Photos

| Photo | Description |
| -------- | -------- |
| [![SU_AXB35-02](./axb35-02.jpeg?thumbnail)](./axb35-02.jpeg) | AXB35-02 from GMKtec EVO-X2 |
| [![SU_AXB35](./axb35_board_with_cooling.jpg?thumbnail)](./axb35_board_with_cooling.jpg) | Earlier revision of the board with the cooling system |
