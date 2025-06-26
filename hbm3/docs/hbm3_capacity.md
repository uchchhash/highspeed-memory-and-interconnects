# 📦 HBM3 Capacity Breakdown – 32 Gb 16High Configuration

This markdown explains the full capacity hierarchy of an HBM3 DRAM stack based on the JEDEC JESD238 spec, focusing on the 32 Gb 16High configuration.

---

## 🧮 Core Parameters

| Parameter           | Value                    |
|---------------------|--------------------------|
| Word Size           | 256 bits (32 bytes)      |
| Column Address Bits | 5 → 32 column slices     |
| Row Address Bits    | 15 → 32,768 rows         |
| Banks per PC        | 64 (6-bit BA + SID)      |
| PCs per Channel     | 2                        |
| Channels per Stack  | 16                       |
| Stack Height        | 16 Dies (16High)         |

---

## 🧩 Calculations from First Principles

### 🟦 1. Bit Cell
- The smallest unit = **1 bit**

---

### 🟦 2. One Column Slice (Prefetch Block)
- Prefetch = **256 bits** = **32 bytes**
- Built from 256 individual cells

---

### 🟦 3. One Row
- 32 column slices × 256 bits = **8,192 bits = 1 KB**
- Each row stores 1 KB → aligns with JEDEC spec page size

---

### 🟦 4. One Bank
- 32,768 rows × 1 KB = **32 MB per bank**

---

### 🟦 5. One Pseudo-Channel (PC)
- 64 banks × 32 MB = **2,048 MB = 16 Gb**

---

### 🟦 6. One Channel (2 PCs)
- 2 × 16 Gb = **32 Gb = 4 GB**

---

### 🟦 7. One Stack (16 Channels)
- 16 × 4 GB = **64 GB**

---

## ✅ Summary Table

| Level              | Quantity                  | Capacity          |
|--------------------|---------------------------|-------------------|
| 1 Cell             | —                         | 1 bit             |
| 1 Column Slice     | 256 cells                 | 256 bits          |
| 1 Row              | 32 Column Slices          | 8,192 bits = 1 KB |
| 1 Bank             | 32K rows × 1 KB           | 32 MB             |
| 1 Pseudo-Channel   | 64 banks × 32 MB          | 2,048 MB = 16 Gb  |
| 1 Channel          | 2 PCs × 16 Gb             | 32 Gb = 4 GB      |
| 1 Stack            | 16 Channels × 4 GB        | 64 GB             |

---

## 📘 Source

- JEDEC JESD238 HBM3 Specification
- DRAM hierarchy and architecture reference models

---

> 📌 Note: 32 Gb 16High means a single stack has 16 vertically stacked dies and each channel supports 32 gigabits of memory. The total capacity per stack = 64 GB.
