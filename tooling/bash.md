Bash
====

Tmux
----

* [Github: Tmux].
* Reconnect to a socket: `tmux -S </path/to/socket> attach`.
* If the above doesn't work, you can recreate the socket: `kill -s USR1 <pid>`,
  and then try again. Search for `SIGUSR1` in the tmux man page.


[Github: Tmux]: https://github.com/tmux/tmux
