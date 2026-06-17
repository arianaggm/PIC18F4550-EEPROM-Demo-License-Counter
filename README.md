# PIC18F4550 EEPROM Demo License Counter

Embedded C project for the PIC18F4550 microcontroller demonstrating how to use internal EEPROM memory to store persistent data across resets. The project implements a simple demo/license counter that tracks the number of remaining uses and sends status messages through UART.

This project was developed as an academic embedded systems practice to understand EEPROM read/write operations, UART serial communication, and persistent state storage in PIC microcontrollers.

## Features

* Uses internal EEPROM memory to store persistent data.
* Detects first-time execution using an EEPROM marker value.
* Stores and updates the number of remaining demo attempts.
* Sends status messages through UART serial communication.
* Displays different messages depending on the number of remaining attempts.
* Demonstrates EEPROM read and write sequences using PIC18F4550 registers.
* Configures UART communication at approximately 9600 baud.

## Technologies Used

* C
* PIC18F4550 microcontroller
* MPLAB X / XC8
* Internal EEPROM
* UART serial communication
* Register-level embedded programming
* Persistent memory storage

## Project Structure

```text
.
├── eeprom.c      # Main application logic and demo counter behavior
├── eeprom.h      # Function declarations
├── functions     # EEPROM and UART helper functions
└── configs       # PIC18F4550 configuration bits
```

Recommended structure:

```text
.
├── README.md
├── src/
│   ├── main.c
│   └── eeprom_utils.c
└── include/
    ├── eeprom.h
    └── config.h
```

## How It Works

1. The microcontroller starts and configures the internal oscillator.
2. UART communication is initialized.
3. The program reads EEPROM address `200`.
4. If address `200` contains the marker value `0x55`, the program recognizes that the device was initialized before.
5. It then reads the remaining attempt counter from EEPROM address `201`.
6. If attempts are still available, it displays the number of remaining tries and decrements the counter.
7. If no attempts remain, it displays an “attempts exhausted” message.
8. If the marker value is not found, the program initializes EEPROM with the marker and a default number of attempts.

## EEPROM Memory Map

| EEPROM Address | Purpose                 |
| -------------: | ----------------------- |
|          `200` | Initialization marker   |
|          `201` | Remaining demo attempts |

## UART Output Behavior

| Condition             | UART Message                                             |
| --------------------- | -------------------------------------------------------- |
| First execution       | Initializes demo attempts                                |
| Attempts remaining    | Displays demo version message and remaining tries        |
| One attempt remaining | Displays singular `try left` message                     |
| No attempts remaining | Displays `Intentos agotados` and `Purchase FULL Version` |

## Main Functions

| Function       | Description                                                                             |
| -------------- | --------------------------------------------------------------------------------------- |
| `main()`       | Initializes UART, checks EEPROM state, updates the attempt counter, and sends messages. |
| `show_trys()`  | Sends UART messages depending on the remaining number of attempts.                      |
| `leeEE()`      | Reads one byte from EEPROM.                                                             |
| `escribeEE()`  | Writes one byte to EEPROM.                                                              |
| `configUART()` | Configures UART serial communication.                                                   |
| `TXsend()`     | Sends one character through UART.                                                       |
| `TXstringln()` | Sends a string followed by line feed and carriage return.                               |
| `TXnumber()`   | Sends a number through UART as ASCII digits.                                            |

## EEPROM Write Sequence

The EEPROM write routine follows the required unlock sequence before writing data:

```c
EECON2 = 0x55;
EECON2 = 0xAA;
EECON1bits.WR = 1;
```

This protects EEPROM memory from accidental writes and ensures that write operations are intentionally triggered.

## Learning Outcomes

Through this project, I practiced:

* Embedded C programming for PIC microcontrollers.
* Reading and writing internal EEPROM memory.
* Using EEPROM for persistent variables.
* Implementing a simple state/check mechanism with a marker byte.
* Configuring UART serial communication.
* Sending strings and numbers through UART.
* Working directly with PIC18F4550 control registers.
* Structuring reusable EEPROM and UART helper functions.

## Possible Improvements

* Rename files with standard extensions: `configs` to `config.h` and `functions` to `eeprom_utils.c`.
* Add a `README.md` and repository description.
* Add a serial monitor screenshot showing the UART output.
* Add comments explaining the EEPROM addresses used.
* Disable interrupts during the EEPROM unlock/write sequence for safer operation.
* Fix the typo `trys` to `tries` in user-facing messages.
* Organize code into `src/` and `include/` folders.
* Add a `.gitignore` for MPLAB/XC8 build files.

## Notes

This project was originally developed as an academic embedded systems practice. It demonstrates how EEPROM can be used to preserve information even after the microcontroller resets or loses power.

This is a learning prototype and is not intended to implement real software licensing or security.
