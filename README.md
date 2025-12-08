# Goal: pi5dashcam
Use a Raspberry Pi 5 + UPS module + USB camera as a rear dashcam  recording 24x7 (+ optionally, an LCD as a rearview mirror when engine's running)

# Current stage: ASSEMBLY
The following parts have been ordered and have started arriving:
* ☑ RPi 5 16GB + active cooler
* ☑ 4 x 21700 batteries (<a href="">Samsung 40T 4000mAh 35A</a>)
* ☑ J5Create 360 1080p USB webcam
* ☑ DFRobot Touchscreen (HDMI + USB) 10.5" 1920x1280
* ☑ Geekworm case (that supports the X1206)
* ☑ 256GB High Endurance uSD card
* Geekworm X1206 UPS module

# Early Test Phase
* ☑ <a href="https://www.raspberrypi.com/software/">Pi Imager</a> updated and image burned to test SD card - <a href="https://downloads.raspberrypi.com/raspios_arm64/images/raspios_arm64-2025-11-24/2025-11-24-raspios-trixie-arm64.img.xz">Pi OS, Debian 13.2 (64 bit, trixie)</a>
* ☑ Pi Imager Customisation options - hostname, (no networking yet), user + password, 
* ☑ Power up - initially unreliable, slightly more stable once booted to desktop
* ☑ DHCP + ssh confirmed; uninstalled CUPS
* ☑ Background set to (no image), color = black 
* ☑ Apply latest patches - sudo apt-get update; sudo apt-get upgrade
* ☑ host rename fix (since raspi=config fails): sudo nano /etc/hosts; sudo nano /etc/hostname
* ☑ Installed <a href="https://gstreamer.freedesktop.org/documentation/installing/on-linux.html?gi-language=c">gstreamer</a>
* ☑ Power supply issues; RPi5 tantrums if it doesn't know it can get 5V 5A.  No joy trying to get it to negotiate with a 130W USB C laptop power supply (probably propietary)
  * ⚠ Will revisit this issue when X1206 arrives; will likely be resolved.
  * ⚠ For now, able to boot by removing all USB devices and only reinserting once desktop has loaded
* ☑ Early video testing
  * ☑ First preview working: `DISPLAY=:0 ffplay -hide_banner -f v4l2 -framerate 30 -video_size 1920x1080  -input_format mjpeg  -i /dev/video0`
    * ⚠ Observing a 3 second delay, which is... concerning.
    * ⚠ Disabled GUI desktop, and now FFPLAY is down to 1-to-1.5 seconds, which might be acceptable...
  * ☑ Second preview method working: `gst-launch-1.0 -v v4l2src device=/dev/video0 ! videoconvert ! fbdevsink`
    * ☑ Pre-requisite: remove DRM overlay from Pi config.txt
    * ☑☑ Almost no delay at all - nice!
* Testing halted until X1206 arrivces, due to unstable power supply.

# TODO
* OS configuration
  * Disable IPv6
  * Tune bootup
  * Set EEPROM power setting (max supply current)
  * Set CONFIG.TXT power setting (max USB current)
* Assembly
  * Match batteries to X1206 UPS card (note: button-top batteries are NOT suitable for X1206 card (21700's) nor the X1202 card (18650's)
  * Carefully identify correct instructions (geekworm have multiple case/card combinations)
  * Match correct mounting screws to each mounting point (both card-to-pi and the case attachment points)
    * (translucent plastic standoffs are for NVMe hat - not needed here)
  * Confirm power consumption vs. power usage (battery) vs. power supply (vehicle USB C) + battery charging
  * Install screen protector for LCD
* Scripts
  * Onscreen gauges (how to render?) -
    * CPU usage
    * Temperature
    * 4 x battery levels
    * Recording status
    * Remaining disk space
    * Wifi status
    * Fan RPM 

# Future stages
Once the items are received I plan on testing first, then install into my vehicle.
* Assemble and test -
  * flash o/s + test pi speed and temp
    * cooling fan *off* unless system gets hot
  * pi + monitor, test touch screen + power monitor separately from the Pi
  * add X1206 ups module - test power off triggers (accessories / voltage / loss of power)
  * add 1 x webcam - test basic functionality
* Scripts (inspired by Fishwithadeagle)
  * Test USB display image
  * test bootup to image
  * test monitor going off/on again doesn't change Pi video out
  * Touchscreen quickset areas:
    * toggle brightness - max vs. min (night vs day)
    * toggle video recording
    * toggle audio recording      

# Likely Obstacles
Generally expecting a few challenges, not least of which is just handling all the video.
* 2x J5 webcams will both appear as "j5" devices; possible performance/contention, etc.
* Monitor may power itself from Pi (vehicle accessories being preferable, thus auto-on/off when engine is running)
* Monitor on/off cycling may affect video output; scripts may need to handle this and reset layout, etc.
* Some drivers may expect Pi OS with Desktop vs. general preference for a lean system

# Future goals
Optimistically, assuming basic functionality is achieved:
* 2nd camera (front-facing)
* Offload footage when home-wifi detected
* Controlled shutdown capability based on car battery monitoring and/or temperature monitoring
* 2x side-facing cameras for 360' coverage (probably low fps, pi cameras off the ribbon cable interfaces)
* Leverage car's cooling system option (e.g. centre console 'fridge')

# Reference info
 Pi: Raspberry Pi 5 Model B Rev 1.1, RAM: 16GB
 OS: Debian GNU/Linux 13.2 (trixie), Linux 6.12.47+rpt-rpi-2712 (arm64)
 
