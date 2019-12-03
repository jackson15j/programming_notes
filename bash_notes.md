Bash Notes
==========

Dump of Bash notes and one-liners.


ffmpeg
------

* [FFMPEG:Convert video files to GIF]:

```bash
 ffmpeg -y -ss <secs_to_start_from> -t <duration_in_secs> -i <source_video> -vf fps=10,scale=320:-1:flags=lanczos,palettegen palette.png && ffmpeg -ss <secs_to_start_from> -t <duration_in_secs> -i <source_video> -i palette.png -filter_complex "fps=10,scale=320:-1:flags=lanczos[x];[x][1:v]paletteuse" <output.gif>
 ffmpeg -y -ss 8 -t 4 -i P1020397.MP4 -vf fps=10,scale=320:-1:flags=lanczos,palettegen palette.png && ffmpeg -ss 8 -t 4 -i P1020397.MP4 -i palette.png -filter_complex "fps=10,scale=320:-1:flags=lanczos[x];[x][1:v]paletteuse" output4.gif
```

INVESTIGATE
===========

* [Linux Extended BPF (eBPF) Tracing Tools] - eBPF (Kerkeley Packet Filter) is
  used to trace latency in Kernel and network packets.

Luks - Fixing slow full disk encryption
=======================================

Currently using [Manjaro], which has slow full disk encryption. This appears to
be because:

* Disk is encrypted during installation, using OS loaded AES libraries.
* OS code runs fast, so creates iterations in the millions to delay brute force
  attacks.
* [Manjaro] sets 2 keys per-partition (instead of 1).
* On boot. Grub loads AES libraries to decrypt.
* Grub's decryption is astronomically slower than at the OS level.
* Decryption time is magnified by the number of keys across each partition that
  needs to be read to load the Kernel (`/`, `/swap`).
* Decryption is in the high seconds, to minutes.

Fix:

* Reduce number of keys per-partition.
* Reduce iterations.
    * **NOTE:** security risk due to reduced brute force time. However, if
      attacker has physical access, they will remove the drive and brute force
      in a faster system.
* Decryption within seconds.
* See: [linux-blog: slow grub luks decryption].
* Steps:

  ```bash
  # Get current iterations and key slots used:
  sudo cryptsetup luksDump /dev/<partition>
  # Add a quick to decrypt safety key:
  sudo cryptsetup luksAddKey /dev/<partition> -S 7 --pbkdf-force-iterations 200000
  # Delete Slot 0 key:
  sudo cryptsetup luksKillSlot /dev/<partition> 0
  # Re-Add Slot 0 key with a smaller iteration:
  sudo cryptsetup luksAddKey /dev/<partition> -S 0 --pbkdf-force-iterations 200000
  # Repeat for remaining partitions and then clean up unused keys.
  # Reboot after changing keys to verify time to desktop from drive decryption.
  ```

[FFMPEG: Convert video files to GIF]: https://superuser.com/questions/556029/how-do-i-convert-a-video-to-gif-using-ffmpeg-with-reasonable-quality#556031
[Linux Extended BPF (eBPF) Tracing Tools]: http://www.brendangregg.com/ebpf.html
[Manjaro]: https://manjaro.org/get-manjaro/
[linux-blog: slow grub luks decryption]: https://linux-blog.anracom.com/2018/11/30/full-encryption-with-luks-sha512-aes-xts-plain64-grub2-really-slow/
