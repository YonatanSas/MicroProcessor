## MicroProcessor - PIC16F877 - Project

This repository contains the code for a 4-bit ALU (Arithmetic Logic Unit) simulator, developed as a project in a course in embedded systems. The project demonstrates the use of a microprocessor with attached peripherals, including a button matrix, LEDs and an LCD screen.

## Preview (On Simulator)
https://github.com/YonatanSas/MicroProcessor/assets/146161426/bfe31a05-6925-4de2-b533-8c5f13ee0c9f

## Project Overview

### Description
The program simulates a 4-bit microprocessor that includes the following components:
- **ALU**: Performs arithmetic and logical operations.
- **I/O**: Interfaces include an LCD display and a button matrix.
- **Memory**: Used to store input values and intermediate results.

### ALU Operations
The ALU identifies and executes operations based on an opcode stored in variable `C`. The operations are:
1. `000` - "ERROR" (No operation)
2. `001` - "A-B" (Subtraction)
3. `010` - "A*B" (Multiplication)
4. `011` - "A/B" (Division)
5. `100` - "A^B" (Exponentiation)
6. `101` - Count the number of `1`s in `B`
7. `110` - Count the number of `0`s in `A`
8. `111` - "ERROR" (No operation)

## Hardware Setup
The project uses the following hardware components connected to the microprocessor:
- **Button Matrix**: For user input.
- **LCD Display**: For displaying results and instructions.

## Software
The code is written in assembly language and compiled using MPLAB. The program includes initialization routines, user input handling, and the implementation of ALU operations.

### Usage
    
1. **Input Values:**
    - Use the button matrix to input values for `A`, `B`, and the opcode `C`.
    - The LCD will guide you through the process.

2. **Execute Operation:**
    - The ALU performs the operation based on the entered opcode.
    - The result is displayed on the LCD.

### Example
1. **Input `A`**: 3 (binary: 011)
2. **Input `B`**: 2 (binary: 010)
3. **Input `C`**: 010 (Multiply)
4. **Result**: Displayed on LCD as 6 (binary: 0110)

## Author
Yonatan Sasson 
