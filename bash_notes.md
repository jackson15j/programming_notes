Bash Notes
==========

Dump of Bash notes and one-liners.


Tmux
----

* [Github: Tmux].
* Reconnect to a socket: `tmux -S </path/to/socket> attach`.
* If the above doesn't work, you can recreate the socket: `kill -s USR1 <pid>`,
  and then try again. Search for `SIGUSR1` in the tmux man page.
* Save your tmux clipboard directly to a file: `Ctrl+b :`, `save-buffer
  <filepath>`.

Ladder Diagrams
---------------

* [mscgen] - render ladder diagrams from text (`*.msc`) files.
* [mscgen js] - Javascript Rendering Webpage + Example files..

Remote Desktop (RDP)
--------------------

* RDP to system on a Windows Domain:
  `xfreerdp /w:1920 /h:1080 /u:<username> /d:<domain> /v:<host>`

Disk Operations
---------------

* Nuke an SSD (Free alternative to: [Dban]) via a distro bootdisk: `blkdiscard
  -svz /dev/<drive>`. See: [blkdiscard discussion].

Rsync
-----

* [How to setup `rsync` daemon on Linux server].
    * Discover server `rsync` folders: `rsync -rdt rsync://<host>:873`.
    * Inspect folders: `rsync -rdt rsync://<host>:873/<folder>`.
* [How to use `rsync` to copy files to a server].
    * Rsync to rsync server: `rsync -vahP --inplace --no-whole-file --delete-delay --info=BACKUP,COPY,DEL,REMOVE,SKIP,STATS --log-file="rsync_to_nas_log.txt" <source> rsync://<user>@<host>:873/<folder>`
    * Quote and escape the folder path if there are spaces.
* [Backup to TrueNas over SSH].

SSH
---

* SSH Forward/Tunnel requests through a tunnel (hop) Host `ssh -f <user>@<host>
  -L <port>:<host>:<port> -N`.
* X-session in a Window: `startx -- /usr/bin/xepher -ac -screen <hxw> -reset :1d`.

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
Links:

* https://linux-blog.anracom.com/2018/11/30/full-encryption-with-luks-sha512-aes-xts-plain64-grub2-really-slow/
* https://forum.manjaro.org/t/decryption-problem/25284/17
* https://resources.infosecinstitute.com/luks-swap-root-boot-partitions/
* https://wiki.archlinux.org/index.php/Dm-crypt/System_configuration#cryptkey
* https://linux.die.net/man/8/cryptsetup
* https://linux.die.net/man/1/ecryptfs-setup-swap
* https://www.reddit.com/r/linuxquestions/comments/bjyvt7/luks_keyfile_not_working_no_key_available_with/
* https://evilshit.wordpress.com/2012/10/29/how-to-mount-luks-encrypted-partitions-manually/
* https://forum.proxmox.com/threads/luks-keyfile-not-working-no-key-available-with-this-passphrase.53935/


[Github: Tmux]: https://github.com/tmux/tmux

[mscgen]: http://www.mcternan.me.uk/mscgen/
[mscgen js]: https://mscgen.js.org
[Linux Extended BPF (eBPF) Tracing Tools]: http://www.brendangregg.com/ebpf.html
[Manjaro]: https://manjaro.org/get-manjaro/
[linux-blog: slow grub luks decryption]: https://linux-blog.anracom.com/2018/11/30/full-encryption-with-luks-sha512-aes-xts-plain64-grub2-really-slow/
[Dban]: https://dban.org
[blkdiscard discussion]: https://utcc.utoronto.ca/~cks/space/blog/linux/ErasingSSDsWithBlkdiscard?showcomments#comments
[How to setup `rsync` daemon on Linux server]: https://www.atlantic.net/vps-hosting/how-to-setup-rsync-daemon-linux-server/
[How to use `rsync` to copy files to a server]: https://www.atlantic.net/vps-hosting/how-to-use-rsync-copy-sync-files-servers/
[Backup to TrueNas over SSH]: https://www.truenas.com/community/threads/how-to-backup-files-to-truenas-with-rsync.90071/
