# stm32f103-template
This is a template project for [stm32f103](http://www.st.com/content/st_com/en/products/microcontrollers/stm32-32-bit-arm-cortex-mcus/stm32f1-series/stm32f103.html?querycriteria=productId=LN1565) microcontroller (driving an LED).
The project requires the following:
- [GNU Tools for ARM Embedded Processors toolchain](https://launchpad.net/gcc-arm-embedded) (compiler, objcopy)
- [STLINK software](https://github.com/texane/stlink) (uploading code to stm32f103)
- [STM32 Standard Peripheral Libraries](http://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32-standard-peripheral-libraries/stsw-stm32054.html)

For more information please see [A template project for STM32 on Linux](https://blog.gypsyengineer.com/fun/a-template-project-for-stm32f103-on-linux.html).

## Usage
`arm-none-eabi-gcc` and `arm-none-eabi-objcopy` should be in `${PATH}`. `${STD_PERIPH_LIBS}` contains a path to STM32 Standard Peripheral Libraries. `${ST_FLASH}` contains a path to `st-flash`.
```
# clean up
make clean

# compile and link the project
STD_PERIPH_LIBS=/home/artem/projects/stm32/src/STM32F10x_StdPeriph_Lib_V3.5.0/ make all

# upload the code to stm32f103 (requires root privileges)
ST_FLASH=/home/artem/tools/stlink/st-flash make burn
```

The commands above should result to output like the following:
```
rm -f *.o *.elf *.hex *.bin
arm-none-eabi-gcc -g -O2 -Wall -T/home/artem/projects/stm32/src/STM32F10x_StdPeriph_Lib_V3.5.0//Project/STM32F10x_StdPeriph_Template/TrueSTUDIO/STM3210B-EVAL/stm32_flash.ld -mlittle-endian -mthumb -mcpu=cortex-m4 -mthumb-interwork -mfloat-abi=hard -mfpu=fpv4-sp-d16 -DSTM32F10X_MD -DUSE_STDPERIPH_DRIVER -Wl,--gc-sections -I. -I/home/artem/projects/stm32/src/STM32F10x_StdPeriph_Lib_V3.5.0//Libraries/CMSIS/CM3/DeviceSupport/ST/STM32F10x/ -I/home/artem/projects/stm32/src/STM32F10x_StdPeriph_Lib_V3.5.0//Libraries/CMSIS/CM3/CoreSupport -I/home/artem/projects/stm32/src/STM32F10x_StdPeriph_Lib_V3.5.0//Libraries/STM32F10x_StdPeriph_Driver/inc main.c /home/artem/projects/stm32/src/STM32F10x_StdPeriph_Lib_V3.5.0//Libraries/CMSIS/CM3/DeviceSupport/ST/STM32F10x/system_stm32f10x.c /home/artem/projects/stm32/src/STM32F10x_StdPeriph_Lib_V3.5.0//Libraries/STM32F10x_StdPeriph_Driver/src/stm32f10x_rcc.c /home/artem/projects/stm32/src/STM32F10x_StdPeriph_Lib_V3.5.0//Libraries/STM32F10x_StdPeriph_Driver/src/stm32f10x_gpio.c /home/artem/projects/stm32/src/STM32F10x_StdPeriph_Lib_V3.5.0//Libraries/CMSIS/CM3/DeviceSupport/ST/STM32F10x/startup/TrueSTUDIO/startup_stm32f10x_md.s -o led.elf
arm-none-eabi-objcopy -O ihex led.elf led.hex
arm-none-eabi-objcopy -O binary led.elf led.bin
sudo /home/artem/tools/stlink/st-flash write led.bin 0x8000000
2016-07-04T23:11:34 INFO src/common.c: Loading device parameters....
2016-07-04T23:11:34 INFO src/common.c: Device connected is: F1 Medium-density device, id 0x20036410
2016-07-04T23:11:34 INFO src/common.c: SRAM size: 0x5000 bytes (20 KiB), Flash: 0x10000 bytes (64 KiB) in pages of 1024 bytes
2016-07-04T23:11:34 INFO src/common.c: Attempting to write 4040 (0xfc8) bytes to stm32 address: 134217728 (0x8000000)
Flash page at addr: 0x08000c00 erased
2016-07-04T23:11:35 INFO src/common.c: Finished erasing 4 pages of 1024 (0x400) bytes
2016-07-04T23:11:35 INFO src/common.c: Starting Flash write for VL/F0/F3 core id
2016-07-04T23:11:35 INFO src/flash_loader.c: Successfully loaded flash loader in sram
  3/3 pages written
2016-07-04T23:11:35 INFO src/common.c: Starting verification of write complete
2016-07-04T23:11:35 INFO src/common.c: Flash written and verified! jolly good!
```
