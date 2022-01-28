# Super Cooling System
This work is based on the [VzBoT RSCS system](https://grabcad.com/library/rscs-for-vzbot-1). Big thanks to the VzBoT crew for providing all of the CAD drawings and awesome videos for information, inspiration, and acceleration!

![Much SCS](./img/SCS.png)

## What's SCS?
This is a system that's meant to cool top layer 3D printed extrusions quickly, helping you up the brrrr with better quality prints.

### What's the problem?
I found that as I tried to print at the higher end of my printers mechanical and extruder capabilities inevitably my print quality would suffer.  Especially challenging were overhangs, even 45 degree outer layers, and models with short layer print times (smaller single parts).

I moved to a dual 5015 cooling setup (EVA) for my part cooling which helped though at higher volumetric flows (~17mm3/sec), certain model features would become problematic (Benchy bow sag, ~45-50 degree overhangs in vase mode would deform at moderate speed, etc... ).

After watching and tuning for these scenarios it became clearer that cooling solutions that travel with the hot end have their limits. I think the main challenge is in that when printing at low layer times (<3sec), the fan ducts are generally not providing adequate cooling to the parts top layer surface because the hot end cooling fan is directly focused over the traveling nozzle.

More science necessary, though I believe cooling time could be calculated for a given material - helpful in determining quality print speed.

### Why try the STS
**First** I want to mention that the only reason I have this cooling problem is largely due to the [Zero-G Mercury project](https://github.com/ZeroGDesign/Mercury). By converting my heretofore fairly well tuned Ender 5 Pro printing at ~75mm/sec into something more 'moderately dangerous' - currently approaching 350mm/sec and 40mms3 volumetric flow.

After watching the [VzBoT video](https://youtu.be/65FVQ1jArME) on cooling solutions I was convinced to try this myself.

My hypothesis is that by adding additional top layer cooling, you'll be able to achieve higher speed/higher quality 3D prints.

If you're game, help me evaluate!

## STS Parts, Printing, Installation
All printed files are found in the STL folder, CAD files *soon

You will neee M3 screws, 2 fans, and the ability to print 15mm overhans clearly - you can do it - or your money back!   

### Hardware
* 2x 120x35 centrifugal fans - make sure they match the voltage and pin  configuration your board can work with!
 * Ability to crimp, solder, or otherwise connect the fans to your 3D printer controller board
* Various M3 bolts/nuts:
 * 2x 40mm bolts to affix fan and shroud to mount
 * 2x 10mm M3 bolts to connect the fan mount and fan extension
 * 2x 6mm M3 bolts to connect the fan shroud to the fan extension
 * 2x 16mm M3 bolts to connect the entire system to the underisde of 2020 Aluminum extrusion
* Roll In Spring T-Nuts for mounting the blower to the top 2020 extrusions-  - something like [these](https://8020.net/fasteningmethods/hardware/tnuts/rollin/rollintnutwithballspring.html)

### Printed Parts
All printed parts are included in the /STL folder - there are 5 parts:

* [Fan Duct Output - 175mm.stl](./stl/Bed%20Duct%20Output%20-%20175mm.stl)
* [Fan Extension.stl](./stl/Fan%20Extension.stl)
* [Fan Bracket.stl](./stl/Fan%20Bracket.stl)
* [Fan Shroud.stl](./stl/Fan%20Shroud.stl)
* [Spacers.stl](./stl/Spacers.stl)


### Installation
1. Connect the Fan Bracket and Fan Extension together using the M3 10mm screws.
1. Thread the M3 6mm screws that will be used to attach the fan duct, let the screws protude slightly from the top so it's easier to mount the duct later
1. Insert the M3 16mm screws that will be used to mount the system to the underside of the 2020 extrusion, thread the t-nuts
1. Using roll-in t-nuts will allow you to tilt the duct and get them inserted under the 2020 extrusion
1. Center the duct so that it's in the middle of your print bed
1. Once the duct is hanging from the 2020 extrusions, insert the spacers between the the extrusion and the duct mount, and tighten the bolts
1. Mount the fan and the shroud

### Klipper gcode macro example
Configure Klipper to control the fans and expose a Gcode macro so it can be controlled by your slicer.

#### Klipper Fan configuration
```
[output_pin Bed-Blower-R]
# BED BLOWER RIGHT
pin: PD12
pwm: true

[output_pin Bed-Blower-L]
# BED BLOWER LEFT
pin: PD13
pwm: true
```

#### Klipper GCode Macro
```
[gcode_macro BED_BLOWER]
gcode:
    # Use a default fan speed to off - expecting an INT as input
    {% set S = (params.S)|default(0)|float %}
    {% if S > 0 %}
        {% set S = (S/100) %}
    {% endif %}
    SET_PIN PIN=Bed-Blower-R VALUE={S}
    SET_PIN PIN=Bed-Blower-L VALUE={S}
```

### SuperSlicer custom gcode integration
In order to automatically turn on the SCS fans at a specific layer height, you need to add some custom `Before Layer Change G-code` section under the 'Printer Settings' in SuperSlicer.

```
; Turn on SCS at 3rd layer
{if layer_num == 2} BED_BLOWER  S=40{endif}
```
