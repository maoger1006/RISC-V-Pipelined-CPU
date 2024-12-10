# RISC-V CPU 

## Overview
This project contains a custom RISC-V CPU design and a corresponding MATLBA GUI for interfacing with and testing the CPU via UART connection. The CPU is structured with a pipelined architecture, a program counter (PC), an instruction memory (IM), a data memory (DM), various pipeline registers, a register file, an ALU, and a control unit. The MATLAB GUI provides a few programs to test CPU by load instructions and read back results.

## VHDL

Instruction format (32) bits:

(31 downto 24): opcode (8-bit operation code) – determines the instruction type and corresponding control signals.

(23 downto 21): Destination register address for write-back. Used when the instruction needs to write a computation result back into a register.

(20 downto 18): Source register address 1, used to read the first operand from the register file.

(17 downto 15): Source register address 2, used to read the second operand from the register file (if the instruction requires two source registers).

(15 downto 0): A 16-bit immediate value. Used by immediate-type instructions (e.g., ADDI, STOREI) or when storing a constant directly into memory or a register.

**Not all instructions need all these fields. Unused bits may be repurposed, for example:**

JMP (Jump) instruction: Uses (31 downto 24) for the opcode and other bits (e.g., (5 downto 0)) to specify the new PC address.

Branch instructions: Use specific opcode fields to identify registers involved in branch conditions and offset or address fields for determining the branch target.

LOAD/STORE instructions: Use certain bits to specify which registers provide addresses or data, and rely on the opcode to determine whether the operation is a load or store, as well as the data granularity (byte, half-word, etc.).

| Opcode          | Instruction    | 
|-----------------|----------------|
| 0000_0001       | SET            | 
| 0000_0010       | MOV            |
| 0000_0011       | SND            |
| 0000_0100       | JMP            | 
| 0000_0101       | LOAD           | 
| 0000_0110       | STORE          | 
| 0000_0111       | STOREi         | 
| 0000_1000       | ADD            |
| 0000_1001       | SUB            | 
| 0000_1010       | AND            |
| 0000_1011       | OR             | 
| 0000_1100       | XOR            |
| 0000_1101       | SLL            | 
| 0000_1110       | SRL            |
| 0000_1111       | SRA            |
| 0001_0000       | SLT            | 
| 0001_0001       | SLTU           | 
| 0001_0010       | ROL            | 
| 0001_0011       | ROR            | 
| 0001_0100       | ADDI           |
| 0001_0101       | SUBI           |
| 0001_0110       | ANDI           | 
| 0001_0111       | ORI            | 
| 0001_1000       | XORI           | 
| 0001_1001       | SLLI           |
| 0001_1010       | SRLI           | 
| 0001_1011       | SRAI           | 
| 0001_1100       | SLTI           | 
| 0001_1101       | SLTUI          |
| 0001_1110       | ROLI           | 
| 0001_1111       | RORI           | 
| 0010_0000       | NOT            |
| 0010_0001       | NOTI           | 
| 0010_0010       | BEQ            |
| 0010_0011       | BNE            |
| 0010_0100       | BLT            |
| 0010_0101       | BGE            |
| 0010_0110       | BLTU           |
| 0010_0111       | BGEU           |
| 0010_1000       | LOAD BYTE      |
| 0010_1010       | LOAD HALF      |
| 0010_1011       | LOAD BYTE UNS  |
| 0010_1100       | LOAD HALF UNS  |
| 0010_1101       | STORE BYTE     |
| 0010_1110       | STORE HALF     |
| 0010_1111       | LOAD UPPER     |

## MATLAB
cpu_gui.m:

The GUI includes predefined test programs (commented out or shown as examples), as well as a mechanism to connect to a given COM port (e.g., COM6) at a baud rate of 9600.

loadTestProgram(): Sends a selected program’s instructions to the CPU.

readData(): Callback triggered when the CPU sends back data. It reads the incoming 4-byte data packets and displays the resulting 32-bit value in the GUI.

## Getting Started

**Loading a Test Program:**

In the GUI, click the "Load test program" button. The selected test program’s instructions will be sent to the CPU. The CPU will start executing them automatically.

### Customization
Expanding the CPU: Feel free to modify or add instructions, increase memory sizes, or adjust pipeline registers. The provided code is modular, making it easier to insert new functionality.

