# pi5dashcam
Raspberry Pi 5 + UPS module + USB camera as dashcams + LCD as a rear view mirror

# Goal
1x rear dashcam, recording 24x7 & shown on LCD monitor whenever the engine is running

# Current stage: DELIVERY
The following parts have been ordered and have started arriving:
* ☑ RPi 5 16GB + active cooler
* ☑ 4 x 18650 batteries (3.7V 3400mAh)
* ☑ J5Create 360 1080p USB webcam
* ☑ DFRobot Touchscreen (HDMI + USB) 10.5" 1920x1280
* Geekworm case (that supports the X1202)
* Geekworm X1202 UPS module
* 256GB High Endurance uSD card

# Early Test Phase
* <a href="https://www.raspberrypi.com/software/">Pi Imager</a> updated and image burned to test SD card - <a href="https://downloads.raspberrypi.com/raspios_arm64/images/raspios_arm64-2025-11-24/2025-11-24-raspios-trixie-arm64.img.xz">Pi OS, Debian 13.2 (64 bit, trixie)</a>
* Pi Imager Customisation options - hostname, (no networking), user + password, 
* Power up - initially unreliable, seems okay now though - to desktop
* DHCP + ssh confirmed
* Background set to (no image), color = black 
* Apply latest patches - sudo apt-get update; sudo apt-get upgrade
* host rename fix (since raspi=config fails): sudo nano /etc/hosts; sudo nano /etc/hostname
* Installed <a href="https://gstreamer.freedesktop.org/documentation/installing/on-linux.html?gi-language=c">gstreamer</a>

# TODO
* OS configuration
  * Disable IPv6
  * Tune bootup
* Assembly
  * Install screen protector 

# Future stages
Once the items are received I plan on testing first, then install into my vehicle.
* Assemble and test -
  * flash o/s + test pi speed and temp
    * cooling fan *off* unless system gets hot
  * pi + monitor, test touch screen + power monitor separately from the Pi
  * add X1202 ups module - test power off triggers (accessories / voltage / loss of power)
  * add 1 x webcam - test basic functionality
* Scripts (inspired by Fishwithadeagle)
  * Test USB display image
  * test bootup to image
  * test monitor going off/on again doesn't change Pi video out

# Likely Obstacles
Generally expecting a few challenges, not least of which is just handling all the video.
* 2x J5 webcams will both appear as "j5" devices; possible performance/contention, etc.
* Monitor may power itself from Pi (vehicle accessories being preferable, thus auto-on/off when engine is running)
* Monitor on/off cycling may affect video output; scripts may need to handle this and reset layout, etc.
* Some drivers may expect Pi OS with Desktop vs. general preference for a lean system

# Future goals
Optimistically, assuming the basic functionality is achieved:
* 2nd camera (front-facing)
* Offload footage when home-wifi detected
* Controlled shutdown capability based on car battery monitoring and/or temperature monitoring
* 2x side-facing cameras for 360' coverage; probably low fps
* Leverage car's cooling system option (centre console 'fridge')


# Reference info
 Raspberry Pi 5 Model B Rev 1.1  16GB
 Debian GNU/Linux 13.2 (trixie)
 Kernel: Linux 6.12.47+rpt-rpi-2712
 Architecture: arm64
