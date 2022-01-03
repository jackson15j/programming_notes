WebOS Notes
===========

Configure TV:

* http://webostv.developer.lge.com/sdk/tools/using-webos-tv-cli/testing-web-app-cli/

Deploy https://github.com/jamesmgittins/smartbomb via: [CLI (`ares.*`)]:

```bash
cd /usr/local/share/webOS_TV_SDK/CLI/bin/
./ares-install --device tv ~/github_forks/smartbomb/com.jamesgittins.smartbomb_0.0.1_all.ipk
```

ChewieBomb
----------

Deployment steps:

```bash
$ cd ~/github_forks/chewiebomb/
$ /usr/local/share/webOS_TV_SDK/CLI/bin/ares-package .
$ /usr/local/share/webOS_TV_SDK/CLI/bin/ares-install --device tv *.ipk
Installing package com.kevinmccray.chewiebomb_1.1.1_all.ipk
Success
$
```

[CLI (`ares.*`)]: https://webostv.developer.lge.com/sdk/command-line-interface/intro-cli/
