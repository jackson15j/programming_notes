Arch bad upgrade on encrypted partition fix
===========================================

Had an issue where a bad Kernel update during `pacman -Syu` resulted in the
system crashing partway through. On subsequent reboots the keyboard no longer
worked at the point of password entry to unlock the root partition.

This is how I fixed this problem.

Fix steps
---------

* Boot from a liveusb (used [Manjaro]) & ensure network connectivity.
* `fdisk -l /dev/<drive>` - to get partition layout.
* `cryptsetup luksOpen /dev/<root_partition> root` - unlock the root partition.
* `mkdir /mnt/x`.
* `mount /dev/mapper/root /mnt/x`.
* `<arch-chroot>/<manjaro-chroot> /mnt/x` - chroot into the root
  partition. Using the OS `chroot` alias, since it configures appropriate
  aliases (eg. `/etc/mtab`, etc...).
* Copy liveusb's `/etc/resolve.conf` into the chroot to enable network
  connectivity.
* `sudo pacman -Sy linux` - update to the latest kernel.
* Quit the chroot.
* _Potentially:_ fix up `ldconfig` issues: [Arch Wiki: Kernel Panics].
    * If errors, check files are used: `pacman -Qo /usr/lib/<lib*.so.*>`
* `mkdir /mnt/boot`.
* `mount /dev/<boot_partition> /mnt/boot`.
* `cp /mnt/x/boot/* /mnt/boot/` - Copy the new kernel to the boot partition (if
  you have a separate boot partition.

If you're having issues with broken packages, a quick and dirty way to
reinstall packages is:

```bash
pacman -Qnq | sudo pacman -S -
```

Link Dump
---------

* [Manjaro].
* [ArchLinux Forum: Kernel update + encrypted root won't boot].
* [Arch Wiki: Kernel Panics].
* [Arch Wiki: Pacman].
* [ArchLinux Forum: pacman -Syu "exists in filesystem" errors from qt4].
* [Arch Wiki: Pacman - Failed to commit transaction. Conflicting files error].
* [Arch Forum: /sbin/ldconfig: File /usr/lib/libhd.so is empty, not checked.].


[Manjaro]: https://manjaro.org/get-manjaro/
[ArchLinux Forum: Kernel update + encrypted root won't boot]: https://bbs.archlinux.org/viewtopic.php?id=60173
[Arch Wiki: Kernel Panics]: https://wiki.archlinux.org/index.php/General_troubleshooting#Kernel_panics
[Arch Wiki: Pacman]: https://wiki.archlinux.org/index.php/Pacman
[ArchLinux Forum: pacman -Syu "exists in filesystem" errors from qt4]: https://bbs.archlinux.org/viewtopic.php?id=221910
[Arch Wiki: Pacman - Failed to commit transaction. Conflicting files error]: https://wiki.archlinux.org/index.php/Pacman#.22Failed_to_commit_transaction_.28conflicting_files.29.22_error
[Arch Forum: /sbin/ldconfig: File /usr/lib/libhd.so is empty, not checked.]: https://bbs.archlinux.org/viewtopic.php?id=73452
