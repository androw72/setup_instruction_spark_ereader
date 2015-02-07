# setup_instruction_spark_ereader

Set up the sd-card:

Connections:
Sd card brakeout board:

CS Chip Select A2
DI: MOSI A5
DI: MISO A4
CLK:SCK A3
GND GND
VCC (3.3v) 3V3



You need files from the following directories. You can use git to download the repositories. For more 

info about [local development](http://community.spark.io/t/local-development-and-gdb-debugging-with-

netbeans-a-step-by-step-guide/7829) and git tools, 

git clone https://github.com/mumblepins/sd-card-library
git clone https://github.com/spark/core-firmware.git
git clone https://github.com/spark/core-common-lib.git
git clone https://github.com/spark/core-communication-lib.git
git clone https://github.com/androw72/spark-ereader.git


Test the 'SparkCore-SD/libraries/SD/examples/Spark-CardInfo.cpp' to verify the SD card and connection.

copy the .cpp and .h files to core-firmware/src &../inc

copy the examples/Spark-CardInfo.cpp to application.cpp and change 

#include "sd-card-library/sd-card-library.h" to #include "sd-card-library.h"

Connect througt the USB-serialport with a serial monitor (115200 baud) (i.e. sparc-dev serial monitor) 

and send a character to trigger the response. The program should read the sd-card and report back the 

library list:


Wiering the board:

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



To run the ereader example:

Copy -e-reader.cpp and EPD.cpp to the core-firmware/src and EPD.h, EReader.h, and picture.h to core-

firmware/inc folder.


Start the serial monitor  and submit a character to trigger the program to start. 




