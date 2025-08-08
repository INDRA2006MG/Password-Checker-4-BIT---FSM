# Password-Checker-4-BIT-FSM

## 📌Project Overview
This repository contains a digital password checker implemented as a Finite State Machine (FSM) for authenticating a fixed 4-bit password. The design uses sequential logic suitable for implementation in hardware description languages such as Verilog or VHDL.

The FSM waits for a 4-bit input supplied all at once, then verifies whether the entered sequence matches the predefined password. On successful authentication, the system outputs a signal indicating access granted.

## ⚙️ Features

- **FSM Stages:**  
  `IDLE` → `CHECK_PASSWORD` → `ACCESS_GRANTED` / `ACCESS_DENIED`  
  (The FSM waits for input, checks the entire 4-bit password simultaneously, then signals the result.)

- **Inputs:**  
  - `password_in[3:0]` – 4-bit input password provided all at once  
  - `start` – Signal to trigger password verification  
  - `reset` – Resets the FSM to `IDLE` state  

- **Outputs:**  
  - `access_granted` – Asserted high if the input password matches the preset password  
  - `access_denied` – Asserted high if the input password is incorrect  
  - `busy` – Indicates the FSM is busy checking the password  

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
## 🔌 Inputs
| Name   | Description        |
|--------|--------------------|
| clk    | Clock              |
| reset  | Reset              |
| S0-S3  | Password input bits|

## 💡 Outputs
| Name           | Description                  |
|----------------|------------------------------|
| access_granted | High if password is correct  |
| error          | High if password is wrong    |
| time_out         | High after 3 wrong attempts  |

## ⚙️ FSM States

| State Name | Code | Description                                               |
|------------|------|-----------------------------------------------------------|
| `IDLE`     | 00   | Waits for a 4-bit password entry                          |
| `CHECK`    | 01   | Compares input with preset password                       |
| `GRANTED`  | 10   | Correct password entered → Grants access                  |
| `DENIED`   | 11   | Wrong password → Increments error counter                 |
| `LOCKED`   | N/A  | 3 wrong attempts → System is locked until reset is given  |

### 🔄 Transitions:
- `IDLE → CHECK` on clock edge after password is entered
- `CHECK → GRANTED` if input == `1011`
- `CHECK → DENIED` if input ≠ `1011`
- `DENIED → LOCKED` if error count ≥ 3
- `GRANTED or DENIED → IDLE` if system not locked
- `LOCKED → IDLE` only on reset

### 🔄 FSM Transitions

| From State | To State | Condition                         |
|------------|----------|-----------------------------------|
| `IDLE`     | `CHECK`  | On clock edge after input is given |
| `CHECK`    | `GRANTED`| If input == `1011`                |
| `CHECK`    | `DENIED` | If input ≠ `1011`                 |
| `DENIED`   | `LOCKED` | If error count ≥ 3                |
| `GRANTED`  | `IDLE`   | If system is not locked           |
| `DENIED`   | `IDLE`   | If system is not locked           |
| `LOCKED`   | `IDLE`   | Only on reset                     |

