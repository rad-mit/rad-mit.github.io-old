# "Anything That Can Go Wrong Will Go Wrong": Arch Linux Edition of Murphy's Law

**16th June, 2025**

Having worked with various Linux distros, from Debian-based Ubuntu and Mint to CentOS for PCs, RPiOS, and NVIDIA’s L4T for Jetson Nano, I finally decided to start using Arch Linux (And no, not *just* so I can say “I use Arch, btw” 😉), something I have been wanting to do for years.

I assumed installation would be similar to what I’d done before and didn’t expect to get stuck early on. But Arch had other plans. I ran into issues I had never faced before, and in true Murphy’s Law fashion, *anything that could go wrong did go wrong*, enough that I decided to document them here. Hopefully, this helps future me (and anyone else walking down this path) remember the fixes.  

At the time of writing, I have a barebones CLI installation of Arch running on my PC with a single root user and WiFi access, bootable from GRUB. This post covers everything up to that point. I haven’t yet started ricing or making it suitable for daily use, that’ll be a part 2 of this post. Still, even just the installation has been a fulfilling learning experience. Arch forces you to understand things I’d previously taken for granted, and that’s what makes it so uniquely customizable. Highly recommended!  

The [Arch Wiki Installation Guide](https://wiki.archlinux.org/title/Installation_guide) was consistently my most useful reference, with a few insightful Reddit posts, links to which I will add as we move along.  

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

## 2. Booting into Arch
### Issue 2: Secure Boot Still Enabled

Now when I tried to boot into the live USB, the system complained about Secure Boot being enabled.

Although I clearly remembered turning Secure Boot off long back and my PC also shows the “booting in insecure mode” message every time it powers on, before the GRUB boot menu is displayed.

So, to check, I went back into the firmware settings and it was actually enabled. I switched it off again, and this time, finally Arch booted fine.


Arch is equal parts frustrating and rewarding—but that’s also what makes it worth using. And this is just the beginning. Stay tuned for part 2, when I finally get around to ricing and turning this barebones install into a usable daily driver.
