Emacs Notes
===========

* [StackOverflow: Tramp ssh + sudo]:
    * `Ctrl+x f`.
    * `/ssh:<user>@<host>|sudo:<host>:/path/to/file`.
* [StackOverflow: Tramp with Public Key]:
    * Can't do: `ssh -i /path/to/ssh_key.pem <user>@<host>` in emacs. Instead
      you can use `~/.ssh/config` settings.

      ```bash
      Host remotehost
          User user
          Port 22
          Hostname remotehost.fqdn
          IdentityFile ~/.ssh/ssh_key.file
      ```

    * Can now do `ssh remotehost` or tramp: `/ssh:remotehost:/path/to/file`.
* [ErgoEmacs: Find Replace in directories] / [GNU Emacs: Replacing text across
  multiple files]:
    * `M-x find-name-dired`, enter filename wildcard.
    * Mark `m` files, then press `Q`.
    * `<find> regex` return, `<replace> string` return.
    * Confirm/deny replace with the usual: `!`, `y`, `n`.

[StackOverflow: Tramp ssh + sudo]: https://stackoverflow.com/questions/2177687/open-file-via-ssh-and-sudo-with-emacs
[StackOverflow: Tramp with Public Key]: https://stackoverflow.com/questions/1353297/editing-remote-files-with-emacs-using-public-key-authentication
[ErgoEmacs: Find Replace in directories]: http://ergoemacs.org/emacs/find_replace_inter.html
[GNU Emacs: Replacing text across multiple files]: https://www.gnu.org/software/emacs/manual/html_node/efaq/Replacing-text-across-multiple-files.html
