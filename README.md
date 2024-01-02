# pi-gamez
Play games using the pi and old game controllers
Note that the setup is not necessarily limited by the compute of the pi.

RPi currently being used is a RPi 0 W

Motivation: want to use a wii u controller for drone games such as LiftOff, so that I don't have to take my controller back home

---
A couple of goals help set the direction of this project:
- Read controller input from Pi
  - [x] Wiimote inputs using the BT on a Pi 0 W
  - [ ] Unsuccesful for Wii U Pro Controller (tried on Pi 0 W and old Dell Inspiron laptop)
- [ ] Setup Pi as a HID
  - [ ] Setup as keyboard --> Make it configurable for the user
  - [ ] Setup as joystick to be read as a general controller to steam
- [ ] Setup Steam Remote Link using RPi 0 2 W
  - Steam [Remote Play](https://help.steampowered.com/en/faqs/view/0689-74B8-92AC-10F2)
- [ ] Send music from Pi to Wiimote
- [ ] Play a steam game
  - [Stick Fight](https://store.steampowered.com/app/674940/Stick_Fight_The_Game/) is a good [local co-op game](https://store.steampowered.com/tags/en/Local%20Co-Op)
- [ ] Get a good setup with a controller that can be used for a drone sim


---
## Connecting with Steam


### Use Pi0 2 W for Steam Remote Play

[Steam remote play](https://help.steampowered.com/en/faqs/view/0689-74B8-92AC-10F2) requires [Steam Link](https://store.steampowered.com/app/353380/Steam_Link/) on the local device. There's a [Reddit](https://www.reddit.com/r/RetroPie/comments/rw8o7u/steam_link_on_pi_zero_2_w/) thread setting this up. Steam also has a [page](https://help.steampowered.com/en/faqs/view/6424-467A-31D9-C6CB) on connecting the RPi to the Steam Link App. This can be setup on a Pi 0 W 2 as per [this site](https://picockpit.com/raspberry-pi/fun-projects-to-do-with-the-raspberry-pi-zero-2/#Steamlink_on_Raspberry_Pi_Zero_2_W) by installing:

```
sudo apt install steamlink
```
A couple caveats:
- Use Debian Buster (with GUI?)
- Increase GPU Memory to 128 MB using [this article](https://support.optisigns.com/hc/en-us/articles/1500010436341-Adjust-the-GPU-memory-on-the-Raspberry-Pi)


---
## Connecting Wii Controller(s)
### Progress so far

As per the [xwiimote](https://dvdhrm.github.io/xwiimote/) wiki, the linux kernel has drivers installed that support these devices:
- :heavy_check_mark: Nintendo Wii Remote (Nintendo RVL-CNT-01)
- :black_square_button: Nintendo Wii Nunchuk Extension
- :black_square_button: Nintendo Wii Balance Board (Nintendo RVL-WBC-01)
- :x: Nintendo Wii U Pro Controller (Nintendo RVL-CNT-01-UC)
- :heavy_minus_sign: Nintendo Wii Remote Plus (Nintendo RVL-CNT-01-TR)
- :heavy_minus_sign: Nintendo Wii Classic Controller Extension
- :heavy_minus_sign: Nintendo Wii Classic Controller Pro Extension
- :heavy_minus_sign: Nintendo Wii Guitar Extensions
- :heavy_minus_sign: Nintendo Wii Drums Extensions

It is recommended that any development is not done using the Kernel driver, but rather in user space using APIs exposing the Kernel. A couple of libraries are documented on [Wiibrew](https://wiibrew.org/wiki/Wiimote/Library). The main methods I believe are:
- [xwiimote](https://github.com/dvdhrm/xwiimote) github
  - The API is documented [here](http://dvdhrm.github.io/xwiimote/api/)
  - TODO: Note that a couple extra packages had to be installed before the xwiimote package was installed. These include: `libudev-dev`, `libtool`, `libncurses5-dev`, `libncursesw5-dev`
- [xwiimote-bindings](https://github.com/dvdhrm/xwiimote-bindings) package that provides APIs for Perl or Python. 


### Linux vs Windows

- Most controllers connect over bluetooth (BT). The BT stack required for this is built in to Linux, but not in to Windows. There are some options for Windows, which I haven't tested yet because they manipulate the BT driver directly:
  - [WiinUPro](https://github.com/KeyPuncher/WiinUPro/releases) and some more info on [their site](https://sites.google.com/site/wiinupro/home?authuser=0)
  - TeHaxor69 realsed some SW on [this GBATEMP thread](https://gbatemp.net/threads/wii-u-pro-controller-to-pc-program-release.343159/)
  - [This reddit post](https://www.reddit.com/r/wiiu/comments/3bzdx0/wii_u_pro_controller_as_xbox_controller_on/) uses the Toshiba driver. 

### Wii U Pro Controller

I have been unable to connect my Wii U Pro Controller. The internet appears not to have a good response to this issue (yet?). I suspect this may be a BL stack issue, considering that all controllers can connect to the Wii U normally. It could also be a HW issue, considering that neither the pi nor my linux laptop could connect to it. Other people also had a similar issues:
    - [Wii U Pro controller was not detected](https://forum.manjaro.org/t/wii-u-pro-controller-is-not-detected-by-bluetooth/54420)
    - Some people found that there was an issue connecting the Wii U Pro Controller after an update [here](https://www.linux.org/threads/solved-cannot-connect-wii-u-pro-controller-after-an-update.33396/)
    - [Guide getting a Wii U Pro Controller to work](https://www.linuxquestions.org/questions/slackware-14/guide-getting-a-wii-u-pro-controller-to-work-4175576590/). 
    Although I haven't tried it, it doesn't look like it will work, because it assumes that the BL connection works, which it didn't for me.
    - [Using Wii U Pro Controller with adaptor reddit thread](https://www.reddit.com/r/linux_gaming/comments/6ce02l/using_the_wii_u_pro_controller_with_adaptor_on/)
### Other Nintendo Controllers

- Note that it also seems that a Switch controller can't be connected yet as mentioned on this [reddit thread](https://www.reddit.com/r/linux_gaming/comments/98xkt9/wiiu_pro_controllers_dont_work/). 
- However [this reddit thread](https://www.reddit.com/r/wiiu/comments/f5cn77/wii_u_pro_controller_in_linux/) suggests that there is a `hid-nintendo` driver which can connect to the switch controller. 
- The last resort is to use the Mayflash adpater which can connect to any remote

---
### Asides

- [Blueman versions](https://repology.org/project/blueman/versions)
- [wiimote driver src code](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/drivers/hid/hid-wiimote-core.c)
- [Ubuntu Forums](https://ubuntuforums.org/showthread.php?t=2159885)
- [Outdoor Target Positioning Using Wii Remote IR Camera and Signal Modulation](https://www.mdpi.com/1424-8220/20/8/2163)
- [3D Spatial Interaction with the Wii Remote for Head-Mounted Display Virtual Reality](https://ro.uow.edu.au/cgi/viewcontent.cgi?article=10591&context=infopapers#:~:text=This%20provided%20spatial%203D%20tracking,a%20number%20of%20other%20ways.)
- Emulating a Gaming Device - Raspberry Pi Zero , may require writing your own HID driver
- [USB Gadget Configfs](https://elinux.org/images/e/ef/USB_Gadget_Configfs_API_0.pdf)
- Sending data from RPi sensor over Serial-Bluetooth [article](https://towardsdatascience.com/sending-data-from-a-raspberry-pi-sensor-unit-over-serial-bluetooth-f9063f3447af)
- Article on setting up steam link on rpi [here](https://www.tomshardware.com/how-to/steam-link-raspberry-pi)
- [Steamworks documentation](https://partner.steamgames.com/doc/features/steam_controller/device) on different input devices
- RPi as a [gamepad](https://opensource.com/article/19/3/gamepad-raspberry-pi) and how to connect to an xbox [stackoverflow post](https://stackoverflow.com/questions/69094678/how-can-i-write-xbox-controller-inputs-over-a-usb-with-raspberry-pi)
- Emulating a gaming device [stackflow post](https://stackoverflow.com/questions/49139136/emulate-a-gaming-device-raspberry-pi-zero) 
- Composite USB Gadgets on the rpi zero [article](https://www.isticktoit.net/?p=1383) and a [gist](https://git.gir.st/hardpass.git/blob/HEAD:/init_usb.sh)
- 
