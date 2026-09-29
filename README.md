# AHB to APB Protocol Bridge Design & Verification

## Overview
This repository contains the Synthesizable Verilog implementation and testbench verification for an **AHB to APB Protocol Bridge**. The bridge acts as an AHB Slave to a high-performance AHB Master and converts AHB transfer requests into standard APB bus cycles for low-power peripheral access.

---

## Module Breakdown & Explanations

### 1. `AHB_Master.v`
* **Purpose:** Acts as the high-speed processor/master component.
* **Functionality:** Generates memory transfer requests (`single_write` and `single_read`). It provides the target address (`Haddr`) and data (`Hwdata`) to be sent over the bus.

### 2. `AHB_slave_interface.v`
* **Purpose:** Acts as the receiver interface on the AHB side.
* **Functionality:** Registers and pipelines incoming addresses and data across clock cycles. It verifies address validity (`valid` signal) and selects the appropriate peripheral (`tempselx`).

### 3. `APB_FSM_Controller.v`
* **Purpose:** Core controller for protocol translation.
* **Functionality:** Implements an **8-State Finite State Machine (FSM)**. Since AHB transfers execute in 1 clock cycle while APB requires 2 cycles (Setup and Enable phases), this module sequences the timing and generates APB control signals (`PSEL`, `PENABLE`, `PWRITE`).

### 4. `APB_Interface.v`
* **Purpose:** Models the low-power peripheral devices.
* **Functionality:** Maps incoming APB signals to target registers/peripherals and generates mock read data (`Prdata`) during read operations.

### 5. `Bridge_Top.v`
* **Purpose:** Top-level structural wrapper.
* **Functionality:** Interconnects `AHB_slave_interface` and `APB_FSM_Controller` using internal signal routing.

### 6. `top_tb.v`
* **Purpose:** Simulation testbench environment.
* **Functionality:** Generates clock and reset signals, drives test vectors (`single_write`), and outputs VCD dump files (`dump.vcd`) for waveform analysis.

---

## Simulation Setup
* **HDL:** Verilog
* **Simulator:** Icarus Verilog / ModelSim (EDA Playground)
* **Waveform Viewer:** EPWave / GTKWave
