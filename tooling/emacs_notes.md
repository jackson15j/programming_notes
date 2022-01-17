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
    * Mark `m` files (`t` marks all files), then press `Q`.
    * `<find> regex` return, `<replace> string` return.
    * Confirm/deny replace with the usual: `!`, `y`, `n`.
* [indent region], both setting keybindings for `indent-rigidly-<left|right>`
  and `C+x TAB`, then `S+<<left>|<right>>`.

Language Setup
==============


* [Github: jackson15j/dot_emacs] - personal Emacs dotfiles.
* [cpp_notes: Editor Config].


Link Dump
=========

* [John D Cook: Emacs on Windows].
* [Steve Losh: A Road to Common Lisp].
* [EmacsNotes: Highlight symbols].
* [Github: wagoodman/dive] - Docker management in emacs.


[StackOverflow: Tramp ssh + sudo]: https://stackoverflow.com/questions/2177687/open-file-via-ssh-and-sudo-with-emacs
[StackOverflow: Tramp with Public Key]: https://stackoverflow.com/questions/1353297/editing-remote-files-with-emacs-using-public-key-authentication
[ErgoEmacs: Find Replace in directories]: http://ergoemacs.org/emacs/find_replace_inter.html
[GNU Emacs: Replacing text across multiple files]: https://www.gnu.org/software/emacs/manual/html_node/efaq/Replacing-text-across-multiple-files.html
[indent region]: https://dougie.io/emacs/indent-selection/

[Github: jackson15j/dot_emacs]: https://github.com/jackson15j/dot_emacs
[cpp_notes: Editor Config]: ../cpp_notes.md#editor-config

[John D Cook: Emacs on Windows]: https://www.johndcook.com/blog/emacs_windows/
[Steve Losh: A Road to Common Lisp]: http://stevelosh.com/blog/2018/08/a-road-to-common-lisp/
[EmacsNotes: Highlight symbols]: https://emacsnotes.wordpress.com/2018/09/11/highlight-symbols-and-other-miscellanea-in-your-program-code/
[Github: wagoodman/dive]: https://github.com/wagoodman/dive
