PowerShell Notes
================

* Get file version from file (eg. `.exe`/`.dll`):
  `[System.Diagnostics.FileVersionInfo]::GetVersionInfo("somefilepath").FileVersion`
* Dump [Environment Variables]: `Get-Item -Path ENV:* | Format-Table -Wrap
  -AutoSize`.
* Avoid truncation: `<command> | Format-Table -Wrap -AutoSize`.

Windows
-------

* List Windows services: [Get-Service].

### Managing Windows IIS


* Viewing AppPool from AWS powershell connection:
    * Via [WebAdministration]:

      ```powershell
      PS C:\Windows\system32> Import-Module WebAdministration
      PS C:\Windows\system32> Get-ChildItem IIS:\AppPools\

      Name                     State        Applications
      ----                     -----        ------------
      DefaultAppPool           Started      Default Web Site
      r7AppPool                Stopped      r7WebSite


      PS C:\Windows\system32>
      ```

    * [IISAdministration]:

      ```powershell
      PS C:\Windows\system32> Import-Module IISAdministration
      PS C:\Windows\system32> get-iisapppool

      Name                 Status       CLR Ver  Pipeline Mode  Start Mode
      ----                 ------       -------  -------------  ----------
      DefaultAppPool       Started      v4.0     Integrated     OnDemand
      r7AppPool            Stopped               Integrated     OnDemand


      PS C:\Windows\system32>
      ```


[Environment Variables]: https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-7.2
[Get-Service]: https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-service?view=powershell-7.2
[WebAdministration]: https://docs.microsoft.com/en-us/powershell/module/webadministration/?view=windowsserver2022-ps
[IISAdministration]:  https://docs.microsoft.com/en-us/powershell/module/iisadministration/?view=windowsserver2022-ps
