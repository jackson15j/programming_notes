Git Notes
=========

* Move file, with history, to a new repo
 ([StackOverflow: Move files between Git repos preserving history]):

  ```bash
  git log --pretty=email --patch-with-stat --reverse --binary -- path/to/file_or_folder | (cd /path/to/new_repository && git am)
  ```

* [StackOverflow: Get the last commit hash from a remote repo without cloning].

  ```bash
  # Get last commit for default branch:
  git ls-remote <url> HEAD | awk '{print $1}'
  # Get short last commit for branch:
  git ls-remote <url> refs/heads/<branch> | awk '{print $1}' | cut -c 1-7 -
  ```

* [StackOVerflow: How to blame a deleted file in git?].

  ```bash
  git log -2 --oneline -- /path/to/file
  ```


[StackOverflow: Move files between Git repos preserving history]: https://stackoverflow.com/questions/1365541/how-to-move-files-from-one-git-repo-to-another-not-a-clone-preserving-history#11426261
[StackOverflow: Get the last commit hash from a remote repo without cloning]: https://stackoverflow.com/questions/24750215/getting-the-last-commit-hash-from-a-remote-repo-without-cloning
[StackOVerflow: How to blame a deleted file in git?]: https://stackoverflow.com/questions/37084243/how-to-blame-a-deleted-file-in-git
