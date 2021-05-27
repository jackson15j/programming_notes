Windows Notes
=============

* [ERRORLEVEL (exit codes)] - Windows is a bit inconsistent with exit codes
  compared to Linux. Not all built-ins set exit codes. Can deal with this like
  this:

  ```bat
  if exist /path/to/delete rmdir /path/to/delete
  ```
* Scan ports: `netstat –ano ¦find /i “listening”`.


[ERRORLEVEL (exit codes)]: https://ss64.com/nt/errorlevel.html
