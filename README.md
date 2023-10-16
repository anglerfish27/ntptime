# ntptime
A slightly modified version of Micropython's built in ntptime.py module to allow you to set your UTC offset on ESP32 (and possibly other) microcontrollers. This was built off the latest Micropython stable release available as of October 16th 2023. Version 1.21.

Intended for ESP32 S3 MCU's though it may work on others perhaps as well. This file allows you edit it so that you can set the UTC offset (time zone) for your location allowing the RTC to have the UTC time stored in it to match your exact local time. Code has only been testined on a few ESP32 S3 MCU's (mostly Unexpected Maker MCUs). The changes are minor when you look at the file. 

You simply provide your offset time at the top of the file and that should do it. You can change your NTP server if you wish to something more appropriate for your region.

Of course this requires that you are connected to the internet! To set the RTC time to your local time (in 24 hour format only) create an RTC instance.
```
Example:
rtc = RTC()

Then call the ntptime after your WiFi is up and running.
ntptime.settime()
```

That's it! The ESP's RTC will now be running with the proper time. Due to time skew that happens especially on ESP32 chipsets you will need to call the ntp.setttime() method
often to ensure you avoid drift. If you are using an ESP32 and you need very very HIGH accuracy time. I seperate module is probably your best bet such as a DS3231 or something similar to that. ESP32 chips have a colored history for their RTC's. As long as you are aware of these quirks this should work for you. Don't forget to call ntptime.settime() often (once an hour or so) due to the ESP32 RTC being unreliable. This has nothing to do with Micropython. 
```
To print the time out from the RTC (to check for skew or whatever reason):
print(rtc.datetime())
```
It will return a tuple with a length of 8. You can then proceed to manipulate the the time to create your own printing of time in any format you which. Such as MM/DD/YYYY or HH:MM::SS or whatever you wish. 

