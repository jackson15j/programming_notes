Git Notes
=========

* Move file, with history, to a new repo
 ([StackOverflow: Move files between Git repos preserving history]):

```bash
git log --pretty=email --patch-with-stat --reverse -- path/to/file_or_folder | (cd /path/to/new_repository && git am)
```



[StackOverflow: Move files between Git repos preserving history]: https://stackoverflow.com/questions/1365541/how-to-move-files-from-one-git-repo-to-another-not-a-clone-preserving-history#11426261
