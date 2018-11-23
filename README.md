# mirage-esp32-notes
Notes I took about my uses of the esp32 portage of mirageOS by @well-typed-lightbulbs.

Important note: I'm using the Odroid-Go model, every informations noted here should be used carefully when using another model.

Notes may sometime be written in French, I will translate them as soon as I can.

## Graphic driver

In order to used @lortex code for esp32 on the Odroid-Go, I need to modify the lcd driver: Indeed the pins used by the device are not the same as the pins used in @lortex device.
And I suppose this does not only exist for the Odroid-Go. In order to solve it, I searched in the sources of the C++ drivers given by the manufacturer ("Display.cpp"), the pins numbers were listed here.
Moreover, I needed to modify the arrays of bits sent to the screen upon initialization (I found the good ones in the same file as the pins).

__Problems to solve:__

The C++ code give a value of 0 for the reset pin.
The problem is, using this value makes the program crash, so I disabled all uses of this pin until I solve this.
Finally, the LCD backlight seems to be never lighted with my modified driver.

## Flashing and launching the app on the odroid

In order to do this, just follow the turorial given here: well-typed-lightbulbs/mirage-samples-esp32.
The following lines are additionnal observations:
* RAM:
    * esp32's RAM is really low (about a few hundred of kb generally), so if your device contains additionnal RAM in SPI, you need to activate it during the configuration. However, you need to be aware that SPI RAM does not allow to allocate DMA buffers, so you will not be able to use them for transactions between the elements of the esp (e.g. the buffer needed for the LCD screen transactions).
* Menuconfig:
    * In order to activate the SPI RAM, go in component config -> esp specific and select the 2nd choice ('y')
    * You will need to increase the size of the flash (16 mb on the Odroid) in flasher config -> flash size
    * You will need python2 in order to continue. If your python instance by default is python3, you can create a new virtualenv or modify  SDKtoolconfig -> python by giving python2 instead of python
* Flash:
    * Update your environment with the PATH leading to your instance of xtensia.
* Monitor:
    * The shortcut for leaving the app is ctrl + ]. If your keyboard is in azerty, use ctrl + altgr + ) instead.
* LCD screen:
    * It may be a bad use case from me, but it seems that the drivers places the point (0, 0) in the top right corner of the screen.
