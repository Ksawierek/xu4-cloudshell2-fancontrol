# xu4-cloudshell2-fancontrol with RAID monitoring

This is a fork from [xu4-cloudshell2-fancontrol](https://github.com/crazyquark/xu4-cloudshell2-fancontrol) that was modified/combined with a script to display RAID Info and HDD Temperatures (modified from [this](https://forum.odroid.com/viewtopic.php?f=147&t=29298#p209432)).
It is possible to set temperature thresholds for CPU and HDD where the fan will be turned on.

# Notes

- this script was tested on Odroid Cloudshell 2 with XU4
- my RAID is encrypted and mounted to /dev/mapper/secure: in case you have multiple Harddrives, you may have to modify the reference to Harddrives for Temperature monitoring
- you can manually enable/disable the lcd-script by executing `systemctl stop cloudshell-lcd` and `systemctl start cloudshell-lcd`

# Installation

- follow [these](https://wiki.odroid.com/accessory/add-on_boards/xu4_cloudshell2/xu4_cloudshell2#enable_lcd_and_fan) steps to install the original lcd-service from Odroid, e.g.:
    ```sh
    odroid@odroid:~$ sudo apt-get update
    odroid@odroid:~$ sudo apt-get install odroid-cloudshell cloudshell2-fan
    odroid@odroid:~$ sudo reboot
    ```
    ([here](http://bazaar.launchpad.net/~kyle1117/+junk/cloudshell-lcd/view/head:/bin/cloudshell-lcd) is the link to the current script)
- overwrite the `/bin/cloudshell-lcd` and  `/bin/checkRAID.bash/` with the content in this repo
- optionally, edit the file `cloudshell-lcd.service` in `/lib/systemd/system` and add two lines in [Service] section:
```sh
ExecStart=/bin/cloudshell-lcd
ExecStopPost=/usr/sbin/i2cset -y 1 0x60 0x05 0x00
```
This will enable the fan whenever the cloudshell-lcd.service stops. To prevent the fan to be enabled on system-shutdown, it is also necessary to copy the file `shutdown-fan` to `/lib/systemd/system-shutdown/`
- see [this](https://forum.odroid.com/viewtopic.php?t=29298#p209064) topic for more information

# Example Screen

![Example LCD Screen](/resources/xu4cs2-fanlcd.jpg#2?raw=true)
