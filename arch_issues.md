# "Anything That Can Go Wrong Will Go Wrong": Arch Linux Edition of Murphy's Law

**16th June, 2025**

Having worked with various Linux distros, from Debian-based Ubuntu and Mint to CentOS for PCs, RPiOS, and NVIDIA’s L4T for Jetson Nano, I finally decided to start using Arch Linux (And no, not *just* so I can say “I use Arch, btw” 😉), something I have been wanting to do for years.

I assumed installation would be similar to what I’d done before and didn’t expect to get stuck early on. But Arch had other plans. I ran into issues I had never faced before, and in true Murphy’s Law fashion, almost *anything that could go wrong did go wrong*, enough that I decided to document them here. Hopefully, this helps future me (and anyone else walking down this path) remember the fixes.  

At the time of writing, I have a barebones CLI installation of Arch running on my PC with a single root user and WiFi access, bootable from GRUB. This post covers everything up to that point. I haven’t yet started ricing or making it suitable for daily use, that’ll be a part 2 of this post. Still, even just the installation has been a fulfilling learning experience. Arch forces you to understand things I’d previously taken for granted, and that’s what makes it so uniquely customizable. Highly recommended!  

The [Arch Wiki Installation Guide](https://wiki.archlinux.org/title/Installation_guide) was consistently my most useful reference, with a few insightful community posts, links to which I will add as in relevant sections.  

---

## 1. USB Flashing

The first step was straightforward: download the latest ISO from the [official downloads page](https://archlinux.org/download/), verify the checksum and signatures, and then create a “live” Arch installer USB. Instructions for this are on the [Arch Wiki](https://wiki.archlinux.org/title/USB_flash_installation_medium).  

### Issue 1: *“Media Not Found”* Error When Booting from Live USB  
Initially, I flashed the ISO using the `cat` method on Ubuntu CLI and tried booting via UEFI. When I selected the USB medium in the boot menu, I was returned to the UEFI menu with a *“media not found”* error.  

I retried with `cp`, `dd`, `tee`, and even [Rufus](http://rufus.ie/en/). Same result each time.  

The fix? **Don’t eject the USB using a file manager or UI.** That silently breaks something in the boot media. Instead, only unmount it with:  

```bash
sudo umount <usb_id_from_lsblk_command>
```

This worked and upon some research, I think that the issue comes from the ISO being cached in memory rather than fully written until the unmount happens, as explained in the answer [here](https://www.linux.org/threads/unmount-vs-eject.27273/). I had almost blamed my 7-year-old USB stick before stumbling on this fix.

## 2. Booting into Arch from live USB, Basic Setup and Package Installation

### Issue 2: Secure Boot Still Enabled

Now when I tried to boot into the live USB, the system complained about Secure Boot being enabled.

Although I clearly remembered turning Secure Boot off long ago and my PC also displays a “booting in insecure mode” message every time it powers on, before the GRUB boot menu is displayed.

It turns out that the "booting in insecure mode" message only means that validation in the Machine Owner Key module is switched off, not secure boot, as explained [here](https://askubuntu.com/questions/726052/ubuntu-booting-in-insecure-mode-with-secureboot-enabled).

So, to check, I went back into the firmware settings and it was actually enabled. I switched it off again, and this time, finally Arch booted fine.


Once all this was fixed, I continued to follow the installation guide until the point where all the partitions are created and mounted, the root user is created, essential packages are installed and the system needs to be rebooted after removing the live USB (Section 4 of the [Installation Guide](https://wiki.archlinux.org/title/Installation_guide)).

## 3. First Boot into Installed Arch and Login with Root User

### Issue 3: Missing Boot Entry for Arch in GRUB menu
Once I rebooted my PC after the install and removing the live USB, I could not find an entry in the GRUB menu for Arch. Running

```bash
sudo os-prober
```

from Ubuntu CLI, identified Windows, Ubuntu and Majaro, but not Arch. The community answers suggested mounting the / (root) partition of Arch and then trying to update GRUB. So, after the following steps

```bash
sudo mount <arch_root_partition> /mnt
sudo update-grub
```
and rebooting, I could find an entry for Arch in the GRUB menu, and could access its CLI as well.

Some helpful articles that explain GRUB, and adding Arch to an existing GRUB installation:
- [Some info on GRUB 2](https://www.rodsbooks.com/efi-bootloaders/grub2.html)
- [Someone with the same problem as me](https://superuser.com/questions/1643965/trying-to-add-arch-linux-to-a-uefi-hard-drive-with-ubuntu-debian-and-grub-alre)
- [This one actually suggests the mount fix](https://bbs.archlinux.org/viewtopic.php?id=276281)
- [GRUB on Arch Wiki](https://wiki.archlinux.org/title/GRUB)


## Conclusion
Arch is already equal parts frustrating and rewarding for me, but that’s also what makes it worth using. And this is just the beginning. Stay tuned for part 2, when I finally get around to ricing and turning this barebones install into a usable daily driver.
