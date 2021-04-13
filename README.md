# ESP32_burn_firmware
batch file to burn firmware of ESP32


```
REM uses SPIFFS, ESP32-CAM used
REM 2021-04-13, FTBserver 1.5 firmware
REM burn test ok

:: To erase esp32 completely, do not rely on Arduino IDE and code upload, it has cluster and odd thing when uses FATFS, 
:: unless format SPIFFS or FATFS everytime on the fly
:: xiaolaba, 2020-MAR-02
:: Arduino 1.8.13, esptool and path,


set comport=COM5
set esptoolpath="C:\Users\user0\AppData\Local\Arduino15\packages\esp32\tools\esptool_py\3.0.0/esptool.exe"
set project=FTBserver

:: erase whole flash of esp32
%esptoolpath% --chip esp32 ^
--port %comport% ^
--baud 921600 ^
erase_flash

pause

REM burn firmware
%esptoolpath% --chip esp32 ^
--port %comport% ^
--baud 921600 ^
--before default_reset ^
--after hard_reset ^
write_flash -z ^
--flash_mode dio ^
--flash_freq 80m ^
--flash_size detect ^
0xe000 boot_app0.bin ^
0x1000 bootloader_qio_80m.bin ^
0x10000 %project%.ino.bin ^
0x8000 %project%.ino.partitions.bin

pause

REM 0xe000 C:\Users\user0\AppData\Local\Arduino15\packages\esp32\hardware\esp32\1.0.5/tools/partitions/boot_app0.bin 
REM 0x1000 C:\Users\user0\AppData\Local\Arduino15\packages\esp32\hardware\esp32\1.0.5/tools/sdk/bin/bootloader_qio_80m.bin 
REM 0x10000 C:\Users\user0\AppData\Local\Temp\arduino_build_451008/FTBserver.ino.bin 
REM 0x8000 C:\Users\user0\AppData\Local\Temp\arduino_build_451008/FTBserver.ino.partitions.bin 



```
