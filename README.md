# Yup -- You've found the STS
This work is based on the [VzBoT RSCS system](https://grabcad.com/library/rscs-for-vzbot-1). Thank you all for providing all of the CAD drawings and [great videos] for information, inspiration, and acceleration!

![Much STS](./img/STS.png)

## What's STS?
It's a system that is meant to cool top layer 3D printed extrusions quickly, so that you can print 3D models faster with better quality.

### What's the problem?
I found that as I tried to print at the higher end of my printers mechanical and extruder capabilities inevitably my print quality would suffer.  Especially challenging were overhangs, even 45 degree outer layers, and models with short layer print times (smaller single parts).

I moved to a dual 5015 cooling setup (EVA) for my part cooling which helped though at higher volumetric flows (~17mm3/sec), certain model features would become problematic (Benchy bow sag, ~45-50 degree overhangs in vase mode would deform at moderate speed, etc... ).

After watching and tuning for these scenarios it became clearer that cooling solutions that travel with the hot end have their limits. I think the main challenge is in that when printing at low layer times (<3sec), the fan ducts are generally not providing adequate cooling to the parts top layer surface because the hot end cooling fan is directly focused over the traveling nozzle.

More science necessary, though I believe cooling time could be calculated for a given material - helpful in determining quality print speed.

### Why try the STS
Well, after watching the [VzBoT video]((https://youtu.be/65FVQ1jArME) on cooling solutions I was convinced to try this myself.

First - I want to mention that the only reason I have this cooling problem is largely due to the Zero-G Mercury project. By converting my heretofore well tuned Ender 5 Pro printing at 50mm/sec into something more akin to 'moderately dangerous' - currently approaching 350mm/sec and 40mms3 volumetric flow.

My hypothesis is that by adding additional top layer cooling, you'll be able to achieve higher speed/higher quality 3D prints.

If you're game, help me evaluate!

## STS Parts, Printing, Installation
All printed files are found in the STL folder, CAD files *soon    

### Hardware
* 2x 120x35 centrifugal fans - make sure they match the voltage and pin  configuration your board can work with!
** Ability to crimp, solder, or otherwise connect the fans to your 3D printer controller board
* Various M3 bolts/nuts - of note: 2x 40mm bolts to affix fan and shroud to mount
* Roll In Spring T-Nuts for mounting the blower to the top 2020 extusions


### Printed Parts
All printed parts are included in the /STL folder

### Klipper configuration
Gotta create some gcode macros that can control the blowers

### Klipper gcode macro example:

### SuperSlicer custom gcode integration:
# SCS
