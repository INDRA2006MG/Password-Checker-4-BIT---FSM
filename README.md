# Password-Checker-4-BIT-FSM

## 📌Project Overview
This repository contains a digital password checker implemented as a Finite State Machine (FSM) for authenticating a fixed 4-bit password. The design uses sequential logic suitable for implementation in hardware description languages such as Verilog or VHDL.

The FSM waits for a 4-bit input supplied all at once, then verifies whether the entered sequence matches the predefined password. On successful authentication, the system outputs a signal indicating access granted.

## ⚙️ Features

- **FSM Stages:**  
  `IDLE` → `CHECK_PASSWORD` → `ACCESS_GRANTED` / `ACCESS_DENIED`  
  (The FSM waits for input, checks the entire 4-bit password simultaneously, then signals the result.)

- **Operation:**  
  - Waits in `IDLE` state until `start` signal is asserted  
  - On `start`, FSM moves to `CHECK_PASSWORD` state and compares input with stored password  
  - Depending on the comparison, signals `access_granted` or `access_denied`  
  - Returns to `IDLE` waiting for the next input  

- **Preset Password:**  
  Fixed 4-bit password defined in the code (default example: `4'b1010`), easily customizable.
# 🛠️ Specifications
- **Software**: Vivado ML Edition (Standard) 2024.2
- **Hardware**: ZedBoard Zynq-7000 ARM / FPGA SoC Development Board
# 🔌 Inputs
| Name   | Description        |
|--------|--------------------|
| clk    | Clock              |
| reset  | Reset              |
| S0-S3  | Password input bits|

# 💡 Outputs
| Name           | Description                  |
|----------------|------------------------------|
| access_granted | High if password is correct  |
| error          | High if password is wrong    |
| time_out         | High after 3 wrong attempts  |

# 📊 FSM State Log (Based on Simulation)

| Input Password | Event Description              | FSM State  | access_granted | error | locked (timeout) |
|----------------|--------------------------------|------------|----------------|-------|------------------|
| `1011`         | ✅ Correct password             | `GRANTED`  | 1              | 0     | 0                |
| `1001`         | ❌ Wrong password #1            | `DENIED`   | 0              | 1     | 0                |
| `0000`         | ❌ Wrong password #2            | `DENIED`   | 0              | 1     | 0                |
| `0101`         | ❌ Wrong password #3 → Locked   | `LOCKED`   | 0              | 1     | 1 ✅              |
| `1011`         | 🔒 Input blocked while locked   | `LOCKED`   | 0              | 0     | 1                |
| `1011`         | 🔁 Reset followed by correct pw | `GRANTED`  | 1              | 0     | 0                |


# 🔄 FSM Transitions

| Current State | Input Condition            | Next State | Description                          |
|---------------|----------------------------|------------|--------------------------------------|
| `IDLE`        | Password entered           | `CHECK`    | Start checking on clock edge         |
| `CHECK`       | Input == `1011`            | `GRANTED`  | Password matched                     |
| `CHECK`       | Input ≠ `1011`             | `DENIED`   | Password incorrect                   |
| `DENIED`      | Error count ≥ 3            | `LOCKED`   | Lock system after 3 failed attempts  |
| `GRANTED`     | System not locked          | `IDLE`     | Return to idle after access granted  |
| `DENIED`      | System not locked          | `IDLE`     | Return to idle for retry             |
| `LOCKED`      | Reset = 1                  | `IDLE`     | Unlock only on reset                 |



<img width="1366" height="907" alt="image" src="https://github.com/user-attachments/assets/8d14effd-a2a2-440f-885c-848d35dac881" />


