# TM1640-16Digit-7Seg-LED-Display-Library
Custom Design TM1640 16 Digiy (4x4) 14.20mm LED Display Board Library

# TM1640_4x4CC

Simple Arduino library for a custom TM1640 module with **4 x 4-digit common-cathode 7-segment displays**.

## Physical display numbering

This library uses the numbering below so it matches the module layout:

```text
[DISPLAY1] [DISPLAY2]
[DISPLAY3] [DISPLAY4]
```

## Compatible boards

- Arduino Nano
- ESP32
- ESP8266
- Any Arduino-compatible board with 2 digital output pins

## Wiring

Module header:

- Pin 1 = GND
- Pin 2 = +5V
- Pin 3 = DIN
- Pin 4 = CLK

## Segment order

Segment bit order is:

```text
bit0=A bit1=B bit2=C bit3=D bit4=E bit5=F bit6=G bit7=DP
```

## Special clock note

On this PCB, the **DP of Digit 2** is electrically shared with the center **":" LEDs**.

That means:

- For normal numeric display, use dot `.` as usual.
- For clock display, use `setDisplayClock()` so the shared DP / colon line is controlled in a practical way.

## Main functions

### Constructor

```cpp
TM1640_4x4CC display(dataPin, clockPin);
```

### begin()

Initializes the GPIO pins and clears the display.

```cpp
display.begin();
```

### update()

Sends the internal buffer to the TM1640.

```cpp
display.update();
```

### clear()

Clears all 16 digits in the internal buffer.

```cpp
display.clear();
display.update();
```

### clearDisplay(displayIndex)

Clears one logical display only.

```cpp
display.clearDisplay(0);   // clears DISPLAY1
```

### setBrightness(level)

Brightness range is 0..7.

```cpp
display.setBrightness(7);
display.update();
```

### setOn(state)

Turns the display output on or off.

```cpp
display.setOn(true);
display.update();
```

### setDisplay(displayIndex, text)

Writes up to 4 characters to one display.
A dot `.` lights the previous digit DP.

```cpp
display.setDisplay(0, "12.34");
```

Result:

```text
[1][2.][3][4]
```

### setDisplay1() ... setDisplay4()

Convenience wrappers for physical display names.

```cpp
display.setDisplay1("1234");
display.setDisplay2("ABCD");
display.setDisplay3("17.08");
display.setDisplay4("HELP");
```

### setDisplayNumber(displayIndex, value, leadingZeros)

Prints an integer value on one display.

```cpp
display.setDisplayNumber(1, 42, false);  // "  42"
display.setDisplayNumber(1, 42, true);   // "0042"
```

### setDisplayClock(displayIndex, hour, minute, colonOn)

Shows `HH:MM` on one display.
The colon uses digit 2 DP because of the PCB wiring.

```cpp
display.setDisplayClock(2, 17, 8, true);
```

### setDigit(displayIndex, digitIndex, value, dot)

Writes one digit manually.

- `displayIndex`: 0..3
- `digitIndex`: 0..3, left to right
- `value`: character to show
- `dot`: lights that digit's dot

```cpp
display.setDigit(3, 0, '1', false);
display.setDigit(3, 1, '7', true);
display.setDigit(3, 2, '0', false);
display.setDigit(3, 3, '8', true);
```

Result:

```text
[1][7.][0][8.]
```

### setDigitRaw(globalDigitIndex, segments)

Writes a raw segment byte to a global digit index (0..15).
This is an advanced function.

```cpp
display.setDigitRaw(0, 0x3F);   // show "0" on raw digit 0
```

## Single demo

The library includes one example sketch:

- `examples/allFunction/allFunction.ino`

It demonstrates all main functions in one file.

## Installation

1. Download the ZIP.
2. In Arduino IDE: **Sketch -> Include Library -> Add .ZIP Library...**
3. Select the ZIP file.

## Special Char -_º

- minus (-), Underscore (_), Degree (x[º])


## Notes

- The module is powered from **5V**.
- With ESP32 / ESP8266, 3.3V logic may work, but a level shifter is safer if the module is not reliable. Eg. CD74HCT244E

For more [Aytac GUL](https://www.aytacgul.com).
