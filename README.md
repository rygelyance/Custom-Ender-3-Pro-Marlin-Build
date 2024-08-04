# Custom-Ender-3-Pro-Marlin-Build
 This is my custom build of Marlin 2.1.4.2 for my modified Ender-3 Pro 3D printer! This was customized specifically for my particular printer and what I wanted out of it in terms of quality and speed.

## What was used
The Marlin firmware is written in C++, I edited it using Visual Studio Code, and I compiled it using Auto-Marlin-Build and PlatformIO.

## My Ender-3 Pro!
Below is my upgraded Ender-3 Pro, which has some significant modifications from stock which affect the changes necessary for the firmware. 
![Image of my Ender-3 Pro](/Images/printer.jpg)
Here is a list of the main physical changes made:
- Dual Gear Extruder (Impacts E-Steps, Linear Advance, and general speed)
- Textured PEI Print Bed (Impacts z-offset height and y-axis input shaping)
- Capricorn PTFE Tubing (Impacts materials that can be printed reliably)
- Satsana Fan Duct (Impacts part cooling and x-axis input shaping)

With these changes, I wanted to recompile a custom firmware to be able to take advantage of the upgrades I've made and improve print speed and reliability.

## Firmware Features
For this firmware I've enabled several features to try and get the best balance of speed and quality. Those features are listed below:
- Custom Boot and Status Screens (see next section)
- Enabled a helpful Bed Tramming Menu for easy bed leveling
- Enabled Marlin's experimental input shaping for maintaining quality at higher speeds
- Enabled Linear Advance with a K-Value of 0.4 (calibrated to printer)
- Set custom e-steps, acceleration values, and input shaping frequencies within the firmware as the new defaults
- Disabled M503, EEPROM Chitchat, arc support, and the info menu in order to reduce firmware size and make more free space for the printer to run optimally

## All the Custom Settings and What They Do:
First are the custom boot and status screens, which aren't anything functional, but they're fun! Both were custom-created by yours truly! The boot screen is just a simple clean Creality logo with a hotend assembly for some cool _aesthetics_, and the status screen is a 2D representation of the classic Benchy model, probably the most well-known test print of all time. 

![Boot Screen](/Images/boot_screen.jpg)

![Status Screen](/Images/stat_screen.jpg)

These were originally created as low resolution images, modified slightly in photoshop to clean up the pixels and then converted to a C++ bitmap using [Marlin's Bitmap Converter](https://marlinfw.org/tools/u8glib/converter.html).

<img src="/Images/Custom%20Boot%20Screen%202%20inverted.png" width="1000"/>

<img src="/Images/2DBenchy.png" width="1000"/>


Next is the Bed Tramming Menu! This one just adds a menu which moves the print head over each of the 4 corners of the bed one at a time, allowing you to easily level the bed and dial it in. Prior to this I needed to manually move the print head over the 4 corners, which was much slower and less accurate.

Input Shaping was added as an experimental feature in Marlin 2.1.2. and was something I was quite interested in using. Input shaping is pretty simple in concept. As a 3D printer moves, the frame and moving parts will have a resonant frequency where they'll vibrate, leading to poor accuracy on parts, especially at higher speeds. Input shaping introduces an anti-vibration signal to the motor signals to stop them from vibrating! I found the y-axis resonant frequency to be 32 Hz, while the x-axis was light enough to have a resonant frequency above 60Hz which doesn't need to be compensated for. In the firmware I put the y-axis frequency in and disabled the x-axis shaping.

Linear Advance lends to the print quality by controlling under/overextrusion when the nozzle slows/accelerates. This stops print corners from being "blobby" and increases the "sharpness" of small details. Again, I dialed this in to a value of 0.4 and set that in the firmware.

I also set custom max values for the e-steps and speed/acceleration values. The e-steps refer to how much the stepper motor turns to extrude 1mm of filament. The new extruder had smaller gears, so needed higher e-steps, which I calibrated to be 143.3 (default is 93). I also increased the max speed and acceleration for x and y to be 750mm/s and 6000mm/s<sup> 2</sup> (defaults of 500 and 500). This was to make sure any high speeds I put into my slicer don't get overriden by the printer itself.

Finally, I commented out a few unused options and features to save space. I orignally had issues with the lcd being nearly completely unresponsive when printing. This behavior stopped when I lowered the firmware size to anything below around 150KB.