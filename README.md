> 📌 **Note:** This project began as a Python-based 24-bit RISC CPU simulator. It’s now evolving into a full Verilog RISC-V implementation targeting the DE1-SoC FPGA in Summer 2025.

---

# 🧠 24-bit RISC CPU Simulator (Python)

## 🚀 Overview

This project simulates a custom 24-bit RISC processor using a five-stage pipeline architecture — Instruction Fetch (IF), Decode (ID), Execute (EX), Memory (MEM), and Writeback (WB). Developed entirely in Python, the simulator models instruction flow and cycle-accurate behavior using a handcrafted 17-instruction ISA. This was built as a learning-focused tool to deepen understanding of CPU design and pipelining without hardware or Verilog dependency.

---

## 💡 Key Features

* 🧱 **Custom 24-bit ISA** with 17 simplified instructions
* ⚙️ **5-Stage Pipeline** (IF, ID, EX, MEM, WB)
* 🔁 **Cycle-by-Cycle Simulation** with verbose logs
* 📄 **Hex-Based Instruction Input** for easy program editing
* 🛠️ **Modular Python Design** with clear file separation

---

## 📂 File Structure

| File                 | Description                                    |
| -------------------- | ---------------------------------------------- |
| `main.py`            | 🔁 Starts and runs the full simulation         |
| `cpu_pipeline.py`    | ⛓️ Coordinates instruction flow across stages  |
| `register_file.py`   | 🧠 Implements the general-purpose register set |
| `memory.py`          | 💾 Handles both instruction and data memory    |
| `instruction_set.py` | 🗂️ Defines ISA formats, types, and opcodes    |
| `test_program.txt`   | 🧪 Sample instruction program (hex-encoded)    |
| `utils.py`           | 🔧 Utility functions and formatting tools      |

---

## 🧠 Pipeline Execution

Each instruction flows through the 5 classic MIPS-style pipeline stages:

```
Cycle 1 → IF   : Fetch instruction at PC  
Cycle 2 → ID   : Decode and read registers  
Cycle 3 → EX   : Perform ALU operations  
Cycle 4 → MEM  : Access data memory (lw/sw)  
Cycle 5 → WB   : Write result to register file  
```


---

## 📜 Supported Instructions 

| Mnemonic | Type | Description                              |
| -------- | ---- | ---------------------------------------- |
| `addi`   | R/I  | Add immediate                            |
| `ori`    | R/I  | Bitwise OR immediate                     |
| `and`    | R    | Bitwise AND                              |
| `sub`    | R    | Subtract                                 |
| `lw`     | I    | Load word from memory                    |
| `sw`     | I    | Store word to memory                     |
| `beq`    | I    | Branch if equal                          |
| `j`      | J    | Unconditional jump                       |
| `bgtz`   | I    | Branch if greater than zero              |
| `r_type` | R    | Generic R-type fallback (AND, SUB, etc.) |

---

## 🧩 Pipeline State Table (Cycles 1–5)

This figure shows instruction progression across the Writeback, Execute, and Fetch stages during the first five cycles of execution:

<p align="center">
  <img src="https://github.com/user-attachments/assets/66ca1e13-ab1b-4d84-b7fa-348522a57e30" alt="Pipeline State Table" width="600">
</p>

---

## 🧾 Final Register File Snapshot

Below is the final register file content with hexadecimal values and notes showing how each register was initialized or modified:

<p align="center">
  <img src="https://github.com/user-attachments/assets/cc211ff5-9bbe-4e95-af1d-4c29449b2f3c" alt="Register File Table" width="500">
</p>

---

## 📊 Full Pipeline and Register Trace

This table captures every cycle of execution, showing pipeline state and full register file contents across all 23 clock cycles:

|   Cycle | WB Stage                      | EXEC Stage        | FETCH (PC)       | Reg $00–$03                                            | Reg $04–$07                                            | Reg $08–$11                                            | Reg $12–$15                                            |
|--------:|:------------------------------|:------------------|:-----------------|:-------------------------------------------------------|:-------------------------------------------------------|:-------------------------------------------------------|:-------------------------------------------------------|
|       1 | NOP or empty                  | —                 | 0x000000         | $00=0x000000, $01=0x000000, $02=0x000000, $03=0x000000 | $04=0x000000, $05=0x000000, $06=0x000000, $07=0x000000 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|       2 | opcode=None, dest=$None       | —                 | 0x000003         | $00=0x000000, $01=0x000000, $02=0x000000, $03=0x000000 | $04=0x000000, $05=0x000000, $06=0x000000, $07=0x000000 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|       3 | opcode=None, dest=$None       | opcode=1 (addi)   | 0x000006         | $00=0x000000, $01=0x000000, $02=0x000000, $03=0x000000 | $04=0x000000, $05=0x000000, $06=0x000000, $07=0x000000 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|       4 | opcode=None, dest=$None       | opcode=1 (addi)   | 0x000009         | $00=0x000000, $01=0x000000, $02=0x000000, $03=0x000000 | $04=0x000000, $05=0x000000, $06=0x000000, $07=0x000000 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|       5 | opcode=1, dest=$1, result=1   | opcode=2 (ori)    | 0x00000C         | $00=0x000000, $01=0x000001, $02=0x000000, $03=0x000000 | $04=0x000000, $05=0x000000, $06=0x000000, $07=0x000000 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|       6 | opcode=1, dest=$2, result=1   | opcode=1 (addi)   | 0x00000F         | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x000000 | $04=0x000000, $05=0x000000, $06=0x000000, $07=0x000000 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|       7 | opcode=2, dest=$3, result=255 | opcode=1 (addi)   | 0x000012         | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x000000, $05=0x000000, $06=0x000000, $07=0x000000 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|       8 | opcode=1, dest=$4, result=15  | opcode=1 (addi)   | 0x000015         | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000000, $07=0x000000 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|       9 | opcode=1, dest=$5, result=0   | opcode=1 (addi)   | 0x000018         | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000000, $07=0x000000 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|      10 | opcode=1, dest=$6, result=16  | opcode=4 (lw)     | 0x00001B         | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000010, $07=0x000000 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|      11 | opcode=1, dest=$7, result=5   | opcode=7 (bgtz)   | 0x00001E         | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000010, $07=0x000005 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|      12 | opcode=4, dest=$8, result=256 | opcode=8 (j)      | JUMP to 0x000027 | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000010, $07=0x000005 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|      13 | opcode=7, dest=$0, result=0   | opcode=1 (addi)   | 0x000027         | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000010, $07=0x000005 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|      14 | opcode=8, dest=$None          | —                 | 0x00002A         | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000010, $07=0x000005 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|      15 | opcode=1, dest=$7, result=4   | opcode=7 (bgtz)   | 0x00002D         | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000010, $07=0x000004 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|      16 | opcode=None                   | opcode=8 (j)      | JUMP to 0x00003C | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000010, $07=0x000004 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|      17 | opcode=7, dest=$0, result=0   | opcode=0 (r_type) | 0x00003C         | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000010, $07=0x000004 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|      18 | opcode=8, dest=$None          | —                 | 0x00003F         | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000010, $07=0x000004 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|      19 | opcode=0, dest=$1, result=0   | opcode=0 (r_type) | 0x000042         | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000010, $07=0x000004 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|      20 | opcode=None                   | opcode=5 (sw)     | HALT at 0x000045 | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000010, $07=0x000004 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|      21 | opcode=0, dest=$4, result=15  | opcode=8 (j)      | JUMP to 0x0007DF | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000010, $07=0x000004 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|      22 | opcode=5, dest=$8, result=13  | —                 | HALT at 0x0007DF | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000010, $07=0x000004 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |
|      23 | opcode=8, dest=$None          | —                 | HALT at 0x0007DF | $00=0x000000, $01=0x000001, $02=0x000001, $03=0x0000FF | $04=0x00000F, $05=0x000000, $06=0x000010, $07=0x000004 | $08=0x000000, $09=0x000000, $10=0x000000, $11=0x000000 | $12=0x000000, $13=0x000000, $14=0x000000, $15=0x000000 |


---

## 🗃️ Final Data Memory Contents

```
0x00000D → 0x000000
```
