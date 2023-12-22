# Morse Code

## Overview
This project contains assembly code for blinking LEDs in a pattern that represents Morse code for a predefined string. It's designed for an ARM architecture and demonstrates direct manipulation of memory and registers to control hardware.

## Key Features
- 📝 Assembly code tailored for ARM.
- 💡 Controls LEDs to blink Morse code.
- 🔄 Includes loops, conditionals, and subroutine calls.

## Code Description
- The code begins by turning off all LEDs and setting up necessary registers.
- It uses a look-up table (`InputLUT`) to store the string that will be converted to Morse code.
- The `CHAR2MORSE` subroutine converts each character to Morse code.
- `LED_BLINK` handles the blinking of LEDs based on the Morse code.
- Delays are implemented using the `DELAY` subroutine to control the timing of blinks.

## Subroutines
- `LED_ON`: Turns on the LED.
- `LED_OFF`: Turns off the LED.
- `CHAR2MORSE`: Converts characters to Morse code.
- `DELAY`: Creates a delay for LED blinking.
- `LED_BLINK`: Controls the on and off state of the LED to blink Morse code.

## Morse Code Representation
- Morse code is represented in a 16-bit format, where each bit is a dot (1) or a dash (0).
