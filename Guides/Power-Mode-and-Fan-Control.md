# Power Mode and Fan Control

### Problem
No fan and power mode control is available in Linux.

### Solution
There is a [ec-su_axb35-linux kernel module](https://github.com/cmetz/ec-su_axb35-linux) written by Christoph Metz.

It allows you to:
 - read current fan speeds and modes
 - switch modes of each of the three available fans
 - control fan speed in `fixed` mode
 - control fan speed with custom curves in `curve` mode
 - read current power mode
 - change current power mode
 - read current APU temperature

:::warning
# Warning
The module is a work in progress, use it at your own risk. It was tested on EC firmware version 1.06 and should theoretically only work on versions 1.04 and above. Please report any possible problems to the module's GitHub repo or in our Discord server.
:::

#### Generic Installation
1. Install module building dependencies (depends on your distro, on debian/ubuntu install `build-essential` and `linux-headers-$(uname -r)` packages).
2. Clone the repo with `git clone https://github.com/cmetz/ec-su_axb35-linux.git`.
3. Build and install the module with `cd ec-su_axb35-linux && sudo make install`.
4. Try loading the module with `modprobe ec_su_axb35` and check your `dmesg` afterwards. You should see the `Sixunited AXB35-02 EC driver loaded` message.
5. Run `scripts/info.sh` and check that all information is there.
6. Run `scripts/test_fan_mode_fixed.sh`, it should test your fans on all 6 fixed levels.
7. If everything is good, you can make the module automatically load on system boot with `sudo echo ec_su_axb35 >> /etc/modules`. If it says "permission denied", drop into root console with `su` or `sudo su -` and try again.

#### Usage
Reading and writing all of the parameters happens through sysfs with `/sys/class/ec_su_axb35` path. You can find detailed information in [the repo's readme file](https://github.com/cmetz/ec-su_axb35-linux/blob/main/README.md).

Some examples:
 - `cat /sys/class/ec_su_axb35/fan2/rpm` to get current rpm of fan2
 - `echo fixed > /sys/class/ec_su_axb35/fan3/mode && echo 2 > /sys/class/ec_su_axb35/fan3/level` to switch fan3 to fixed mode and set speed to level 2
 - `echo balanced > /sys/class/ec_su_axb35/apu/power_mode` to set power mode to balanced (85W)

You can also call `su_axb35_monitor` anywhere to monitor all available values in realtime:  
![su_axb35_monitor](./su_axb35_monitor.png)

To apply specific settings at system startup, place them in your `rc.local` file or utilize any other methods for executing commands during boot, such as creating a systemd unit. In the future, these parameters will also be configurable through module options.

#### Custom Curves
The custom curves define temperature thresholds for fan power levels:
- **Ramp-up curve** (`/sys/class/ec_su_axb35/fanX/rampup_curve`): temperature points where fan increases to the next level
- **Ramp-down curve** (`/sys/class/ec_su_axb35/fanX/rampdown_curve`): temperature points where fan decreases to the previous level

For example, with these settings:
- `rampup_curve = 60,70,83,95,97`
- `rampdown_curve = 40,50,80,94,96`

**When CPU is heating up:**
- Below 60°: Fan at level 0 (minimum)
- At 60°: Increases to level 1
- At 70°: Increases to level 2
- At 83°: Increases to level 3
- At 95°: Increases to level 4
- At 97°: Increases to level 5 (maximum)

**When CPU is cooling down:**
- Above 96°: Fan stays at level 5
- Below 96°: Decreases to level 4
- Below 94°: Decreases to level 3
- Below 80°: Decreases to level 2
- Below 50°: Decreases to level 1
- Below 40°: Decreases to level 0

This creates a buffer at each level that prevents the fan from rapidly switching between speeds when temperature fluctuates around threshold values.

Curves are being applied only when `curve` mode is set on a specific fan, each fan has their own curves.

### Fine-tuning Power Limits
If you want more gradual control over power modes, there's a [RyzenAdj](https://github.com/FlyGoat/RyzenAdj) utility.

Main 3 parameters we are interested in are:
 - `STAPM LIMIT` (sustained power draw)
 - `PPT LIMIT FAST` (boost power draw)
 - `PPT LIMIT SLOW` (average power draw)

Default values on all 3 selectable power modes (use `ryzenadj --info` to read them):

| Power Mode  | STAPM LIMIT | PPT LIMIT FAST | PPT LIMIT SLOW |
| ----------- | ----------- | -------------- | -------------- |
| Quiet       | 54.0        | 100.0          | 54.0           |
| Balanced    | 85.0        | 120.0          | 120.0          |
| Performance | 120.0       | 140.0          | 120.0          |

Example on setting the power limit to static 100W with no boost:  
`ryzenadj --stapm-limit=100000 --fast-limit=100000 --slow-limit=100000`

**Note that changing the power mode via the EC controller (using `ec-su_axb35-linux` or otherwise) resets these values to their defaults.**

### Relevant Pages
 - [[Guides/Hardware-Monitoring]]
 - [[Guides/Power-Modes-and-Performance]]
