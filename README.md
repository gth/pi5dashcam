# pi5dashcam
Raspberry Pi 5 + UPS module + USB camera as dashcams + LCD as a rear view mirror

# Goal
1x rear dashcam, recording 24x7 & shown on LCD monitor whenever the engine is running

# Current stage: ORDERING
The following parts have been ordered:
* RPi 5 16GB
* Geekworm X1202 UPS module + 4 x 18650 batteries
* Geekworm case (that supports the X1202)
* J5Create 360 1080p USB webcam
* DFRobot 1920x1200 Touchscreen (HDMI + USB), either 8.9" or 10.5"

# Future stages
Once the items are received I plan on testing first, then install into my vehicle.
* Assemble and test -
  * flash o/s + test pi speed and temp + cooling fan settings (preferably *off* unless hot)
  * pi + monitor, test touch screen + power monitor separately from the Pi
  * add X1202 ups module - test power off triggers (accessories / voltage / loss of power)
  * add 1 x webcam - test basic functionality
* Scripts (inspired by Fishwithadeagle)
  * Test USB display image
  * test bootup to image
  * test monitor going off/on again doesn't change Pi video out

# Likely Obstacles
Generally expecting a few challenges, not least of which is just handling all the video.
* If I run 2 x webcams, both j5s will appear as "j5" devices; possible performance contention, difficulty separating them
* Monitor may power itself from Pi (vehicle accessories being preferable, thus auto-on/off when engine is running)
* Monitor on/off cycling may affect video output; scripts may need to handle this and reset layout, etc.
* Some drivers may expect Pi OS with Desktop, which I'd like to avoid (no need for it, really)

# Future goals
Optimistically, assuming the basic functionality is achieved:
* 2nd camera (front-facing)
* Offload footage when home-wifi detected
* Controlled shutdown capability based on car battery monitoring and/or temperature monitoring
* 2x side-facing cameras for 360' coverage; probably low fps
* Leverage car's cooling system option (centre console 'fridge')
