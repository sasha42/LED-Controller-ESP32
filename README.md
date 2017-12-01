# LED Controller for ESP32

First you need to install all the dependencies

  sudo pip3 install esptool adafruit-ampy

Get latest firmware of micropython for EPS32 from [micropython's website](https://micropython.org/download#esp32). You will also need [CP210x drivers](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers). Next flash it using

  esptool.py --chip esp32 --port /dev/cu.SLAB_USBtoUART write_flash -z 0x1000 firmware.bin

You can now connect to the micropython console via

  screen /dev/cu.SLAB_USBtoUART 115200

Type `help()` and press enter. If all works well you should get a printout. Now run the following:

  >>> import os
  >>> os.listdir()
  ['boot.py']

This should show you what's on the ESP already. You can next try to follow examples from [cnx-software](https://www.cnx-software.com/2017/10/16/esp32-micropython-tutorials/) to test out WiFi connectivity and more.

The next step is to flash the OLED (display) driver to the ESP32:

  ampy --port /dev/cu.SLAB_USBtoUART put lib/ssd1306.py

Now check whether you can use the OLED display. Connect to the console and type:

  >>> import machine
  >>> import ssd1306

  >>> i2c = machine.I2C(scl=machine.Pin(4), sda=machine.Pin(5))
  >>> oled = ssd1306.SSD1306_I2C(128, 64, i2c)

  >>> oled.fill(0) 
  >>> oled.text('Hello world', 0, 0)
  >>> oled.show()

You should now have a ESP that looks like this!
--> insert picture

## Useful commands
### Scan for WiFi
```
import network
sta_if = network.WLAN(network.STA_IF); sta_if.active(True)
sta_if.scan()
```

### Display QR code
```
python3 image-encoder.py fixmem.png
# then some black magic with the framebuffer 
```

### Play with LEDs
```
import neopixel
import machine
np = neopixel.NeoPixel(machine.Pin(16), 8)
np[0] = (255, 0, 0)
np[1] = (0, 128, 0)
np[2] = (0, 0, 64)
np.write()
```

## Resources
* [OLED setup](http://www.instructables.com/id/MicroPython-on-an-ESP32-Board-With-Integrated-SSD1/)
* [More advanced OLED stuff](https://learn.adafruit.com/micropython-hardware-ssd1306-oled-display/circuitpython#drawing)
* [Displaying graphics on OLED](https://forum.micropython.org/viewtopic.php?t=2974)
