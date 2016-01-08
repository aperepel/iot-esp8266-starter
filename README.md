# Overview
A base IoT template for ESP8266-based WiFi boards.
* On startup, it enters the SmartConfig mode and
awaits for WiFi credentials to be provided (the easiest is to use your smartphone on the same network). There
are alternative, less robust methods, but not covered here.
* Once it gets WiFi credentials, it connects to the configured network
* Everything is accompanied by OLED prompts and debug info on UART/Serial.
* After connection is established, the board's IP is printed.
* The board turns off the OLED display
* Press the button once - it flashes the logo and prints the IP address. Turns off the display again.
* Hold the button for 5 seconds - wipes out WiFi credentials and goes into the SmartConfig mode (e.g.
  when you need to connect to a different WiFi network.)

# Why?
Over time I recognized repeatable patterns and best practices I followed for my projects and wanted to
capture those as a starter project. E.g. if you're interested in a connected device which goes beyond
blinking an LED and take provisioning/deployment/security and operations seriously.

From this state, use your favorite MQTT library, host the WebServer, POST JSON to your REST API endpoint, etc.
All while interacting with the whole gamut of sensors, the usual Arduino way.

# TBD
* more efficient sleep for when running on battery (currently draws ~20mA in always-on mode)
* Fritzing wiring diagram (if it has esp8266 components or similar?)
* A video demonstrating the flow

# Components
* Any ESP8266/esp12e/NodeMCU (no Lua, only using native Arduino/C++ code) - https://github.com/esp8266/esp8266-wiki/wiki
* SSD_1306 128x64 OLED display in I2C mode (not SPI)
* A push button + a 10K resistor to control display and force WiFi credential resets
* ESP8266 SmartConfig Android app: https://play.google.com/store/apps/details?id=com.cmmakerclub.iot.esptouch .
  Used to perform initial WiFi setup and provide credentials to the board. *no passwords are hardcoded
  in the source code*.
* For iOS users (I haven't tried it, but..) https://github.com/EspressifApp/EsptouchForIOS
* http://www.platformio.org (seriously, save yourself some trouble and start using it today)

# Building
```
# serial port monitor part is optional, but nice
platformio run -t upload && platformio serialports monitor --baud 115200
```

# IDE Support
Really, any IDE which PlatformIO supports will do (which in practice means everything.)
From free offerings I prefer Eclipse CDT for C++ development (a piece of me dies every time I use
Eclipse, but it still is light years ahead of other free alternatives for IoT development ;) )
```
# read PlatformIO reference docs for details, but here's an Eclipse example (respond Y when prompted)
platformio init --ide eclipse --board esp12e
```
In Eclipse, right-click in the project pane -> Import from Existing sources.

# Notes
* The `ESP_SSD1306` library is a display driver fork of the `Adafruit_SSD1306` with changes required for ESP8266.
* I had to inline the `Adafruit_GFX` library to pick up a more recent version from the one listed in PlatformIO repos.
