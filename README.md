# RISC-V Single-Cycle Processor — Verilog Implementation
## Title
Design and Implementation of a 32-bit RISC-V Single-Cycle Processor supporting RV32I Base Integer Instruction Set
____________________________________________________________________________________________________________________________________________________________________

## 1.Introduction
This project showcases the complete hardware implementation of a 32-bit single-cycle RISC-V processor using Verilog HDL. The processor was designed, simulated, and tested in Xilinx Vivado 2025.1. Each module in the datapath was first built and verified individually, and then integrated to form a fully working CPU.

The design supports a significant subset of the RV32I base instruction set and follows the classic single-cycle architecture, where every instruction is executed in a single clock cycle. The instruction memory is implemented using big-endian byte ordering, and all immediate values are properly sign-extended in accordance with the RISC-V specification.
____________________________________________________________________________________________________________________________________________________________________

## 2. Architecture Overview

![image_alt](https://github.com/Rahi1908/Risc_V_Single_Cycle/blob/526104b2894dec1bdcafc37b9dffe85c06e06dcc/images/Architecture.png)


  **Type**             **Instructions**
1. R-Type   :      add, sub, and, or, xor, sll, srl, sra, slt, sltu
2. I-Type   :      addi, andi, ori, xori, slti, sltui, slli, srli, srai
3. S-Type   :      lw, sw
4. B-Type   :      beq

____________________________________________________________________________________________________________________________________________________________________

## 3. Data path overview
The processor follows the standard single-cycle RISC-V datapath:
- **Instruction Fetch** — PC increments by 4 each cycle, or jumps to branch target if PCSrc is asserted
- **Instruction Decode** — Register file reads two source registers; ImmGen sign-extends the immediate
- **Execute** — ALU performs the operation; second operand selected between register or immediate via ALUSrc mux
- **Memory Access** — Data memory performs load or store based on control signals
- **Write Back** — Result mux selects between ALU result and memory read data to write back to register file

____________________________________________________________________________________________________________________________________________________________________

## 4. Module Descriptions
- **pc.v**
Holds the current program counter. Updates on every rising clock edge. Active-low synchronous reset initializes PC to 0.

- **pc_adder.v**
Simple 32-bit adder used in two places — adding 4 to PC for sequential execution, and adding the sign-extended immediate to PC for branch target calculation.

- **instruction_memory.v**
128-byte byte-addressable instruction memory. Uses big-endian byte ordering. Instructions are loaded from TEST_INSTRUCTIONS.dat using $readmemb. Outputs a 32-bit instruction based on the current PC address.
verilogassign inst = {insts[readAddr], insts[readAddr+1], 
               insts[readAddr+2], insts[readAddr+3]};
               
- **control_unit.v**
Top-level control module. Instantiates the main decoder and ALU decoder internally. Generates all control signals — regWrite, memWrite, memRead, memtoReg, ALUSrc, PCSrc — based on opcode, funct3, and funct7. PCSrc is asserted when branch is taken and ALU zero flag is set.

- **main_decoder.v**
Decodes the 7-bit opcode to produce high-level control signals and a 2-bit ALUOp hint passed to the ALU decoder.

- **alu_decoder.v**
Takes ALUOp from main decoder along with funct3 and funct7 to produce a 4-bit ALU control signal covering all supported operations.

- **register_file.v**
32 general-purpose 32-bit registers. Register x0 is hardwired to zero. Synchronous write on rising clock edge, combinational read. x2 (stack pointer) initialized to 128 on reset.

- **ImmGen.v**
Generates sign-extended 32-bit immediates for I-type, S-type, and B-type instructions. B-type immediate already encodes the left shift by 1 via the appended 1'b0 at bit 0.

- **alu.v**
32-bit ALU supporting 10 operations — ADD, SUB, SLL, SLT, SLTU, XOR, SRL, SRA, OR, AND. Outputs a zero flag used for branch decisions, along with overflow, carry, and comparison flags.

- **data_memory.v**
128-byte byte-addressable data memory. Synchronous write, combinational read. Uses little-endian byte ordering for data storage. Active-low synchronous reset.

- **mux.v**
General-purpose 2-to-1 32-bit multiplexer used for PC select, ALU input select, and writeback data select.

- **single_cycle_top.v**
Top-level module that instantiates and wires all datapath and control modules together.

- **tb_riscv_sc.v**
Testbench that generates clock and reset signals. Active-low reset is held for 10ns then released. Simulation runs for 3000ns.

____________________________________________________________________________________________________________________________________________________________________

## 5. Processor Verification












