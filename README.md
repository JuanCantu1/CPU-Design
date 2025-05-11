> 📌 **Note:** This project began as a 24-bit RISC processor simulator in Python. It is now being expanded into a full RISC-V implementation using Verilog, targeting the DE1-SoC FPGA platform in Summer 2025.

---

# 🧠 24-bit RISC CPU Simulator

## Project Overview

This project simulates a 24-bit RISC processor with a five-stage pipeline architecture (IF, ID, EX, MEM, WB). Designed entirely in Python, it models instruction execution at the cycle level using a custom 17-instruction ISA. The goal was to understand pipelining by simulating instruction flow, register updates, and memory interactions — all without relying on Verilog (yet).

---

## 💾 Core Features

* 🧱 **Custom 24-bit Instruction Set**
* ⚙️ **5-Stage Pipeline** (Fetch, Decode, Execute, Memory, Writeback)
* 🔁 **Cycle-by-Cycle Logging**
* 📄 **Hex-encoded Instruction Input**
* 🛠️ **Fully Modular Python Design**

---

## 📁 Files Breakdown

| File                 | Description                                   |
| -------------------- | --------------------------------------------- |
| `main.py`            | 🚀 Entry point – runs the pipeline simulation |
| `cpu_pipeline.py`    | 🔧 Manages instruction flow through pipeline  |
| `register_file.py`   | 🧠 Implements register reads/writes           |
| `memory.py`          | 💾 Simulates both instruction and data memory |
| `instruction_set.py` | 🗂️ Instruction format and opcodes            |
| `test_program.txt`   | 🧪 Sample program (hex-encoded instructions)  |
| `utils.py`           | 🧰 Misc. formatting and helper functions      |

---

## 🧠 Instruction Pipeline

Each instruction moves through a five-stage pipeline. There’s no hazard detection or forwarding — just raw instruction flow.



```
Cycle 1: IF     → Fetch instruction at PC
Cycle 2: ID     → Decode and read registers
Cycle 3: EX     → ALU operations
Cycle 4: MEM    → Access memory (lw/sw)
Cycle 5: WB     → Write result to register file
```

---

<details>
<summary>📜 Supported Instructions</summary>

| Mnemonic | Type | Description          |
| -------- | ---- | -------------------- |
| `addi`   | R/I  | Add immediate        |
| `ori`    | R/I  | Bitwise OR immediate |
| `and`    | R    | Bitwise AND          |
| `sub`    | R    | Subtract             |
| `lw`     | I    | Load word from RAM   |
| `sw`     | I    | Store word to RAM    |
| `beq`    | I    | Branch if equal      |
| `j`      | J    | Unconditional jump   |

</details>

---


## 🧪 Sample Output

```
Cycle 1 | Fetch: PC=0x000000
Cycle 2 | Decode: addi $1, $0, 5
Cycle 3 | Execute: $1 = $0 + 5
Cycle 4 | Memory: (NOP)
Cycle 5 | Writeback: Reg[1] = 5
```

---
