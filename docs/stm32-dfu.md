STM32 DFU Howto
===============


Mapple bootloader
-----------------

[Original documentation](http://leaflabs.com/docs/bootloader.html)

### Build bootloader firmware

    $ git clone git://github.com/plumbum/maple-bootloader.git
    $ cd maple-bootloader
    $ make
    $ ls -lh build/maple_boot.* # this is the compiled bootloader binary

### Upload binary to MCU

  * Get [stm32loader.py](https://github.com/leaflabs/libmaple/raw/master/support/stm32loader.py).
  * Connect PC to STM32 UART1 TTL port.
  * Pull BOOT0 to VCC and pull BOOT1 to GND.
  * Reboot MCU.
  * Run `python stm32loader.py -p ser-port -evw maple_boot.bin`.

or via ST-Link

  * Get and build [st-util](https://github.com/texane/stlink)
  * Run `st-util -m`
  * Upload bootloader `arm-none-eabi-gdb build/maple_boot.elf -ex "tar extended-remote :4242" -ex "load" -ex "quit"`

dfu-util
--------

Download latest [release (0.8)](http://dfu-util.sourceforge.net/releases/) of [DFU-UTIL](http://dfu-util.sourceforge.net/).

[Build](http://dfu-util.sourceforge.net/build.html):

    [./autogen.sh]
    ./configure
    make


### Find adapters

    $ dfu-util -l
    dfu-util 0.8

    Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.
    Copyright 2010-2014 Tormod Volden and Stefan Schmidt
    This program is Free Software and has ABSOLUTELY NO WARRANTY
    Please report bugs to dfu-util@lists.gnumonks.org

    Found DFU: [1eaf:0003] ver=0201, devnum=7, cfg=1, intf=0, alt=1, name="DFU Program FLASH 0x08005000", serial="LLM 003"
    Found DFU: [1eaf:0003] ver=0201, devnum=7, cfg=1, intf=0, alt=0, name="DFU Program RAM 0x20000C00", serial="LLM 003"


### Upload FLASH content from MCU to file

    $ dfu-util -a 1 -U fw_flash.bin 
    dfu-util 0.8

    Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.
    Copyright 2010-2014 Tormod Volden and Stefan Schmidt
    This program is Free Software and has ABSOLUTELY NO WARRANTY
    Please report bugs to dfu-util@lists.gnumonks.org

    Opening DFU capable USB device...
    ID 1eaf:0003
    Run-time device DFU version 0110
    Claiming USB DFU Interface...
    Setting Alternate Setting #1 ...
    Determining device status: state = dfuIDLE, status = 0
    dfuIDLE, continuing
    DFU mode device DFU version 0110
    Device returned transfer size 1024
    Copying data from DFU device to PC
    Upload	[======================== ]  99%       277504 bytesdfu-util: Error during upload
    Upload	[=========================] 100%       503808 bytes
    Upload done.


### Upload RAM content from MCU to file

    $ dfu-util -a 0 -U fw_ram.bin


