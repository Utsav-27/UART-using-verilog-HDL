# UART Using Verilog HDL

## 📌 Overview

This repository contains a synthesizable implementation of a Universal Asynchronous Receiver/Transmitter (UART) written in Verilog HDL. The design supports full-duplex serial communication and is intended for FPGA-based systems operating at a 50 MHz clock frequency.

The UART timing is controlled by a user-defined parameter called CLKS_PER_BIT, which must be manually calculated using the system clock and desired baud rate.

---

## 📌 Basic Concept of UART

UART (Universal Asynchronous Receiver/Transmitter) is an asynchronous serial communication protocol. It does not use a shared clock signal between transmitter and receiver. Instead, both sides agree on a communication speed known as the baud rate.

A standard UART frame consists of:

- 1 Start bit (logic 0)
- N Data bits (LSB first)
- 1 Stop bit (logic 1)

Since there is no shared clock, both transmitter and receiver must use identical timing derived from their local clock.

---

## 📌 Core Timing Principle

The UART in this repository uses the system clock (50 MHz) to generate the required baud timing.

To determine how long each UART bit should last, the number of clock cycles per bit must be calculated.

This value is called:

CLKS_PER_BIT

The user must compute this value manually and provide it to the design.

---

## 📌 Clock Per Bit Calculation

The formula used is:

CLKS_PER_BIT = CLOCK_FREQUENCY / BAUD_RATE

Where:

CLOCK_FREQUENCY = 50,000,000 Hz  
BAUD_RATE = Desired communication speed  

### Example for 9600 Baud

CLKS_PER_BIT = 50,000,000 / 9600  
CLKS_PER_BIT ≈ 5208  

### Example for 115200 Baud

CLKS_PER_BIT = 50,000,000 / 115200  
CLKS_PER_BIT ≈ 434  

The calculated integer value must be supplied to the UART modules for proper operation.

The design does NOT automatically compute this value internally.

---

## 📌 Design Features

- Designed for 50 MHz system clock
- User-defined CLKS_PER_BIT
- User-defined DATA_WIDTH
- Separate TX and RX state machines
- Full-duplex communication
- Synthesizable RTL
- Modular and reusable structure

---

## 📌 Transmitter Operation

1. Remains in IDLE state with TX line high.
2. When transmission starts, sends Start bit (logic 0).
3. Transmits DATA_WIDTH bits (LSB first).
4. Sends Stop bit (logic 1).
5. Returns to IDLE state.
6. Busy signal remains active during transmission.

---

## 📌 Receiver Operation

1. Monitors RX line for falling edge (Start bit).
2. Waits half bit duration to sample in the center of the bit.
3. Samples DATA_WIDTH bits.
4. Waits for Stop bit.
5. Asserts receive-done signal after successful reception.

Correct sampling accuracy depends entirely on proper CLKS_PER_BIT calculation.

---

## 📌 Applications

- FPGA to PC serial communication
- Embedded debugging interface
- Microcontroller communication
- Serial console systems
- Data logging
- Academic digital design projects

---

## 📌 Summary

This UART implementation provides a configurable and reusable serial communication core for FPGA systems. Timing accuracy depends on correct manual calculation of CLKS_PER_BIT using the system clock frequency and desired baud rate.
