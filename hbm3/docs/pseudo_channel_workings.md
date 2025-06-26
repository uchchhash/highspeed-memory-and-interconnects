
# 🚀 HBM3 Pseudo Channel Operation Breakdown

This document explains the inner workings of **HBM3 pseudo channels** using the waveform diagram from the JEDEC specification (Figure 2). It breaks down timing, commands, and signal activity across the pseudo-channels PC0 and PC1.

---

## 🧠 What is a Pseudo Channel (PC)?

Each HBM3 **channel** is split into **two pseudo channels** for independent access:
- **PC0** → DQ[31:0]
- **PC1** → DQ[63:32]

This enhances parallelism and increases bandwidth utilization.

---

## 🔄 Pseudo Channel Timeline Explained

### 1. ⏱ Clocking

- **CK_t / CK_c**: Differential clocks for timing reference
- HBM3 uses DDR (Double Data Rate): transfers on both rising and falling edges

---

### 2. 🧩 ACTIVATE Commands

#### ACTIVATE PC0
- Brings an entire row into the row buffer from memory cells.
- Delay required before issuing READ: **tRCD (Row to Column Delay)**.

#### ACTIVATE PC1
- Activates a row for PC1 independently, respecting **tRRD** (ACT to ACT delay).

---

### 3. 🎯 READ Commands

#### READ PC0
- Issued after tRCD from ACTIVATE.
- Pulls 256 bits (32 bytes) in 8 bursts (BL8) through DQ[31:0].

#### READ PC1
- Independent of PC0, uses DQ[63:32].
- Starts after its own tRCD delay.

**Read Latency (RL)**: Time from READ command to data output.

---

### 4. 📡 Data Output

Each PC transfers data in bursts:

| Pseudo-Channel | DQ Bus       | Transfer | Data per Burst |
|----------------|--------------|----------|----------------|
| PC0            | DQ[31:0]     | BL8      | 256 bits       |
| PC1            | DQ[63:32]    | BL8      | 256 bits       |

---

### 5. 🔁 Multiple Activates & Reads

- **ACTIVATE to ACTIVATE (same PC)**: Allowed after tRRD if accessing different banks.
- **READ to READ (same PC)**: Can happen back-to-back if tCCD spacing is met.
- **Second READ often skips extra tRCD**: If targeting already open row (row hit).

---

### 6. 🔚 PRECHARGE Commands

- **PRECHARGE** closes a row after tRAS (minimum active time).
- **tRTP** is required between last READ and PRECHARGE.

You may see:
- One PRECHARGE per row
- Multiple PRECHARGEs if multiple banks in PC1 are accessed

---

## ⏱️ Timing Summary

| Parameter | Meaning                              |
|----------:|--------------------------------------|
| `tRCD`    | ACTIVATE → READ                      |
| `tRRD`    | ACTIVATE → ACTIVATE (diff banks)     |
| `tRAS`    | Row Active time                      |
| `tRTP`    | READ → PRECHARGE                     |
| `RL`      | READ Latency                         |
| `BL8`     | Burst Length = 8 × 32-bit            |

---

## ✅ Operation Sequence

```text
T0  : ACTIVATE PC0
T1  : ACTIVATE PC1 (after tRRD)
T2  : READ PC0 (after tRCD)
T3  : Data Da–Da+7 output on DQ[31:0] after RL
T4  : READ PC0 again (new row)
T5  : READ PC1
T6  : Data Db–Db+7 output on DQ[63:32] after RL
T7  : PRECHARGE PC0
T8  : PRECHARGE PC1
```

---

## 🔧 Why Pseudo Channels Matter

- Allow independent access to memory banks
- Boost throughput using parallelism
- Reduce contention and interleaving delays

---

## 📚 Reference

- JEDEC JESD238 HBM3 DRAM Standard, Figure 2 (Pseudo Channel Operation)
