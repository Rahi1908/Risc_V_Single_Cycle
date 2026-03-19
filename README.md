# RISC-V Single-Cycle Processor — Verilog Implementation
## Title
Design and Implementation of a 32-bit RISC-V Single-Cycle Processor supporting RV32I Base Integer Instruction Set
_______________________________________________________________________________________________________________________________________________________________________

## 1.Introduction
This project showcases the complete hardware implementation of a 32-bit single-cycle RISC-V processor using Verilog HDL. The processor was designed, simulated, and tested in Xilinx Vivado 2025.1. Each module in the datapath was first built and verified individually, and then integrated to form a fully working CPU.

The design supports a significant subset of the RV32I base instruction set and follows the classic single-cycle architecture, where every instruction is executed in a single clock cycle. The instruction memory is implemented using big-endian byte ordering, and all immediate values are properly sign-extended in accordance with the RISC-V specification.
_______________________________________________________________________________________________________________________________________________________________________

## 2. Architecture Overview
  **Type**             **Instructions**
1. R-Type   :      add, sub, and, or, xor, sll, srl, sra, slt, sltu
2. I-Type   :      addi, andi, ori, xori, slti, sltui, slli, srli, srai
3. S-Type   :      lw, sw
4. B-Type   :      beq

_______________________________________________________________________________________________________________________________________________________________________
