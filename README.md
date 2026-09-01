# Asus-pro-art-P16-on-linux
I am using a pro art with a 5080 and a r9, this is not applicable to ones with NVIDIA spark.

Some notes on using the Pro art P16 on Linux, assuming your fine with just using the iGPU and the touchpad dile: There is nothing more to report, the laptop works exactly as you would on windows. If your on battery you will only notice the better battery life  with me getting a max of 12h active doing office work and coding and a max of 23h keeping it in sleep. Plugged in is a different story with the GPU issues being a large problem, the dGPU is not detected keeping you on the iGPU. This is due to issues with NVIDIA and Asus drivers out of the box, with the fix explained later. Assuming you don't mind that large issue install Linux and you'll have a good time, otherwise keep reading for more info on getting everything working as it should.

# Tested distos

This is mostly for fedora with gnome (Don't use KDE it is shit) however I have installed openSUSE and arch to different levels of success. OpenSUSE is mostly the same as fedora however it is not as smooth sailing but if your ok with reading more documentation and diagnosing issues it is mostly the same just more time involved. Arch is very hit or miss it worked but it didn't after a update, programs stoped working, configs reset, resuluton issues etc. Some of my friends that daily arch say that it is mostly me being new to arch and a skill issue not the laptop, so I'll conclude that if your experienced with arch or using a arch based distro you will be fine. Other distros tell me and ill update this repo, same for other laptops make a new .md with your expirance.

## Before starting 

Update your kernel and all the other updates, I recommend doing this as the first step post install as if you fuck up, it is easy to fix (re-install). You can in theory just copy all the commands in order from this page however please don't, I don't need that pressure and the original documentation goes into far more detail in-case you want something different to my config.

## Nvidia drivers

NVIDIA as normal is the main issue when it comes to using Linux on this laptop. Drivers are inconstant and can break however this link is very helpful on installing a more stable version (for fedora), however I must stress if you have issues DISABLE SECURE BOOT. Secure boot is designed for windows even tho fedora has secure boot and it works well it will sabotage GPU switching on the pro art, try with both as it is better security wise to have secure boot but have the compatibility issues in mind.

Regardless here are the commands you need exacted for essayer reading as the main source is abit confusing **BUT read the documentation if you dont know what your doing or if your using secure boot**.

```
sudo dnf update -y # and reboot if you are not on the latest kernel
```

```
sudo dnf install akmod-nvidia # rhel/centos users can use kmod-nvidia instead
```

```
sudo dnf install xorg-x11-drv-nvidia-cuda #optional for cuda/nvdec/nvenc support
```
Wait 30 minutes to allow for the drivers to install correctly (after you see the complete message) or use to identify if it is installed ``` modinfo -F version nvidia ``` 

```
systemctl reboot
```

Source: https://rpmfusion.org/Howto/NVIDIA

For secure boot: https://rpmfusion.org/Howto/Secure%20Boot

## Other Asus drivers

Drivers needed for full control of your GPU switching and some other stuff.

### Supergfxctl

This is for ROG laptops really, the creator says that this is compatible with pro art just not officially and I tend to agree. It dose have some issues but that is very minor and doesn't affect the underlying job it has to do.
```
sudo dnf copr enable lukenukem/asus-linux
```

```
sudo dnf install supergfxctl
```

```
sudo systemctl start supergfxd.service
sudo systemctl enable supergfxd.service
```

### Asusctl 
Controls other futons needed such as fan curves and power levels, there is also a optional GUI control panel you can install it if you want i am not including it as I don't feel it is needed.
```
sudo dnf install asusctl 
```

```
sudo systemctl start asusd.service 
sudo systemctl enable asusd.service
```

Source for Supergfxctl and Asusctl: https://asus-linux.org/

### Gnome GPU switcher 
This makes it essayer to switch betwen iGPU and dGPU without using the terminal, this is not needed but a nice to have. No real need for a command just use the gnome website as if your not using gnome just skip as you cant use it ether way.

https://extensions.gnome.org/extension/7018/gpu-supergfxctl-switch/

### Touchpad drivers  

The touch pad is not a pressing issue for most but may as well get the functionality you pay for, I found only one driver for this however it dose have some performance issues by Linux standers but if your coming from the widows drivers by Asus you'll be happy with it. 

First run - installs system packages
```
$ INSTALL_DIR_PATH="/home/$USER/.local/share/asus-dialpad-driver" \
  INSTALL_UDEV_DIR_PATH="/etc/udev" \
  bash install.sh
```
Reboot when prompted (required for package layering)
```
$ systemctl reboot
```
Second run - completes driver installation
```
$ INSTALL_DIR_PATH="/home/$USER/.local/share/asus-dialpad-driver" \
  INSTALL_UDEV_DIR_PATH="/etc/udev" \
  bash install.sh
```
Final reboot (required for group membership)
```
$ systemctl reboot
```
Source: https://github.com/asus-linux-drivers/asus-dialpad-driver

## Bugs 

Here is a list of sources for fixing issues that you might see.

https://gitlab.com/asus-linux/asusctl/-/work_items/555

## Disclaimer 

I am not responsible for anything you do based on my advice, you are responsible to do your own research and to know what your doing to your computer. There is a high chance that this is out dated or wrong by the tine you read this, I have provided sources for that reson, If anything dose go wrong that is on you to solve,. However I do encourage  you to come back with findings and help make this a better source for others. 

This repository is in the public domain do whatever you want with it however, the sources linked have no affiliation with me please check their licensees.

Sincerely, 

-海


