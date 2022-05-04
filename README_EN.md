This is an automatic translation, may be incorrect in some places. See sources and examples!

# GyverTM1637
GyverTM1637 - library for 7-segment display on TM1637 chip with a lot of fun
- Output numbers in an array or targeting
- Output letters from the list of available (scroll below) in an array or aimingly
- A separate function for displaying hours and minutes (hours without zero on the left, minutes with zero)
- Displaying a number from -999 to 9999 with a sign
- Ready running line function
- Brightness change and colon status functions automatically update the display
- Value update function with vertical scroll effect
- Value update function with twist effect (better to see once)

### Compatibility
Compatible with all Arduino platforms (using Arduino functions)

### Documentation
The library has [extended documentation](https://alexgyver.ru/tm1637_display/)

## Content
- [Install](#install)
- [Initialization](#init)
- [Usage](#usage)
- [Example](#example)
- [Versions](#versions)
- [Bugs and feedback](#feedback)

<a id="install"></a>
## Installation
- The library can be found under the name **GyverTM1637** and installed via the library manager in:
    - Arduino IDE
    - Arduino IDE v2
    - PlatformIO
- [Download library](https://github.com/GyverLibs/GyverTM1637/archive/refs/heads/main.zip) .zip archive for manual installation:
    - Unzip and put in *C:\Program Files (x86)\Arduino\libraries* (Windows x64)
    - Unpack and put in *C:\Program Files\Arduino\libraries* (Windows x32)
    - Unpack and put in *Documents/Arduino/libraries/*
    - (Arduino IDE) automatic installation from .zip: *Sketch/Include library/Add .ZIP library…* and specify the downloaded archive
- Read more detailed instructions for installing libraries [here] (https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE% D0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)

<a id="init"></a>
## Initialization
```cpp
GyverTM1637 disp(CLK, DIO);
```

<a id="usage"></a>
## Usage
```cpp
void display(uint8_t DispData[]); // displays numbers in an array by cells. 0 to 9 (byte values[] = {3, 5, 9, 0}; )
void display(uint8_t BitAddr, uint8_t DispData); // outputs the DispData digit to the specified display cell BitAddr
void display(uint8_t bit0, uint8_t bit1, uint8_t bit2, uint8_t bit3); // if too lazy to create an array, displays the numbers in the cells

void displayByte(uint8_t DispData[]); // outputs a byte like 0xe6 and constant letters like _a , _b .... in an array
void displayByte(uint8_t BitAddr, uint8_t DispData); // outputs a byte like 0xe6 and constant letters like _a , _b .... into a cell
void displayByte(uint8_t bit0, uint8_t bit1, uint8_t bit2, uint8_t bit3); // if too lazy to create an array, outputs bytes to cells

void displayClock(uint8_t hrs, uint8_t mins); // prints hours and minutes
void displayClockScroll(uint8_t hrs, uint8_t mins, int delayms); // displays hours and minutes with a scrolling effect
void displayClockTwist(uint8_t hrs, uint8_t mins, int delayms); // prints hours and minutes with a twist effect

void displayInt(int value); // prints a number from -999 to 9999 (yes, with a minus sign)
void runningString(uint8_t DispData[], byte amount, int delayMs); // ticker (array, sizeof(array), delay in ms)
void clear(void); // clear display
void point(boolean PointFlag); // on / off point (POINT_ON / POINT_OFF)Cranberry
void brightness(uint8_t bright, uint8_t = 0x40, uint8_t = 0xc0); // brightness 0 - 7

void scroll(uint8_t BitAddr, uint8_t DispData, int delayms); // update value by scrolling (address, NUMBER, delay in ms)
void scroll(uint8_t DispData[], int delayms); // update value by scrolling (array of DIGITS, delay in ms)
void scroll(uint8_t bit0, uint8_t bit1, uint8_t bit2, uint8_t bit3, int delayms); // scroll character by character
void scrollByte(uint8_t BitAddr, uint8_t DispData, int delayms); // update value by scroll (address, BYTE, delay in ms)
void scrollByte(uint8_t DispData[], int delayms); // update value by scrolling (byte array, delay in ms)
void scrollByte(uint8_t bit0, uint8_t bit1, uint8_t bit2, uint8_t bit3, int delayms); // scroll character by character

void twist(uint8_t BitAddr, uint8_t DispData, int delayms); // update value by twisting (address, NUMBER, delay in ms)
void twist(uint8_t DispData[], int delayms); // update value by twisting (array of DIGITS, delay in ms)
void twist(uint8_t bit0, uint8_t bit1, uint8_t bit2, uint8_t bit3, int delayms); // twist character by character
void twistByte(uint8_t BitAddr, uint8_t DispData, int delayms); // update value by twisting (address, BYTE, delay in ms)
void twistByte(uint8_t DispData[], int delayms); // update value by twisting (byte array, delay in ms)
void twistByte(uint8_t bit0, uint8_t bit1, uint8_t bit2, uint8_t bit3, int delayms); // twist character by character
```

<a id="example"></a>
## Example
See **examples** for other examples!
```cpp
/*
   Display example with register TM1637
   shows all the features of the GyverTM1637 library
   AlexGyver Technologies http://alexgyver.ru/
*/

#define CLK 2
#define DIO 3

#include "GyverTM1637.h"
GyverTM1637 disp(CLK, DIO);

uint32_t Now, clocktimer;
boolean flag;

void setup() {
  Serial.begin(9600);
  disp.clear();
  disp.brightness(7); //brightness, 0 - 7 (minimum - maximum)

}

void loop() {
  runningText();
  scroll();
  scrollClock();
  twists();
  twistClock();
  ints();
  bytes();
  fadeBlink();
  normClock();
}

void twist() {
  // twist array of NUMBERS
  byte digs[4] = {3, 5, 7, 1};
  disp.twist(digs, 50); // scroll speed 100
  delay(1000);

  // twist targeting(cell, BYTE, speed)
  disp.twistByte(0, _1, 50);
  delay(1000);

  // twist aiming(cell, NUMBER, speed)
  disp.twist(0, 8, 70);
  delay(1000);

  disp.clear();
  delay(200);
  for (byte i = 0; i < 10; i++) {
    disp.twist(3, i, 20);
    delay(200);
  }

  // twisting the BYTE array
  byte troll[4] = {_t, _r, _o, _l};
  disp.twistByte(troll, 50);
  delay(1000);

  // targeted twist of BYTE(cell, byte, rate)
  disp.twistByte(2, _G, 50);
  delay(1000);
}

void twistClock() {
  byte hrs = 21, mins = 55;
  uint32_t tmr;
  Now = millis();
  while (millis () - Now < 10000) { // every 10 seconds
    if (millis() - tmr > 500) { // every half second
      tmr = millis();
      flag = !flag;
      disp.point(flag); // off/off point

      if (flag) {
        // ***** clock! ****
        mins++;
        if (mins > 59) {
          mins = 0;
          hrs++;
          if (hrs > 24) hrs = 0;
        }
        // ***** clock! ****
        disp.displayClockTwist(hrs, mins, 35); // output time
      }
    }
  }
  disp.point(0); // off points
}

void scroll() {
  // scroll array of NUMBERS
  byte digs[4] = {3, 5, 7, 1};
  disp.scroll(digs, 100); // scroll speed 100
  delay(1000);

  // scroll aiming(cell, NUMBER, speed)
  disp.scroll(0, 8, 200);
  delay(1000);

  disp.clear();
  delay(1000);
  for (byte i = 0; i < 10; i++) {
    disp.scroll(3, i, 50);
    delay(400);
  }

  // scroll through the BYTE array
  byte troll[4] = {_t, _r, _o, _l};
  disp.scrollByte(troll, 100);
  delay(1000);

  // targeted scrolling of BYTE(cell, byte, rate)
  disp.scrollByte(2, _G, 50);
  delay(1000);
}

void bytes() {
  // output bytes from the array
  byte troll[4] = {_t, _r, _o, _l};
  disp.displayByte(troll);
  delay(1000);

  // output bytes directly (4 in parentheses)
  disp.displayByte(_L, _O, _L, _empty);
  delay(1000);

  // output bytes "targeted"
  disp.displayByte(3, _O); // 3rd cell, letter O
  delay(1000);

  // output numbers from the array
  byte hell[4] = {6, 6, 6, 6};
  display.display(hell);
  delay(1000);

  // output numbers directly (4 in brackets)
  display.display(1, 2, 3, 4);
  delay(1000);

  // output numbers "target"
  display.display(0, 9); // 0 cell, number 9
  delay(1000);
}

void fadeBlink() {
  // write HELL
  disp.displayByte(_H, _E, _L, _L);

  Now = millis();
  while (millis () - Now < 3000) { // 3 seconds
    for (int i = 7; i > 0; i--) {
      disp.brightness(i); // change brightness
      delay(40);
    }
    for (int i = 0; i < 8; i++) {
      disp.brightness(i); // change brightness
      delay(40);
    }
  }
}

void scrollClock() {
  byte hrs = 15, mins = 0;
  uint32_t tmr;
  Now = millis();
  while (millis () - Now < 10000) { // every 10 seconds
    if (millis() - tmr > 500) { // every half second
      tmr = millis();
      flag = !flag;
      disp.point(flag); // off/off point

      if (flag) {
        // ***** clock! ****
        mins++;
        if (mins > 59) {
          mins = 0;
          hrs++;
          if (hrs > 24) hrs = 0;
        }
        // ***** clock! ****
        disp.displayClockScroll(hrs, mins, 70); // output time
      }
    }
  }
  disp.point(0); // off points
}

void normClock() {
  byte hrs = 15, mins = 0;
  uint32_t tmr;
  Now = millis();
  while (millis () - Now < 10000) { // every 10 seconds
    if (millis() - tmr > 500) { // every half second
      tmr = millis();
      flag = !flag;
      disp.point(flag); // off/off point

      // ***** clock! ****
      mins++;
      if (mins > 59) {
        mins = 0;
        hrs++;
        if (hrs > 24) hrs = 0;
      }// ***** clock! ****
      disp.displayClock(hrs, mins); // display the time with the clock function
    }
  }
  disp.point(0); // off points
}

void ints() {
  // stupidly send numbers
  disp.displayInt(-999);
  delay(500);
  displayInt(-99);
  delay(500);
  disp.displayInt(-9);
  delay(500);
  displayInt(0);
  delay(500);
  display.displayInt(6);
  delay(500);
  display.displayInt(66);
  delay(500);
  disp.displayInt(666);
  delay(500);
  disp.displayInt(6666);
  delay(500);
}

void runningText() {
  byte welcome_banner[] = {_H, _E, _L, _L, _O, _empty, _empty,
                           _e, _n, _j, _o, _y, _empty, _empty,
                           _1, _6, _3, _7, _empty, _d, _i, _S, _P, _l, _a, _y
                          };
  disp.runningString(welcome_banner, sizeof(welcome_banner), 200); // 200 is the time in milliseconds!
}

```

<a id="versions"></a>
## Versions
- v1.4 - fixed data types and errors, added compatibility with ESP
- v1.4.1 - ESP32 compatibility
- v1.4.2 - slightly redesigned point output, you can not update

<a id="feedback"></a>
## Bugs and feedback
When you find bugs, create an **Issue**, or better, immediately write to the mail [alex@alexgyver.ru](mailto:alex@alexgyver.ru)
The library is open for revision and your **Pull Request**'s!