Bash
====

Tmux
----

* [Github: Tmux].
* Reconnect to a socket: `tmux -S </path/to/socket> attach`.
* If the above doesn't work, you can recreate the socket: `kill -s USR1 <pid>`,
  and then try again. Search for `SIGUSR1` in the tmux man page.

Ladder Diagrams
---------------

* [mscgen] - render ladder diagrams from text (`*.msc`) files.
* [mscgen js] - Javascript Rendering Webpage + Example files..

Remote Desktop (RDP)
--------------------

* RDP to system on a Windows Domain:
  `xfreerdp /w:1920 /h:1080 /u:<username> /d:<domain> /v:<host>`

[Github: Tmux]: https://github.com/tmux/tmux

[mscgen]: http://www.mcternan.me.uk/mscgen/
[mscgen js]: https://mscgen.js.org
