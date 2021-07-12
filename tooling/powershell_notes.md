PowerShell Notes
================

* Get file version from file (eg. `.exe`/`.dll`):
  `[System.Diagnostics.FileVersionInfo]::GetVersionInfo("somefilepath").FileVersion`
