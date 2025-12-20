# Goal: pi5dashcam
Use a Raspberry Pi 5 + UPS module + USB camera as a rear dashcam  recording 24x7 (+ optionally, an LCD as a rearview mirror when engine's running)

# ⧖ Parts list:
* ☑ Raspberry Pi 5 Model B Rev 1.1, RAM: 16GB + active cooler
* ☑ 4 x 21700 batteries (Samsung 40T 4000mAh 35A)
* ☑ J5Create 360 1080p USB webcam
* ☑ DFRobot Touchscreen (HDMI + USB) 10.5" 1920x1280
  * ☑ Temporary screen protector; ⧖ waiting on properly sized protector
* ☑ 256GB High Endurance uSD card
* ☑ Geekworm X1206 UPS module
  * Pros: Requires larger 21700 batteries (flat-top!); longer runtime.  Con: doesn't accept wide voltage input range like the X1202
* ⧖ <a href="https://www.amazon.com.au/dp/B0DTTH8ZTY">X1206-C1 Geekworm case</a> (that supports the Pi5 + X1206 card together)
  * Note that these cases are card specific - e.g. X1202 card will not fit in a X1206-C1 case / X1206 card will not fit in a X1202-C1 case
* ⧖ <a href="https://www.amazon.com.au/dp/B09B829DL9">Automotive Power voltage regulator board</a> - 9-36V DC input,  5V 5A DC USB output

# ☑ Early Test Phase

* ☑ <a href="https://www.raspberrypi.com/software/">Pi Imager</a> updated and image burned to test SD card - <a href="https://downloads.raspberrypi.com/raspios_arm64/images/raspios_arm64-2025-11-24/2025-11-24-raspios-trixie-arm64.img.xz">Pi OS, Debian 13.2 (64 bit, trixie)</a>
* ☑ Pi Imager Customisation options - hostname, (no networking yet), user + password, 
* ☑ Power up - initially unreliable, slightly more stable once booted to desktop
* ☑ DHCP + ssh confirmed; uninstalled CUPS
* ☑ Background set to (no image), color = black 
* ☑ Apply latest patches - sudo apt-get update; sudo apt-get upgrade
* ☑ host rename fix (since raspi=config fails): sudo nano /etc/hosts; sudo nano /etc/hostname
* ☑ Installed <a href="https://gstreamer.freedesktop.org/documentation/installing/on-linux.html?gi-language=c">gstreamer</a>
* ☑ Power supply issues; RPi5 tantrums if it doesn't know it can get 5V 5A.  No joy trying to get it to negotiate with a 130W USB C laptop power supply (probably propietary)
  * ☑ Reviewed now that X1206 is in place: resolved.
  * ☑ Previously needed to remove all USB devices in order boot up - this is no longer necessary.
* ☑ Early video testing
  * ☑ First preview working: `DISPLAY=:0 ffplay -hide_banner -f v4l2 -framerate 30 -video_size 1920x1080  -input_format mjpeg  -i /dev/video0`
    * ⚠ Observing a 3 second delay, which is... concerning.
    * ⚠ Disabled GUI desktop, and now FFPLAY is down to 1-to-1.5 seconds...
  * ☑ Second "GST" method - working! `gst-launch-1.0 -v v4l2src device=/dev/video0 ! videoconvert ! fbdevsink`
    * ☑ Removed DRM overlay from Pi config.txt - Almost no delay at all. Nice!
  * ☑ X1206 arrived - Pi power on is stable
  * ☑ Add X1206 module + install 4 x 21700 batteries
    * ⚠ Note: button-top batteries are NOT suitable, e.g. X1206 card needs flat 21700s; X1202 card needs flat 18650s.
  * ☑ Carefully follow **correct** instructions (Geekworm/Suptronics have multiple case/card combinations)
  * ☑ Match correct mounting screws to each mounting point (both card-to-pi and the case attachment points)
    * ⚠ Note: translucent plastic standoffs are for NVMe hat - not needed here.
  * ☑ Install screen protector for LCD
  * ☑ Cooling fan remains *off* unless system gets hot.
  * ☑ Amended eeprom config (max supply current)
  * ☑ Amended firmware/config.txt (max USB current)
  * ☑ Bash scripts (ported from bundled python code)
    * ☑ X1206 Battery state of charge (tested range 0.1%-102%)
    * ☑ X1206 Input voltage (3.x to 4.x)
    * ☑ X1206 Power supply status (pin 6)
    * ☑ X1206 Recharging status (reading pin 16 state)
    * ☑ Pi input voltage
    * ☑ Pi CPU temp
    * ☑ CPU fan RPM (tested using 'stress')
    * ☑ Pi CPU frequency
    * ☑ Throttle status (now/previously)
    
# Current stage: Main TESTING

* ☑ X1206 Power supply status (pin 6) needs to handle first boot (needs to be set to input mode)
* Assemble and test using 256GB uSD card as storage
  * flash o/s + test pi speed and temp
  * pi + monitor, test touch screen + power monitor separately from the Pi
  * ⧖ Test UPS triggers: vehicle accessories vs. low voltage vs. low state of charge (how low?)
  * ⧖ Test 1 x webcam - test basic functionality
* ⧖ OS configuration
  * ⧖ Disable IPv6
  * ⧖ Tune bootup
* ⧖ Assembly
  * ⧖ Confirm in-vehicle power consumption (pi) vs. power usage (battery) vs. power supply (vehicle USB C) + battery charging
* ⧖ Scripts
  * ⧖ Add uptime
  * ⧖ Track max / min values (set min to 200 at start)
  * ⧖ Add trend graphs (like HA pi stats?)
  * ⧖ Onscreen **gauges**
    * ⧖ CPU usage
    * ⧖ Temperature
    * ⧖ Overall battery level (no visiblity of individual cells)
    * ⧖ Recording progress/status
    * ⧖ Remaining disk space
    * ⧖ Wifi signal 
      
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
