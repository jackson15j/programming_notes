RaspberryPi Notes
=================

Boot in Chromium kiosk mode
---------------------------

* `nano /home/pi/.config/lxsession/LXDE-pi/autostart`:

```bash
@xset s noblank
@xset s off
@xset –dpms
@chromium-browser --incognito --kiosk --start-fullscreen http://www.domain.com
```

Install Pimoroni 4" Screen
--------------------------

* `curl https://get.pimoroni.com/hyperpixel4 | bash`
* https://shop.pimoroni.com/products/hyperpixel-4
* https://github.com/pimoroni/hyperpixel4
