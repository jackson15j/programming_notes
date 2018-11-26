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


[StackOverflow: Tramp ssh + sudo]: https://stackoverflow.com/questions/2177687/open-file-via-ssh-and-sudo-with-emacs
[StackOverflow: Tramp with Public Key]: https://stackoverflow.com/questions/1353297/editing-remote-files-with-emacs-using-public-key-authentication
