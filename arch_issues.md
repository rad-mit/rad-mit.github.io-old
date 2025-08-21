## "Anything that can go wrong will wrong": Arch Linux Edition of Murphy's Law

16th June, 2025

Having experimented with various Linux distros, starting from Debian-based Ubuntu and Mint to CentOS for PCs, RPiOS, L4T for developer boards such as Jetson Nano, I finally decided to start using Arch Linux (and no, I did not do it just to say I use arch, btw ;) ). I had thought basic installation would be similar to what I had worked with before, and barely expected to get stuck with it. But, Arch surprised me right from the start, and I had to encounter issues I hadn't seen earlier, which made me want to document them so that I don't forget the fixes, and for anyone else who might be on the same path as me.

As of now, I have a barebones CLI installation of Arch on my PC with a single root user and WiFi access which I can access from my GRUB menu during bootup, so this post covers every issue I had until then. I am yet to start ricing and make it usable for my daily tasks. Whenever I do that, there will be a part 2 to this blog post :) But until then, even just the installation was pretty fulfilling in terms of learning stuff I've never had to bother much about in the past (which is how Arch is so uniquely customizable), so highly recommended!

I found the [OG Arch Wiki guide for Installation](https://wiki.archlinux.org/title/Installation_guide) as consistently the most useful resource, apart from a couple of insightful reddit posts, which I will link below.

### USB Flashing and Boot into Arch
The first step was to download the latest ISO image from the [Downloads Page](https://archlinux.org/download/), verifying its checksum and signatures, and then creating a "live" Arch Installer USB stick as per instructions given [here](https://wiki.archlinux.org/title/USB_flash_installation_medium). 

#### Issue 1: Media not found while booting using the live USB
I used the ```cat``` method from my Ubuntu CLI and rebooted my PC to access the UEFI settings. Once there, I selected my USB drive in the boot menu. I was returned to the UEFI menu, with the message ```media not found```. I then returned to my Ubuntu, and tried with ```cp```, ```dd```, ```tee``` and [rufus](http://rufus.ie/en/) as well, but had the same issue each time. Turns out, using any kind of UI to eject the USB after flashing it with the ISO image would not work. I had to only unmount it with ```sudo umount <usb_id_from_lsblk_command>```. I had just tried this on a whim, as I couldn't find any solution online and was starting to wonder if I couldn't use my 7 years old USB anymore.

2. I have a GRUB setup, and when my laptop starts up, I see a message ```booting in insecure mode``` before the GRUB menu appears. But it was still enabled 

### cat /sys/firmware/efi/fw_platform_size
If the command returns 64, the system is booted in UEFI mode and has a 64-bit x64 UEFI.



   

<!-- **Project description:** Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### 1. Suggest hypotheses about the causes of observed phenomena

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

```javascript
if (isAwesome){
  return true
}
```

### 2. Assess assumptions on which statistical inference will be based

```javascript
if (isAwesome){
  return true
}
```

### 3. Support the selection of appropriate statistical tools and techniques

<img src="images/dummy_thumbnail.jpg?raw=true"/>

### 4. Provide a basis for further data collection through surveys or experiments

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/). -->
