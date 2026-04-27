# 6T SRAM Cell with Latch-Based Sense Amplifier

![Technology](https://img.shields.io/badge/Technology-gpdk045%2045nm-green)
![Tool](https://img.shields.io/badge/Tool-Cadence%20Virtuoso-blue)
![Simulator](https://img.shields.io/badge/Simulator-ADE%20L%20Spectre-orange)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

A complete **6T SRAM cell** integrated with a **Latch-Based Sense Amplifier**, designed and simulated in Cadence Virtuoso using **gpdk045 (45nm)** technology. The design covers the full SRAM operation pipeline — Precharge, Write, Cell Storage, and Sense Amplification.

---

## Specifications

| Parameter | Value |
|-----------|-------|
| Supply Voltage (VDD) | 1.0V |
| Technology Node | gpdk045 (45nm) |
| Cell Type | 6T SRAM |
| Sense Amplifier Type | Latch-Based |
| Simulator | Spectre (ADE L) |
| Analysis | Transient |

---

## Design Architecture

The complete SRAM operation is broken into 4 blocks:

```
Precharge (PRE)
      │
      ▼
Write Amplifier (WRITE_AMP)
      │
      ▼
6T SRAM Cell (6T_SRAM)
      │
     BL / BLB
      │
      ▼
Latch-Based Sense Amplifier (SENSE_AMP)
      │
      ▼
    Output
```

| Block | Description |
|-------|-------------|
| **PRE** | Precharges Bit Lines (BL & BLB) to VDD before every read/write cycle |
| **WRITE_AMP** | Drives BL and BLB to write data into the 6T cell |
| **6T_SRAM** | Core storage cell — two cross-coupled inverters + two access transistors |
| **SENSE_AMP** | Latch-based sense amplifier that detects differential voltage on BL/BLB and amplifies to full logic levels |

---

## Folder Structure

```
6T_SRAM_gpdk045/
├── PRE/
│   ├── schematics/       ← Precharge circuit schematic
│   └── simulations/      ← Precharge transient waveforms
├── WRITE_AMP/
│   ├── schematics/       ← Write amplifier schematic
│   └── simulations/      ← Write transient waveforms
├── 6T_SRAM/
│   ├── schematics/       ← 6T cell schematic
│   └── simulations/      ← Read/Write transient waveforms
└── SENSE_AMP/
    ├── schematics/       ← Latch-based SA schematic
    └── simulations/      ← SA output transient waveforms
```

---

## Simulation Results

### Transient — Read & Write Operation
Transient simulation verifying:
- Successful **Write 0→1** and **Write 1→0** operations via Write Amplifier
- Correct **Read** operation with BL/BLB differential development
- **Sense Amplifier** correctly detecting and amplifying differential voltage on BL/BLB to full logic output

### Key Signals Verified
| Signal | Role |
|--------|------|
| BL | Bit Line |
| BLB | Bit Line Bar (complementary) |
| WL | Word Line — enables access transistors |
| Q / QB | Storage nodes of 6T cell |
| SA_EN | Sense Amplifier Enable |
| SA_OUT | Sense Amplifier Output |

---

## Design Notes

- **Precharge circuit** ensures BL and BLB are equalized to VDD before each cycle, reducing read time
- **6T cell** uses two cross-coupled inverters for storage and two NMOS access transistors controlled by WL
- **Latch-based Sense Amplifier** detects small differential voltage developed on BL/BLB during read and regenerates it to full VDD/GND levels rapidly
- Full operation pipeline verified through transient simulation at **VDD = 1.0V**
- Designed using **Cadence Virtuoso** with **Spectre** simulator via **ADE L**

---

## Future Improvements

- [ ] SNM analysis — Hold, Read, Write
- [ ] Process corner simulations (TT, FF, SS, SF, FS)
- [ ] Monte Carlo analysis for offset in Sense Amplifier
- [ ] Full memory array integration (multiple bitcells)
- [ ] Power consumption analysis
