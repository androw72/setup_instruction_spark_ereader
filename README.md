# setup_instruction_spark_ereader

__Set up the sd-card:__

Connect the Sd card brakeout board:

CS Chip Select A2  
DI: MOSI A5  
DO: MISO A4  
CLK:SCK A3  
GND GND  
VCC (3.3v) 3V3  
  


You need files from the following directories. You can use git to download the repositories. For more info about how to compile localy look at the tutorial for [local development](http://community.spark.io/t/local-development-and-gdb-debugging-with-netbeans-a-step-by-step-guide/7829). For local spark development you need [core-firmware](https://github.com/spark/core-firmware.git), [core-common-lib](https://github.com/spark/core-common-lib.git), and [core-communication-lib](https://github.com/spark/core-communication-lib.git)  

To use the sd-card copy or clone the [sd-card-library](https://github.com/mumblepins/sd-card-library). And download the [spark-ereader](https://github.com/androw72/spark-ereader.git) files for ereader with spark core.    


__Test the [Spark-CardInfo](https://github.com/mumblepins/sd-card-library/blob/master/firmware/examples/Spark-CardInfo.cpp) example to verify the SD card and the connections.__

 - copy the .cpp and .h files to core-firmware/src &../inc. 
 - copy the examples/Spark-CardInfo.cpp to ./core-firmware/src/application.cpp
 - Change the line in application.cpp 'include "sd-card-library/sd-card-library.h"' to include '"sd-card-library.h"'  

The following files are added to the firmware library.
 
```sh 
  ./core-firmware/inc/sd-card-library.h
  ./core-firmware/inc/sd-fat-util.h
  ./core-firmware/inc/sd-fat.h
  ./core-firmware/inc/sd-info.h
  ./core-firmware/inc/sd2-card-config.h
  ./core-firmware/inc/sd2-card.h
  ./core-firmware/inc/fat-structs.h
  ./core-firmware/src/File.cpp
  ./core-firmware/src/sd-card-library.cpp
  ./core-firmware/src/sd-file.cpp
  ./core-firmware/src/sd-volume.cpp
  ./core-firmware/src/sd2-card.cpp
  ./core-firmware/src/application.cpp
```
  
Connect througt the USB-serialport with a serial monitor (115200 baud) (i.e. sparc-dev serial monitor) and send a character to trigger the response. The program should read the sd-card and report back the library list:


__Wiring the EPD board:__

Look at repapers pin description for the [EPD-board](http://repaper.org/doc/extension_board.html)

Pin Number Description Color

Vcc 3V Red 3.3V  
(LED1) White - - -  
(UART_RX) Grey - - -  
(UART_TX) Purple - - -  
(SW2) Blue - - -  
Temperature Green A0  
SPI_CLK Yellow A3  
BUSY Orange D7  
PWM Brown D5  
/RESET Black D6  
PANEL_ON Red D2  
DISCHARGE White D4  
BORDER_CONTROL Grey D3  
SPI_MISO Purple A4  
SPI_MOSI Blue A5  
(RST/SBWTDIO) Green - - -  
(TEST/SBWTCK) Yellow - - -  
FLASH_CS Orange D1  
/EPD_CS Brown D0  
GND Black GND  

__SD Card file structure__  
Download the [wyolum directory](https://github.com/wyolum/EPD).  Copy the [IMAGES/](https://github.com/wyolum/EPD/tree/master/libraries/EReader/examples/IMAGES) directory to the root folder of your SD card.  Copy the file called “unifont.wff” to the root directory of your SD card. 

```sh 
  [SD card root]/
  IMAGES/
  unifont.wff
```


__Run the ereader example:__

Copy -ereader.cpp and EPD.cpp to the core-firmware/src and EPD.h, EReader.h, and picture.h to core-firmware/inc folder.  


```sh 
  ./core-firmware/inc/EPD.h
  ./core-firmware/inc/EReader.h
  ./core-firmware/inc/picture.h
  ./core-firmware/src/EPD.cpp
  ./core-firmware/src/EReader.cpp
```

__Note__
If you don't want to use wlan you can do the following to turn it off. This also reduces the amount of compiled code downloaded to the spark-core so you will have more memory available.
```sh 
Comment out the spark wlan below in the ./core-common-lib/SPARK_Firmware_Driver/inc/platform_config.h

/* Uncomment the line below to enable WLAN, WIRING, SFLASH and RTC functionality */
//#define SPARK_WLAN_ENABLE
#define SPARK_WIRING_ENABLE
#define SPARK_SFLASH_ENABLE
#define SPARK_RTC_ENABLE
```

__Test the ereader program with image-files on the sd-card__  
Start the serial monitor  and submit a character to trigger the program to start. The program will first read an image stored in the flash memory. Then it draws figures and display images from the sd-card




