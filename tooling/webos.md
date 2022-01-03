WebOS Notes
===========

Develop & Deploy WebOS applications for LG TV's:

* [WebOS: SDK Install].
* Configure TV: [WebOS: Configure TV as Developer Device]
  (`prisoner@<ip>:9922`, Password=`<empty>`).
* [WebOS: Update SDK]:
  ```bash
  sudo /usr/local/share/webOS_TV_SDK/ComponentManager/./componentmanager_linux64
  ```
* Extend Dev time:
  ```bash
  $ /usr/local/share/webOS_TV_SDK/CLI/bin/ares-extend-dev --device tv
  Launched 'Developer Mode' app.
  Please check the 'Remain Session' through the device screen.
  $
  ```

ChewieBomb
----------

ChewieBomb is an application for viewing content from the Videogame site:
[GiantBomb].

[WebOS: Deploy from CLI] ([CLI (`ares.*`)]):

```bash
$ cd ~/github_forks/chewiebomb/
$ /usr/local/share/webOS_TV_SDK/CLI/bin/ares-package .
$ /usr/local/share/webOS_TV_SDK/CLI/bin/ares-install --device tv *.ipk
Installing package com.kevinmccray.chewiebomb_1.1.1_all.ipk
Success
$
```

SmartBomb
---------

[Github: smartbomb] is a deprecated application for viewing content from the
Videogame site: [GiantBomb].

[WebOS: Deploy from CLI] ([CLI (`ares.*`)]):

```bash
cd /usr/local/share/webOS_TV_SDK/CLI/bin/
./ares-install --device tv ~/github_forks/smartbomb/com.jamesgittins.smartbomb_0.0.1_all.ipk
```


[WebOS: SDK Install]: https://webostv.developer.lge.com/sdk/installation/download-installer/#
[WebOS: Update SDK]: https://webostv.developer.lge.com/sdk/installation/download-installer/?wos_flag=AddUpgradeRemove#AddUpgradeRemove
[WebOS: Configure TV as Developer Device]: https://webostv.developer.lge.com/develop/app-test/using-devmode-app/
[CLI (`ares.*`)]: https://webostv.developer.lge.com/sdk/command-line-interface/intro-cli/
[WebOS: Deploy from CLI]: https://webostv.developer.lge.com/sdk/command-line-interface/testing-web-app-cli/

[GiantBomb]: https://www.giantbomb.com
[Github: smartbomb]: https://github.com/jamesmgittins/smartbomb
