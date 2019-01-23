Ubiquiti Unifi AP
=================

Recently bought a pair of Unifi Lite AP's to build a Mesh WiFi at home after
reading these reviews:

* [Arstechnica: UniFi (long) Review].
* [Arstechnica: UniFi (long) Review 3 year follow up].

Here's the tips and pain points.

Controller Setup
----------------

I have a NAS with VirtualBox, so:

* Spun up a headless Linux instance ([Manjaro]-Architect ISO).
* Set VirtualBox NIC to `Bridged`, `Access All`.
* Enabled the network post-install and rebooted: `sudo systemctl enable
  dhcpcd.service`.
* Installed the `unifi` package: `yaourt -S unifi`.
* Enabled the `unifi` package and rebooted: `sudo systemctl enable
  unifi.service`.

From a web browser we can configure the AP and Controller:

* Go to: `https://<controller_ip>:8443`.
* Go through Wizard.
* Plug in AP(s) into a PoE switch, or into a non-PoE switch via the PoE
  injector.
* When the AP appears in the browser, _"adopt"_ it.

How to...
=========

...reset the AP?
----------------

Physically:

* While powered, depress the Reset button on the AP for ~10secs or until the
  light turns off.
* Reset is complete when the white light comes up and is solid.

SSH:

* [SSH in](#...ssh-in?).
* `syswrapper.sh restore-default`.
* Reference: [Ubnt Help: Factory Reset AP].

...ssh in?
----------

* SSH credentials:
    * Factory defaults: `ubnt`/`ubnt`.
    * Post adoption/provisioned:
        * Go to _"Settings"_ on the Controller and scroll down to the bottom of
          the initial settings page.
        * Username and generated password (click _"eye"_ icon) under _"SSH"_
          section.

...up/downgrade over ssh?
-------------------------

* [SSH in](#...ssh-in?).
* [Ubnt: Download firmware] locally.
* `scp /path/to/<firmware>.bin <user>@<ap_ip>:/tmp/fwupdate.bin`.
* `syswrapper.sh upgrade2 &`.
* AP will reboot.
* Reference:
    * [Ubnt Help: Downgrade firmware].
    * [Ubnt Forum: Downgrade firmware].

Troubleshooting
===============

If none of these help, check the [Ubiquiti Unifi Forums].

Cannot ping AP
--------------

* _"Forget"_ AP in the Controller.
* [Reset the AP](#...reset-the-ap?).
* Should be pingable shortly.

AP is _"Isolated"_
------------------

* If you can't ping the AP: [Cannot ping AP](#cannot-ping-ap).
* Did you recently update and the AP is now _"Isolated"_?: [Up/Downgrade over
  SSH](...up/downgrade-over-ssh?).
* If `AP > Config > Wireless Uplinks > Allow messhing to another access point`
  is unticked, then check the cabling between AP and switch. It may be bad.
* Reference: [Ubnt Forum: Unifi AP isolated state].

AP is _"Connected (Wireless)"_
------------------------------

* Does the AP become _"Isolated"_ if you untick: `AP > Config > Wireless
  Uplinks > Allow messhing to another access point`? Check the cabling between
  AP and switch. It may be bad.

AP fails _"Provisioning"_
-------------------------

* [SSH in](#...ssh-in?).
* `set-inform http://<controller_ip/fqdn>:8080/inform`.
* `info` - what is the error?
    * Decrypt Error:
        * Did you recently update and the AP now fails _"Provisioning"_?:
          [Up/Downgrade over SSH](...up/downgrade-over-ssh?).
        * Or: [Ubnt Forum: set-inform Decrypt Error].
* `tail /var/log/messages to investigate.


[Manjaro]: https://manjaro.org/get-manjaro/
[Ubiquiti Unifi Forums]: https://community.ubnt.com/t5/UniFi-Wireless/bd-p/UniFi
[Ubnt Help: Factory Reset AP]: https://help.ubnt.com/hc/en-us/articles/205143490-UniFi-How-to-reset-the-UniFi-Access-Point-to-factory-defaults
[Ubnt: Download firmware]: https://www.ubnt.com/download/unifi/unifi-ap-ac-lite/uap-ac-lite
[Ubnt Help: Downgrade firmware]: https://help.ubnt.com/hc/en-us/articles/204910064-UniFi-Upgrading-firmware-image-via-SSH%C2%A0
[Ubnt Forum: Downgrade firmware]: https://community.ubnt.com/t5/UniFi-Wireless/Downgrade-firmware-UAP-AP-AC-PRO/td-p/1634144
[Ubnt Forum: Unifi AP isolated state]: http://www.edugeek.net/forums/wireless-networks/146889-unifi-ap-isolated-state-2.html
[Ubnt Forum: set-inform Decrypt Error]: https://community.ubnt.com/t5/UniFi-Wireless/UAP-PRO-2-3-6-set-inform-Decrypt-Error/td-p/341312

[Arstechnica: UniFi (long) Review]: https://arstechnica.com/gadgets/2015/10/review-ubiquiti-unifi-made-me-realize-how-terrible-consumer-wi-fi-gear-is/
[Arstechnica: UniFi (long) Review 3 year follow up]: https://arstechnica.com/information-technology/2018/07/enterprise-wi-fi-at-home-part-two-reflecting-on-almost-three-years-with-pro-gear/
