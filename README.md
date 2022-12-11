# Apeks

Apeks is a DIY 12-bit through-hole adaptation of Mutable Instruments Peaks. Peaks was discontinued in 2017, but it's still pretty useful, and you can still see it in modern racks. For a starter system, it offers lots of functionality, so we made a version that's cheap and easy to fabricate, build, and modify.

![front.jpg](img/0_display.jpg)

## Hardware:
We saw what sound-force.nl did for Braids, and noticed Peaks used the same classic STM32 processor, so the same style of conversion could be done. Overall the conversion process was very similar to what Sound Force describes in their page about Braids: the layout is the same as it leaves space for THT components, the main PCB is shorter than 100mm, and the firmware only needs to have a few files changed (dac.h, dac.cc, lfo.h, and mini_sequencer.h).

The output stage needed to be modified slightly, as Peaks' output swing is 16V, which at 12 bits means the resolution would only be at almost 4mV. That's almost 5 cents at 1V/Oct, which is too much, as we could hear the steps in the output.

This version uses a 12 bit DAC, and the output op-amps scale the output to +/- 5V. This allows for a resolution of around 2.44mV, which we find acceptable for non-pitch related modulation.


## Firmware
For the DAC, the dac.h file is modified to bitshift the 16 bit values by 4 bits. The SPI output message use masks according to the MCP4822 write commands.

To bring the LFOs and mini-sequencers up to the maximum level (if not, they would be at around +/-3.125V), a couple of changes are needed. The lfo.h set_level arguments need to be set to 65535 instead of 40960, and in the output line of mini_sequencer.h, the same number needs to be changed.

The firmware is available as source and pre-compiled. We used a CP2102 and STM32CubeProg to program the chip (see images for a wiring diagram).

When you use the module for the first time, flash the bootloader and the main firmware. Then take care to calibrate it according to [these instructions](https://forum.mutable-instruments.net/t/trimpots-versus-software-calibration-of-peaks/8213/2).


## Panel
There are two panel versions. One uses 6mm caps for the trigger buttons, with LEDs that shine through the PCB beside them. The other has 8mm holes for the original illuminated buttons.
*Note: We've only tried our prototype panels for now, we haven't had these panels fabricated yet.*


## Further modifications
This version stays as close as possible to the original, but we'd be excited to see further modifications and improvements. We're working on a 64kb firmware (no easter egg and smaller bootloader), and a Dead Man's Catch adaptation.

But why not try to add CV inputs with attenuverters for the pots, make an SMD micro version that can be pre-fabbed, use an adapter board to use another DAC, see if you can do the same for Tides v1 etc...


## Build notes
We've included a BOM with links to Tayda, where you can source almost all the parts quite cheaply. The BOM includes some helpful notes about some components.

It's very hard to get ahold of boards with genuine STM32 chips right now. We haven't tested this with any other versions, like APM32, GD32, CH32 etc... but we can only assume that they will work too. **Just be careful that the pinouts match!**


## Contact
This is a DIY project, with no instructions or build support. However, I can try to answer some questions you may have (if I have the time) at [migueldetrool@protonmail.com](mailto:migueldetrool@protonmail.com).

Check out [Quinie's blog and shop](https://www.quinie.nl), where there are PCB and panels for sale.

*If you get these boards or panels fabricated, please do contact me. These boards have been tested, but still, maybe I oversaw a problem, so it would be good to have confirmation that these work for others.*

### Thanks
Thanks to sound-force.nl and matias-levy for inspiration, and Emilie Gillet for the original design and helpful forum posts.