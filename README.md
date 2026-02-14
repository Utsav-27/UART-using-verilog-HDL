# UART Using Verilog HDL

This repository contains a synthesizable and parameterized implementation of a Universal Asynchronous Receiver/Transmitter (UART) written in Verilog HDL. The design operates using a system clock and allows the user to configure only:

- Clock Frequency  
- Baud Rate  
- Data Width  

The internal timing required for UART communication is derived automatically from the provided clock frequency and baud rate.

---

## 📌 1. Basic Introduction to UART

UART (Universal Asynchronous Receiver/Transmitter) is a hardware-based serial communication protocol used to transfer data between digital systems without a shared clock line.

Key Characteristics:

- Asynchronous communication
- Two primary signals: TX (Transmit) and RX (Receive)
- Full-duplex communication support
- Widely used in FPGAs, microcontrollers, and embedded systems

A standard UART frame consists of:

- 1 Start bit (logic 0)
- N Data bits (LSB first)
- 1 Stop bit (logic 1)

Both communicating devices must operate at the same baud rate for reliable data transfer.

---

## 📌 2. Core Timing Concept

Since UART does not use an external clock, timing must be generated internally using the system clock.

The number of clock cycles required for one UART bit is derived from:

CLKS_PER_BIT = CLOCK_FREQUENCY / BAUD_RATE

The user provides:

- CLOCK_FREQUENCY (for example, 50 MHz)
- BAUD_RATE (for example, 9600 or 115200)

The design calculates the required timing internally using these parameters.

---

## 📌 3. Example Calculations (50 MHz Clock)

For 9600 baud:

50,000,000 / 9600 ≈ 5208 clock cycles per bit  

For 115200 baud:

50,000,000 / 115200 ≈ 434 clock cycles per bit  

Changing the baud rate automatically changes the bit timing.

---

## 📌 4. Design Features

- User-defined clock frequency  
- User-defined baud rate  
- User-defined data width  
- Automatic internal timing calculation  
- Full-duplex UART (TX + RX)  
- FSM-based architecture  
- Fully synthesizable RTL  
- Suitable for FPGA implementation  

---

## 📌 5. Transmitter Operation

1. Remains in IDLE state with TX line high  
2. On transmit request, sends Start bit  
3. Transmits DATA_WIDTH bits (LSB first)  
4. Sends Stop bit  
5. Returns to IDLE state  
6. Indicates busy status during transmission  

---

## 📌 6. Receiver Operation

1. Detects Start bit (falling edge on RX line)  
2. Waits half bit duration for mid-bit sampling  
3. Samples DATA_WIDTH bits  
4. Checks Stop bit  
5. Asserts receive-done signal when frame is complete  

Correct sampling depends on proper clock frequency and baud rate selection.

---

## 📌 7. Application Areas

- FPGA to PC serial communication  
- Embedded system debugging  
- Microcontroller interfacing  
- Serial data logging systems  
- Academic digital design projects  
- Hardware communication experiments  

---

## 📌 8. Summary

This UART implementation provides a clean, configurable, and reusable serial communication core. The user defines only the clock frequency and baud rate, and the internal design derives the required timing automatically, making the module flexible and easy to adapt to different communication requirements.
