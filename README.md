## Instruction for epaper display development board([EPD](http://www.adafruit.com/products/1346)) with the [spark_ereader](https://github.com/androw72/spark-ereader) library

__Introduction__  
The spark ereader is a library that displays images and text. It's designed to work with MCUs that has small amount of RAM. To display text a font is read from the SD-card and translated to graphics. (To use the spark RAM memory as display memory you can examin the seedstudio [example](https://github.com/androw72/spark-seeedstudio-epaper)). Images are stored on the sd-card in the so called 'wif' format.

To debug the setup and wiring the first example tests the SD card only. And then the ereader library is tested. The ereader library demo is run and the application shows images and text from the SD card.

__Set up the sd-card:__

Connect the [Sd card brakeout board](https://learn.adafruit.com/adafruit-micro-sd-breakout-board-card-tutorial):

CS Chip Select A2  
DI: MOSI A5  
DO: MISO A4  
CLK:SCK A3  
GND GND  
VCC (3.3v) 3V3  
  

__Libraries for spark-Core__  
 For local spark development you need [core-firmware](https://github.com/spark/core-firmware.git), [core-common-lib](https://github.com/spark/core-common-lib.git), and [core-communication-lib](https://github.com/spark/core-communication-lib.git). You can use git to download the repositories. For more information look at the tutorial for [local development](http://community.spark.io/t/local-development-and-gdb-debugging-with-netbeans-a-step-by-step-guide/7829).  

__Libraries for SD card and epaper display__  
Download or clone the [sd-card-library](https://github.com/mumblepins/sd-card-library). And for the Ereader application download the [spark-ereader](https://github.com/androw72/spark-ereader.git) files for ereader with spark core.    


__Test the [Spark-CardInfo](https://github.com/mumblepins/sd-card-library/blob/master/firmware/examples/Spark-CardInfo.cpp) example to verify the SD card and the connections.__

 - copy the .cpp and .h files to core-firmware/src &../inc. 
 - copy the examples/Spark-CardInfo.cpp to ./core-firmware/src/application.cpp
 - Change the line in application.cpp 'include "sd-card-library/sd-card-library.h"' to include '"sd-card-library.h"'  

The following files are added to the firmware library.
 
```sh 
  //header files
  ./core-firmware/inc/sd-card-library.h
  ./core-firmware/inc/sd-fat-util.h
  ./core-firmware/inc/sd-fat.h
  ./core-firmware/inc/sd-info.h
  ./core-firmware/inc/sd2-card-config.h
  ./core-firmware/inc/sd2-card.h
  ./core-firmware/inc/fat-structs.h
  
  // source files
  ./core-firmware/src/File.cpp
  ./core-firmware/src/sd-card-library.cpp
  ./core-firmware/src/sd-file.cpp
  ./core-firmware/src/sd-volume.cpp
  ./core-firmware/src/sd2-card.cpp
  ./core-firmware/src/application.cpp
```

__Run the SD example__  
Connect througt the USB-serialport with a serial monitor, 115200 baud. You can use the sparc-dev serial monitor. Send a character to trigger the response. The program should read the sd-card and report back the library list of the SD-card.


__Wiring the EPD board:__  
Look at repapers pin description for the [EPD-board](http://repaper.org/doc/extension_board.html)

Vcc 3V Red 3.3V  
(LED1) White - not connected  
(UART_RX) Grey - not connected  
(UART_TX) Purple - not connected  
(SW2) Blue - not connected  
Temperature Green A0  
SPI_CLK Yellow A3  
BUSY Orange D7  
PWM Brown A7  
RESET Black D6  
PANEL_ON Red D2  
DISCHARGE White D4  
BORDER_CONTROL Grey D3  
SPI_MISO Purple A4  
SPI_MOSI Blue A5  
(RST/SBWTDIO) Green - not connected  
(TEST/SBWTCK) Yellow - not connected  
FLASH_CS Orange D1  
EPD_CS Brown D0  
GND Black GND  

__SD Card file structure__  
Download the [wyolum directory](https://github.com/wyolum/EPD).  Copy the [IMAGES/](https://github.com/wyolum/EPD/tree/master/libraries/EReader/examples/IMAGES) directory to the root folder of your SD card.  Copy the file called “unifont.wff” to the root directory of your SD card. 

```sh 
  [SD card root]/
  IMAGES/
  unifont.wff
```


__Setup the ereader example:__  
Copy ereader.cpp and EPD.cpp from the spark-ereader library to the core-firmware/src and EPD.h, EReader.h, and picture.h to core-firmware/inc folder.  


```sh 
  //header files
  ./core-firmware/inc/EPD.h
  ./core-firmware/inc/EReader.h
  ./core-firmware/inc/picture.h
  
  //source files
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

__Test the ereader program that loads image files from the sd-card__  
Start the serial monitor  and submit a character to trigger the program to start. The program will first read an image stored in the flash memory. Then it draws figures and display images from the sd-card




