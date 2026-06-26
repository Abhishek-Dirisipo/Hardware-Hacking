---
title: "CH341A USB Programmer: The Complete Ubuntu Handbook"
subtitle: "Firmware Extraction, Analysis, and Recovery for Hardware Security Engineers"
version: "1.0"
date: "2026"
target_os: "Ubuntu 24.04 LTS"
primary_tool: "Flashrom 1.6+"
---

# CH341A USB Programmer: The Complete Ubuntu Handbook

**Firmware Extraction, Analysis, and Recovery for Hardware Security Engineers**

---

Version 1.0 · Ubuntu 24.04 LTS · Flashrom 1.6+

---

## Legal Disclaimer

This handbook is provided for educational purposes and authorized security research only. All techniques described herein must only be applied to devices you own outright, or to devices for which you have received explicit written authorization from the owner. Unauthorized interception, modification, or extraction of firmware may violate local, national, or international laws including but not limited to the Computer Fraud and Abuse Act (CFAA), the Digital Millennium Copyright Act (DMCA), and equivalent legislation in other jurisdictions. The authors and contributors accept no liability for misuse of any information contained in this document. Practice responsible disclosure. When you discover vulnerabilities, notify the affected vendor before public disclosure.

---

## Table of Contents

- [Preface](#preface)
- [Chapter 1: Introduction to the CH341A](#chapter-1-introduction-to-the-ch341a-usb-programmer)
  - 1.1 Purpose and Background
  - 1.2 Supported Protocols
  - 1.3 Common Use Cases in Hardware Security
  - 1.4 CH341A vs Other Programmers
  - 1.5 Key Limitations
- [Chapter 2: Hardware Deep Dive](#chapter-2-hardware-deep-dive)
  - 2.1 Board Variants (Black, Green, Blue)
  - 2.2 The 5V Voltage Problem
  - 2.3 The 3.3V Fix
  - 2.4 Pinout Reference
  - 2.5 Identifying Pin 1
  - 2.6 Jumper Settings
  - 2.7 Clip Types
  - 2.8 ESD Safety
- [Chapter 3: Ubuntu Installation and System Setup](#chapter-3-ubuntu-installation-and-system-setup)
  - 3.1 System Prerequisites
  - 3.2 Installing Flashrom
  - 3.3 Supporting Tools
  - 3.4 USB Device Recognition
  - 3.5 Permissions and udev Rules
  - 3.6 Verifying the Installation
- [Chapter 4: Flashrom Complete Reference](#chapter-4-flashrom-complete-reference)
  - 4.1 Architecture Overview
  - 4.2 The Programmer String
  - 4.3 Chip Detection
  - 4.4 Verbose Mode
  - 4.5 Reading Firmware
  - 4.6 Writing Firmware
  - 4.7 Verifying Firmware
  - 4.8 Erasing
  - 4.9 Layout Files and Partial Operations
  - 4.10 JEDEC ID Reading
  - 4.11 Status Register Operations
  - 4.12 Complete Flag Reference Table
- [Chapter 5: SPI NOR Flash Deep Dive](#chapter-5-spi-nor-flash-deep-dive)
  - 5.1 SPI Protocol Theory
  - 5.2 JEDEC ID Structure
  - 5.3 Major Manufacturers Reference
  - 5.4 Physical Package Reference
  - 5.5 Detection Workflow
  - 5.6 Voltage Requirements
- [Chapter 6: I²C EEPROM Operations](#chapter-6-ic-eeprom-operations)
- [Chapter 7: Complete Firmware Dump Workflow](#chapter-7-complete-firmware-dump-workflow)
- [Chapter 8: Writing Firmware Safely](#chapter-8-writing-firmware-safely)
- [Chapter 9: Firmware Analysis](#chapter-9-firmware-analysis)
- [Chapter 10: Troubleshooting](#chapter-10-troubleshooting)
- [Chapter 11: Electrical Troubleshooting](#chapter-11-electrical-troubleshooting-with-a-multimeter)
- [Chapter 12: Logic Analyzer Integration](#chapter-12-logic-analyzer-integration)
- [Chapter 13: Real-World Examples](#chapter-13-real-world-examples)
- [Chapter 14: Best Practices](#chapter-14-best-practices-for-professional-hardware-security-work)
- [Chapter 15: Flashrom Error Reference](#chapter-15-flashrom-error-reference)
- [Chapter 16: Quick Reference](#chapter-16-quick-reference)
- [Appendix A: Pandoc Conversion](#appendix-a-converting-this-handbook-to-pdf-and-docx)
- [Appendix B: Flashrom Verification Notes](#appendix-b-flashrom-command-verification-notes)
- [Appendix C: Supported Chip Reference](#appendix-c-supported-chip-reference)

---

## Preface

This handbook exists because hardware security research is inseparable from firmware. Every router you pentest, every IP camera you audit, every embedded Linux device you reverse engineer has its firmware stored on a small, 8-pin flash chip. To read that chip — to extract the firmware, inspect it, modify it, and recover the device if something goes wrong — you need the right tool, the right knowledge, and a systematic workflow. The CH341A USB programmer provides the tool. This handbook provides the rest.

The CH341A is a USB interface chip produced by Nanjing Qinheng Microelectronics (QinHeng Electronics). It was originally designed as a USB-to-serial bridge for embedded development. Over time, the hardware community discovered that it could also function as a low-cost SPI NOR flash programmer when paired with the right software. Today it is a staple of hardware security labs worldwide — not because it is the best programmer available, but because it is inexpensive, widely available, and works reliably with Flashrom on Linux.

This handbook is written in the tradition of O'Reilly technical books: deep, precise, command-heavy, and complete. Every chapter explains the *why* before the *how*. Every command is followed by its expected output. Every dangerous operation is explicitly flagged. If a limitation exists, it is stated plainly rather than glossed over with a vague workaround.

### Who This Handbook Is For

This handbook is written for several overlapping audiences:

**Hardware penetration testers** who need to extract and analyze firmware from client devices during authorized engagements. You will find the complete workflow from physical connection to verified firmware dump, plus analysis techniques for finding credentials, certificates, and vulnerabilities in extracted images.

**Firmware analysts and reverse engineers** who receive firmware images from colleagues or download them from manufacturer websites. Even without physical hardware, the analysis chapters in this handbook — binwalk, strings, hexdump, filesystem extraction — apply directly to your work.

**Embedded Linux engineers** who need to recover a bricked device, update BIOS firmware on a system without an internal programmer, or inspect the firmware of a device under development.

**Beginners** who have purchased a CH341A programmer and want to understand not just the commands, but the underlying hardware, protocols, and reasoning that make those commands work.

### What Is Not Covered

This handbook covers SPI NOR flash programming and I²C EEPROM operations via the CH341A on Ubuntu Linux. It does not cover:

- **JTAG debugging** — a completely different protocol requiring different hardware (OpenOCD, J-Link, CMSIS-DAP)
- **UART console access** — while CH341A can function as a USB-to-serial adapter, that mode is distinct from its programmer role and is not documented here
- **eMMC programming** — requires specialized hardware (ISP adapters, dedicated eMMC programmers)
- **UFS storage** — not supported by CH341A
- **SPI NAND flash** — CH341A/Flashrom has very limited SPI NAND support; this handbook focuses on SPI NOR

### A Note on Safety

The single most common cause of chip damage when using a CH341A is voltage mismatch. Most unmodified CH341A boards supply 5 volts on their data lines. Most modern SPI NOR flash chips operate at 3.3 volts. Applying 5V to a 3.3V-only chip can cause permanent, irreversible damage within milliseconds. Chapter 2 documents this problem in full and explains the hardware modifications required to make the CH341A safe for modern flash chips. Do not skip that chapter.

### How to Use This Handbook

Read Chapters 1 through 3 before touching any hardware. They cover the theory, hardware safety, and software installation that everything else depends on. Chapter 4 is the Flashrom reference — return to it whenever you need to look up a specific command or flag. Chapters 7 and 8 document the complete dump and write workflows that you will use on every real engagement. The remaining chapters are reference material: consult them as needed.

---

## Chapter 1: Introduction to the CH341A USB Programmer

### 1.1 Purpose and Background

The CH341A is a multi-function USB interface chip manufactured by Nanjing Qinheng Microelectronics Co., Ltd. (also known as QinHeng Electronics), a Chinese semiconductor company that produces a range of USB bridge chips for embedded applications. The CH341A specifically supports USB 2.0 Full Speed (12 Mbps) and can operate in several modes: USB-to-serial (UART), USB-to-parallel, USB-to-I²C, and USB-to-SPI.

When connected to a host PC via USB, the CH341A presents itself with USB Vendor ID `0x1A86` and Product ID `0x5512` when operating in programmer mode. This VID:PID pair is what Flashrom uses to identify and communicate with the device via libusb. (Note: when operating as a USB-to-serial adapter, the same chip presents as `0x1A86:0x7523` — a different PID that is handled by the `ch341` kernel serial driver rather than by Flashrom.)

The CH341A became popular in the hardware security community for a straightforward reason: it costs between $3 and $15 USD on most electronics marketplaces, comes bundled with a SOIC-8 test clip and ribbon cable, and works out of the box with Flashrom on Linux. For a tool that can extract the complete firmware image from a router, IP camera, or laptop BIOS chip, that price-to-capability ratio is remarkable.

The typical CH341A programmer board includes:
- The CH341A chip itself (SSOP-28 package)
- A 25-series SPI flash ZIF socket (for DIP-8 through-hole chips)
- A header for the SOIC-8 clip cable
- A USB Type-A connector
- Voltage selection circuitry (varies by board revision)
- Status LEDs

Understanding what the CH341A *is* — a USB bridge chip, not a dedicated flash programmer ASIC — helps explain both its capabilities and its limitations.

---

### 1.2 Supported Protocols

#### 1.2.1 SPI (Serial Peripheral Interface)

SPI is the primary use case for the CH341A in hardware security work. SPI NOR flash chips are the most common firmware storage medium in embedded systems. They are found in routers, IP cameras, smart TVs, IoT devices, laptop BIOS chips, industrial controllers, and countless other products.

The CH341A implements SPI master mode, allowing it to communicate with any SPI-compatible flash chip using the standard SPI NOR command set (READ, FAST_READ, PAGE_PROGRAM, SECTOR_ERASE, CHIP_ERASE, WREN, RDSR, RDID, etc.). Flashrom abstracts all of this through its `ch341a_spi` programmer backend.

**What CH341A SPI supports:**
- SPI NOR flash (the overwhelming majority of use cases)
- Chips from 64 KB to 128 MB (and some 256 MB chips in newer Flashrom versions)
- Standard SPI (single-bit MOSI/MISO)

**What CH341A SPI does NOT reliably support:**
- SPI NAND flash (different command set, bad block management, ECC requirements)
- Dual SPI or Quad SPI (DSPI/QSPI) — CH341A uses standard single-bit SPI only

#### 1.2.2 I²C (Inter-Integrated Circuit)

The CH341A can also operate as an I²C master, enabling communication with 24Cxx series EEPROM chips. However, I²C support via the CH341A requires different software than Flashrom — specifically `i2c-tools` and `eeprog`. Flashrom's `ch341a_spi` programmer string does not support I²C. This distinction is critical and is documented in full in Chapter 6.

Common I²C EEPROM targets in hardware security:
- 24C02: 256 bytes — configuration data, serial numbers
- 24C256: 32 KB — larger configuration storage
- 24C512: 64 KB — firmware supplements, calibration data

#### 1.2.3 Parallel (Legacy)

The CH341A supports a parallel interface mode for some legacy parallel EEPROM chips (27-series and similar). This mode is rarely used in modern hardware security work and is not documented in this handbook. Flashrom does not support the CH341A in parallel mode.

#### 1.2.4 What the CH341A Cannot Do

Be explicit about these limitations before spending time on hardware that will not work:

| Protocol | CH341A Support | Alternative Tool |
|---|---|---|
| JTAG | ❌ None | OpenOCD + J-Link / FT2232H |
| UART (firmware programmer) | ❌ None | Dedicated UART bootloader tools |
| eMMC | ❌ None | ISP adapter + dedicated tool |
| UFS | ❌ None | Specialized UFS programmer |
| SPI NAND | ⚠️ Very limited | Dedicated NAND programmer |
| SPI NOR (standard) | ✅ Excellent | — |
| I²C EEPROM | ✅ With i2c-tools | — |

---

### 1.3 Common Use Cases in Hardware Security

#### 1.3.1 Laptop BIOS/UEFI Firmware Extraction

Modern x86 laptops store their BIOS/UEFI firmware in an SPI NOR flash chip — typically a Winbond W25Q128 (16 MB) or Macronix MX25L12835F (16 MB). These chips are usually SOIC-8 packages soldered directly to the motherboard. The CH341A with a SOIC-8 clip can read these chips in-circuit (with the laptop powered off) or out-of-circuit (after desoldering). BIOS extraction is used for: firmware analysis, write protection bypass, password removal, ME firmware extraction, and custom BIOS installation.

#### 1.3.2 Router Firmware Extraction and Modification

Home and small business routers are among the most common CH341A targets. The SPI NOR flash chip (typically 4 MB to 16 MB) contains the complete router firmware: U-Boot bootloader, Linux kernel, and SquashFS root filesystem. Extracting router firmware enables: finding default credentials, identifying unpatched vulnerabilities, extracting private keys, modifying firmware for custom features, and recovering routers bricked by failed OTA updates.

#### 1.3.3 IP Camera Firmware Analysis

IP cameras running embedded Linux frequently contain hardcoded credentials, private keys, and insecure network services. Extracting the firmware via CH341A provides direct access to `/etc/passwd`, `/etc/shadow`, SSL certificates, RTSP stream credentials, and the application binaries responsible for camera functionality.

#### 1.3.4 IoT Device Reverse Engineering

The broad category of IoT devices — smart plugs, thermostats, door locks, industrial sensors — almost universally uses SPI NOR flash for firmware storage. These devices often run stripped-down Linux or RTOS firmware with minimal security, making firmware extraction via CH341A a high-value technique during security assessments.

#### 1.3.5 Bricked Device Recovery

When an OTA firmware update fails, or when an experimental custom firmware bricks a device, the CH341A provides a recovery path that bypasses the device's own bootloader. By writing a known-good firmware image directly to the flash chip, a bricked device can often be recovered within minutes.

---

### 1.4 CH341A vs Other Programmers

| Tool | Protocol Support | SPI Speed | Default Voltage | Linux Support | Approx. Price | Pentest Suitability |
|---|---|---|---|---|---|---|
| CH341A | SPI NOR, I²C | ~1 MHz | 5V (⚠️ mod required) | Excellent (Flashrom) | $3–$15 | High (cheap, portable) |
| Bus Pirate v3 | SPI, I²C, UART, JTAG | ~1 MHz | 3.3V/5V selectable | Good (Flashrom + OpenOCD) | $30–$60 | High (versatile) |
| Bus Pirate v4 | SPI, I²C, UART, 1-Wire | ~3 MHz | 3.3V/5V selectable | Good | $50–$80 | High |
| FT2232H module | SPI, I²C, UART, JTAG | ~30 MHz | 3.3V (with buffer) | Excellent | $15–$40 | Very high (fast, versatile) |
| Raspberry Pi (bit-bang) | SPI, I²C, UART | ~5 MHz | 3.3V | Native | $35–$80 | Medium (bulky) |
| DirtyPCBs GreatFET | SPI, I²C, USB, JTAG | Varies | 3.3V | Good | $100+ | High (extensible) |

The CH341A wins on cost and availability. For dedicated SPI NOR flash work, it is entirely adequate. For engagements requiring JTAG or high-speed programming, choose an FT2232H-based solution.

---

### 1.5 Key Limitations

Understanding the CH341A's limitations before deployment prevents wasted time and damaged hardware:

**Voltage:** The most critical limitation. Unmodified CH341A boards supply 5V on data lines. Applying 5V to a 3.3V-only chip causes permanent damage. Hardware modification is required before use with modern flash chips. (See Chapter 2.)

**Speed:** The CH341A operates at approximately 1 MHz SPI clock. Dedicated programmers using FT2232H can reach 30 MHz. For a 16 MB chip, this translates to roughly 130 seconds for a full read on CH341A vs. under 10 seconds on a fast programmer. In practice, this is acceptable for security work.

**SPI NAND:** Not reliably supported. SPI NAND requires ECC handling and bad block management that Flashrom's CH341A backend does not implement for general use.

**Large chips:** Chips larger than 128 Mb (16 MB) may have limited or untested support depending on Flashrom version. Always verify with `flashrom -L` before attempting to program an unusual large chip.

**I²C speed:** The CH341A's I²C implementation is limited to Standard Mode (100 kHz). Some chips require Fast Mode (400 kHz) for reliable operation.

**Driver conflicts:** The `ch341` kernel module (used for serial mode) can conflict with Flashrom's libusb access in programmer mode. This is a common issue that Chapter 3 and Chapter 10 document in full.

---

### 1.6 Chapter Summary

The CH341A is a low-cost USB-to-SPI/I²C bridge chip that has become a standard tool in hardware security labs. Its VID:PID in programmer mode is `1A86:5512`. Its primary capability is SPI NOR flash programming via Flashrom. It cannot perform JTAG, UART firmware programming, eMMC, or UFS operations. Its critical limitation is a 5V default output voltage that requires hardware modification before use with modern 3.3V flash chips. With that modification applied, it is a reliable, portable, and affordable tool for firmware extraction, analysis, and recovery.

---

## Chapter 2: Hardware Deep Dive

### Purpose

Before connecting the CH341A to any chip, you must understand the hardware completely. This chapter documents the physical variants, the voltage hazard that has destroyed countless flash chips, the required hardware modifications, pinout diagrams, and connection procedures. Read this chapter entirely before touching hardware.

### 2.1 Board Variants

The CH341A programmer is manufactured by numerous Chinese electronics vendors and sold under many brand names. The boards are commonly differentiated by PCB color: black, green, and blue. While the underlying CH341A chip is identical across all variants, the surrounding circuitry — particularly the voltage regulation — differs between board revisions.

#### 2.1.1 Black CH341A

The black CH341A is the most common variant found in hardware security labs. It typically features:

- A black PCB with gold or silver trace finish
- A 25-series ZIF socket for DIP-8 through-hole SPI NOR flash chips
- A 24-series ZIF socket for DIP-8 through-hole I²C EEPROM chips (labeled separately, usually with a mode selection jumper)
- A 1.27mm-pitch header for the SOIC-8 clip cable
- Green or red status LEDs
- A USB Type-A male connector (plugs directly into host PC)

The black board is manufactured in several hardware revisions. Early revisions (often found on older boards) are the most problematic because they supply 5V directly from USB on the data lines with no voltage regulation option. Some later black board revisions added a voltage selection circuit — but it may not function correctly without modification (see Section 2.3).

#### 2.1.2 Green CH341A

The green CH341A uses the same CH341A chip and the same VID:PID (`1A86:5512`). From Flashrom's perspective, it is functionally identical to the black variant. The green board is often sold as a "programmer + adapter" kit with multiple ZIF socket adapters.

Key notes for the green variant:
- Some green board revisions include a 3.3V LDO regulator, but the output may still be misrouted, supplying 5V to the programming pins. Verify with a multimeter before use.
- Flashrom compatibility is identical to the black board.
- ZIF socket and clip header pinout is identical.

#### 2.1.3 Blue CH341A

Blue CH341A boards are less common. Some blue variants are marketed specifically as "3.3V safe" and include a pre-installed voltage regulator modification. However, these marketing claims are not always accurate. The only reliable way to know the output voltage of any CH341A board is to measure it with a multimeter (see Section 2.3.4).

#### 2.1.4 Variant Comparison Table

| Variant | Color | ZIF Socket | Clip Header | Default VCC | Voltage Mod Needed | Notes |
|---|---|---|---|---|---|---|
| Black (early rev) | Black | Yes (DIP-8) | Yes (SOIC-8) | 5V ⚠️ | Yes | Most common in labs |
| Black (late rev) | Black | Yes (DIP-8) | Yes (SOIC-8) | 5V ⚠️ | Yes | Selection jumper may not work |
| Green | Green | Yes (DIP-8) | Yes (SOIC-8) | 5V ⚠️ | Usually | Verify with multimeter |
| Blue | Blue | Yes (DIP-8) | Yes (SOIC-8) | 5V or 3.3V | Verify | "3.3V safe" claims unreliable |

**Bottom line:** Regardless of board color or marketing claims, always measure the output voltage with a multimeter before connecting to any chip.

---

### 2.2 The 5V Voltage Problem

> **🔴 DANGER:** This is the most important safety section in the handbook. Applying 5V to a 3.3V-only chip will cause immediate and permanent damage. The chip will not show obvious signs of failure until it stops working entirely. There is no recovery from overvoltage damage.

#### 2.2.1 Why CH341A Defaults to 5V

The CH341A chip itself is a 5V device. USB bus voltage is 5V. In the most basic CH341A programmer implementations, the SPI data lines (CLK, CS, MOSI, MISO) are driven directly from the CH341A's I/O pins at 5V logic levels. Additionally, the VCC power supplied to the target chip via the programmer header may be taken directly from the USB 5V rail.

This creates two distinct voltage problems:
1. **VCC overvoltage:** The programmer supplies 5V to the target chip's power supply pin (VCC), exceeding the maximum rating of 3.3V-only chips.
2. **I/O overvoltage:** Even if VCC is correct, the SPI data lines are driven at 5V logic levels, which can exceed the I/O maximum voltage rating of 3.3V chips even if their VCC is correct.

#### 2.2.2 Which Chips Are Affected

Almost all SPI NOR flash chips manufactured after approximately 2010 are 3.3V-only devices. Their absolute maximum VCC rating is typically 3.6V. Their absolute maximum I/O voltage is typically VCC + 0.3V = 3.6V.

Common 3.3V-only chips that will be damaged by 5V:
- Winbond W25Q series (W25Q16, W25Q32, W25Q64, W25Q128, W25Q256)
- Macronix MX25L series
- GigaDevice GD25Q series
- ISSI IS25LP series
- Micron N25Q series
- Most chips manufactured after 2005

Some older chips are 5V tolerant:
- Winbond W25X series (some variants)
- Atmel AT25DF series (5V tolerant variants exist, but verify the specific part number)
- Very old 27-series EPROMs

**Never assume a chip is 5V tolerant.** Look up the specific part number's datasheet and read the absolute maximum ratings section.

#### 2.2.3 Failure Mode

When 5V is applied to a 3.3V-only chip, the following sequence typically occurs:

1. The chip's internal protection diodes conduct, drawing excessive current
2. Internal gate oxides in the CMOS circuitry are stressed beyond their breakdown voltage
3. Latch-up may occur, causing the chip to draw large amounts of current
4. Permanent damage to internal circuitry — the chip becomes non-functional

The chip may feel warm or hot to the touch during this process. In many cases, the damage is not immediately apparent — the chip may still respond to JEDEC ID queries but return corrupted data on reads, or it may fail entirely on the next power cycle.

---

### 2.3 The 3.3V Fix

There are three approaches to making the CH341A safe for 3.3V chips. They are listed in order of recommendation.

#### 2.3.1 Hardware Modification: LDO Voltage Regulator (Recommended)

This is the most reliable fix. It involves replacing or supplementing the power path so that the VCC supplied to the target chip is 3.3V, and the I/O lines are also 3.3V.

**Required materials:**
- AMS1117-3.3 LDO voltage regulator (SOT-223 package, ~$0.50)
- Soldering iron and solder
- Flux
- Multimeter for verification

**Conceptual procedure (verify against your specific board's schematic):**

```text
Before modification:
USB 5V ──────────────────────── VCC header pin (5V to chip)
                    │
                 CH341A (5V I/O) ── SPI data lines (5V logic)

After modification:
USB 5V ── [AMS1117-3.3] ─────── VCC header pin (3.3V to chip)
                    │
                 CH341A now receives 3.3V VCC
                 CH341A I/O pins follow VCC = 3.3V logic
```

The exact implementation varies by board revision. On most black CH341A boards:
1. Locate the voltage regulator or the VCC trace feeding the header
2. If there is an existing regulator, check if it is a 3.3V type (AMS1117-3.3 is marked "1117 3.3" or "AMS 3.3")
3. If it is a 5V regulator or direct connection, replace it with an AMS1117-3.3
4. Verify the output voltage at the header pins with a multimeter before connecting any chip

> **⚠️ CAUTION:** This modification requires soldering skills. If you are not comfortable with surface-mount soldering, use a pre-modified board or a level-shifter approach instead.

#### 2.3.2 Level Shifter Approach

A bidirectional level shifter board (based on the BSS138 MOSFET or similar) can be inserted between the CH341A headers and the target chip. The high-voltage side connects to the CH341A (5V), and the low-voltage side connects to the target chip (3.3V). This approach works without hardware modification to the CH341A itself.

```text
CH341A (5V) ──[Level Shifter HV]──[Level Shifter LV]── Target chip (3.3V)
```

The disadvantage is the additional wiring complexity and potential for signal integrity issues at higher SPI speeds.

#### 2.3.3 Pre-Modified Boards

Some vendors sell CH341A boards advertised as "3.3V ready" with the voltage modification pre-applied. These can be used, but should be verified with a multimeter before first use regardless of vendor claims.

#### 2.3.4 Verification After Modification

Before connecting any chip, verify the output voltage:

```text
Procedure:
1. Connect CH341A to USB (no chip connected to clip)
2. Set multimeter to DC voltage, 20V range
3. Place RED probe on VCC pin of SOIC-8 header (pin 8 position)
4. Place BLACK probe on GND pin of SOIC-8 header (pin 4 position)
5. Read voltage on multimeter display

Acceptable: 3.2V – 3.4V ✅
Dangerous:  4.5V – 5.2V 🔴 DO NOT USE — apply fix first
```

---

### 2.4 Pinout Reference

#### 2.4.1 SOIC-8 Package Pinout (SPI NOR Flash)

The SOIC-8 (Small Outline Integrated Circuit, 8-pin) is the most common package for SPI NOR flash chips. Understanding the pinout is essential for correct clip orientation.

```text
         SOIC-8 Package — Top View
         (Pin 1 identified by dot or chamfer)

          ●
    ┌─────┴──────────┐
 1 ─┤ CS#        VCC ├─ 8
 2 ─┤ MISO      HOLD ├─ 7
 3 ─┤ WP         CLK ├─ 6
 4 ─┤ GND       MOSI ├─ 5
    └────────────────┘

Pin 1 = CS# (Chip Select, active low)
Pin 2 = MISO (Master In Slave Out — data FROM chip TO host)
Pin 3 = WP# (Write Protect, active low — pull HIGH to allow writes)
Pin 4 = GND (Ground)
Pin 5 = MOSI (Master Out Slave In — data FROM host TO chip)
Pin 6 = CLK (SPI Clock — generated by master/CH341A)
Pin 7 = HOLD# (Hold, active low — pull HIGH for normal operation)
Pin 8 = VCC (Power supply — 3.3V for most modern chips)
```

**Pin descriptions:**

- **CS# (Pin 1):** Chip Select. Must be pulled LOW to activate the chip. The CH341A controls this line. During idle, CS# is HIGH (chip deselected). When a command is being sent, CS# goes LOW for the duration of the transaction.

- **MISO (Pin 2):** Master In Slave Out. Data flows FROM the flash chip TO the CH341A on this line. When you read firmware, the chip sends bytes on MISO.

- **WP# (Pin 3):** Write Protect. When this pin is LOW, the chip's write protection is enabled and program/erase operations are blocked. Many PCBs pull this pin HIGH via a resistor (10kΩ to 100kΩ to VCC) to allow writes. Some boards pull it LOW permanently to prevent accidental writes. Check with a multimeter.

- **GND (Pin 4):** Ground reference. Must be connected.

- **MOSI (Pin 5):** Master Out Slave In. Data flows FROM the CH341A TO the flash chip. Commands and write data travel on MOSI.

- **CLK (Pin 6):** SPI Clock. Generated by the CH341A master. Data is sampled on clock edges according to the SPI mode (most flash chips use Mode 0 or Mode 3).

- **HOLD# (Pin 7):** Hold. When asserted LOW, pauses the current SPI transfer without deselecting the chip. Pull HIGH for normal operation. Some chips label this pin as RESET# instead.

- **VCC (Pin 8):** Power supply voltage. **3.3V for virtually all modern SPI NOR flash chips.** This is the pin where you measure voltage with a multimeter to verify your CH341A is supplying the correct voltage.

#### 2.4.2 SOIC-16 Pinout (Wider Packages)

Some flash chips, particularly those with Dual or Quad SPI interfaces, come in SOIC-16 packages. The SOIC-16 has 16 pins and is physically wider than SOIC-8.

```text
         SOIC-16 Package — Top View

          ●
    ┌─────┴──────────────────┐
 1 ─┤ /S (CS#)        VCC   ├─ 16
 2 ─┤ DQ1 (MISO)      /HOLD ├─ 15
 3 ─┤ /WP              DQ2  ├─ 14  (Quad SPI DQ2)
 4 ─┤ GND              DQ3  ├─ 13  (Quad SPI DQ3)
 5 ─┤ NC               NC   ├─ 12
 6 ─┤ NC               NC   ├─ 11
 7 ─┤ NC               NC   ├─ 10
 8 ─┤ DQ0 (MOSI)      CLK   ├─ 9
    └────────────────────────┘

Note: NC = No Connect. The additional DQ2/DQ3 pins are used only in
Quad SPI mode. Flashrom uses standard single-bit SPI, so DQ2 and DQ3
are not used during CH341A programming operations.
```

#### 2.4.3 DIP-8 Pinout (ZIF Socket)

DIP-8 (Dual Inline Package) chips are through-hole packages used in the CH341A's ZIF socket. The pinout is identical to SOIC-8 in function:

```text
         DIP-8 Package — Top View

         ●
    ┌────┴───────────┐
 1 ─┤ CS#        VCC ├─ 8
 2 ─┤ MISO      HOLD ├─ 7
 3 ─┤ WP         CLK ├─ 6
 4 ─┤ GND       MOSI ├─ 5
    └────────────────┘

Pin 1 = notch end, left side (top view, notch at top)
```

---

### 2.5 Identifying Pin 1

Correctly identifying pin 1 before attaching the clip is critical. Reversed clip orientation means commands go to the wrong pins, which produces no-detection results at best and chip damage at worst.

```text
Three ways manufacturers mark Pin 1:

METHOD 1: Dot on package body
┌──●─────────┐
│  CHIP TEXT  │    ← Pin 1 is on the side with the dot
└────────────┘

METHOD 2: Chamfer (beveled corner)
┌────────────┐
│  CHIP TEXT  │    ← Pin 1 is at the chamfered corner
└───/─────────

METHOD 3: Silkscreen indicator on PCB
  │
┌─┴──────────┐
│  CHIP TEXT  │    ← Pin 1 is nearest the silkscreen line
└────────────┘

Once pin 1 is identified on the chip:
Pin 1 is at the TOP-LEFT of the chip when the text is readable
(standard orientation: read the chip marking like a book — 
 pin 1 is top-left).

CORRECT clip orientation:
  Clip Pin 1 (red wire) aligns with Chip Pin 1 (dot)

INCORRECT — clip rotated 180°:
  Clip Pin 1 aligns with Chip Pin 8
  Result: VCC connected to CS#, GND connected to HOLD
  Result: No detection at best, chip damage possible
```

**Practical verification:** After attaching the clip, run `sudo flashrom -p ch341a_spi` and check if the chip is detected. If detection fails and you have already verified all other conditions (voltage, clip contact, target powered off), try rotating the clip 180° and retesting.

---

### 2.6 Jumper Settings

Most CH341A boards include one or more jumpers. Their functions vary by board revision. Common jumpers:

| Jumper Label | Position A | Position B | Notes |
|---|---|---|---|
| 3.3V / 5V | 3.3V | 5V | Only present on boards with voltage selection circuit. Verify with multimeter regardless. |
| 25xx / 24xx | SPI (25xx) | I²C (24xx) | Selects between SPI flash socket and I²C EEPROM socket. Set to 25xx for SPI NOR work. |
| S1 | Varies | Varies | Some boards have an unlabeled jumper that enables/disables onboard VCC supply to target |

> **⚠️ CAUTION:** Jumper labeling is inconsistent across manufacturers. Always verify voltage at the header pins with a multimeter regardless of jumper position. A jumper set to "3.3V" does not guarantee the board is actually supplying 3.3V if the regulator is absent or defective.

---

### 2.7 Clip Types

#### 2.7.1 SOIC-8 Test Clip (Pomona 5250 Style)

The most common clip for CH341A work. Clamps over the SOIC-8 package and makes contact with all 8 leads simultaneously. Available in two styles:

- **Clamshell (Pomona 5250 and equivalents):** High quality, reliable contact. Comes with a ribbon cable that plugs into the CH341A header.
- **Chinese equivalents:** Widely available, much cheaper, acceptable quality for most work. Inspect for bent or missing spring pins before use.

#### 2.7.2 SOIC-16 Clip

Identical concept to SOIC-8 clip but wider. Necessary for chips in SOIC-16 packages. Less commonly included with CH341A kits; purchase separately.

#### 2.7.3 Individual Pogo Pins

For chips in packages that cannot be clipped (QFN, WSON, BGA), individual spring-loaded pogo pins can be positioned and held by hand or with a jig. This approach is significantly more error-prone than using a dedicated clip and should be used only when no clip is available for the target package.

#### 2.7.4 Verifying Clip Contact Quality

Before every session, verify that the clip is making reliable contact:

```text
Procedure:
1. Attach clip to a known-good chip (test chip, not the target)
2. Set multimeter to continuity mode
3. Touch probes to corresponding pins at both ends of each clip wire
4. All 8 pins should show continuity (beep or low resistance)
5. Wiggle the cable gently — if continuity breaks, the cable has an
   intermittent connection and must be replaced
```

---

### 2.8 ESD Safety

Electrostatic discharge (ESD) can destroy flash chips, and ESD damage is particularly insidious because it may not cause immediate failure — the chip may function normally for days before failing permanently.

**ESD prevention measures:**
- Wear an anti-static wrist strap connected to a ground point (chassis ground of a plugged-in PC, or a grounding mat)
- Work on an ESD-safe mat or at minimum keep the chip touching a grounded surface
- Handle chips by their body, not by the leads
- Before picking up a chip, touch a grounded metal surface to discharge static buildup
- Store chips in anti-static bags or foam when not in use
- In dry weather (low humidity), ESD risk is significantly higher

---

### 2.9 Chapter Summary

The CH341A comes in black, green, and blue variants that are functionally identical from Flashrom's perspective but differ in their voltage output circuitry. The critical issue affecting all variants is the default 5V output, which must be corrected to 3.3V via hardware modification before use with modern SPI NOR flash chips. The SOIC-8 pinout (CS#, MISO, WP#, GND, MOSI, CLK, HOLD#, VCC from pin 1 to pin 8) is the standard for the vast majority of SPI NOR flash targets. Pin 1 is identified by a dot or chamfer on the package body. Always verify output voltage with a multimeter before connecting to any chip. Always verify clip continuity before every session.

---

## Chapter 3: Ubuntu Installation and System Setup

### Purpose

A complete CH341A workflow on Ubuntu 24.04 LTS requires several software packages beyond Flashrom itself. This chapter documents the installation of every required tool, the USB permission configuration that allows non-root access, kernel module considerations, and a complete verification procedure to confirm the system is ready for hardware work.

### 3.1 System Prerequisites

Begin with a fully updated Ubuntu 24.04 LTS system. Running against an outdated package database is a common source of dependency issues during installation.

```bash
# Update the package index and upgrade all installed packages
sudo apt update && sudo apt upgrade -y
```

**Why upgrade first:** Ubuntu's package manager resolves dependencies against the current package index. An outdated index can cause dependency conflicts when installing packages like `flashrom` that depend on libraries also used by other system components. Upgrading ensures you start from a consistent baseline.

After the upgrade completes, a reboot may be required if the kernel was updated:

```bash
# Reboot if a new kernel was installed (check if reboot is needed)
[ -f /var/run/reboot-required ] && echo "REBOOT REQUIRED" || echo "No reboot needed"
```

---

### 3.2 Installing Flashrom

#### 3.2.1 From Ubuntu Repositories

```bash
sudo apt install flashrom -y
```

#### 3.2.2 Checking the Version

```bash
flashrom --version
```

**Expected output:**
```
flashrom v1.2 on Linux 6.8.0-38-generic (x86_64)
flashrom is free software, get the source code at https://flashrom.org
```

> **[!IMPORTANT]**
> Ubuntu 24.04 LTS ships **Flashrom 1.2** in its official repositories. This handbook documents Flashrom 1.6+ syntax. The vast majority of commands in this handbook work identically in 1.2 and 1.6. Where differences exist, they are explicitly noted. If you require Flashrom 1.6 specifically, compile from source as documented below.

#### 3.2.3 Compiling Flashrom 1.6 from Source

If your workflow requires features or chip support added in Flashrom 1.3–1.6, compile from source:

```bash
# Install all build dependencies
sudo apt install -y \
    build-essential \
    git \
    cmake \
    pkg-config \
    libpci-dev \
    libusb-1.0-0-dev \
    libftdi1-dev \
    meson \
    ninja-build \
    python3

# Clone the official Flashrom repository
git clone https://review.coreboot.org/flashrom.git
cd flashrom

# Check out a specific release tag if desired
# git tag -l | grep -E '^v[0-9]'  # list release tags
# git checkout v1.4.0             # example: check out 1.4.0

# Set up the Meson build system
meson setup build

# Compile
ninja -C build

# Install system-wide
sudo ninja -C build install

# Verify the installed version
flashrom --version
```

**Dependency explanations:**
- `build-essential` — GCC compiler, make, and standard build tools
- `libpci-dev` — PCI bus access library (needed for internal programmer support)
- `libusb-1.0-0-dev` — USB access library used by the `ch341a_spi` backend
- `libftdi1-dev` — FTDI chip library (for FT2232H and similar programmers)
- `meson` + `ninja-build` — Flashrom's build system (replaced autotools in v1.2+)

---

### 3.3 Installing Supporting Tools

#### 3.3.1 usbutils

```bash
sudo apt install usbutils -y
lsusb --version
```

`usbutils` provides `lsusb`, which lists all USB devices connected to the system. You will use this to verify that the CH341A is detected by the operating system.

#### 3.3.2 binutils

```bash
sudo apt install binutils -y
```

`binutils` provides several tools used in firmware analysis: `objdump` (disassembly), `readelf` (ELF binary inspection), `strings` (extract printable strings), `ar` (archive tool), and `nm` (symbol listing). These are used in Chapter 9.

#### 3.3.3 git

```bash
sudo apt install git -y
```

Required for cloning Flashrom source and other security tools from source repositories.

#### 3.3.4 binwalk v2.x (Primary)

```bash
sudo apt install binwalk -y
binwalk --help | head -5
```

**Expected output:**
```
Binwalk v2.3.4
Craig Heffner, ReFirmLabs

Usage: binwalk [OPTIONS] [FILE1] [FILE2] [FILE3] ...
```

Ubuntu 24.04 ships binwalk 2.3.x from its repositories. This is the version documented throughout this handbook. Binwalk v2.x requires several optional dependencies for full functionality:

```bash
# Optional but recommended: dependencies for binwalk extraction
sudo apt install -y \
    python3-lzma \
    python3-crypto \
    squashfs-tools \
    mtd-utils \
    unrar \
    p7zip-full \
    cabextract
```

#### 3.3.5 binwalk v3 (Optional, pip)

Binwalk v3 is a complete rewrite by ReFirmLabs, installable via pip. It is NOT the same as v2 — the command-line syntax differs in several respects. To use v3 without conflicting with the apt-installed v2:

```bash
# Create an isolated virtual environment for binwalk v3
python3 -m venv ~/binwalk3-env

# Activate the environment
source ~/binwalk3-env/bin/activate

# Install binwalk v3
pip install binwalk

# Verify
binwalk --version

# Deactivate when done
deactivate
```

> **[!NOTE]**
> This handbook uses binwalk v2.x syntax throughout. If you are using v3, check `binwalk --help` to verify flag compatibility with your installed version.

#### 3.3.6 i2c-tools

```bash
sudo apt install i2c-tools -y
```

`i2c-tools` provides the following commands for I²C EEPROM work (Chapter 6):
- `i2cdetect` — scan an I²C bus for connected devices
- `i2cdump` — dump the contents of an I²C device
- `i2cget` — read individual bytes from an I²C device
- `i2cset` — write individual bytes to an I²C device
- `i2ctransfer` — perform complex multi-message I²C transfers

#### 3.3.7 eeprog

```bash
# Check if eeprog is available in Ubuntu 24.04 repos
apt-cache show eeprog 2>/dev/null && echo "Available" || echo "Not in repos"

# If available:
sudo apt install eeprog -y
```

> **[!NOTE]**
> `eeprog` may not be present in Ubuntu 24.04 repositories. If `apt-cache show eeprog` returns no results, you must compile from source. The eeprog project is hosted at various locations; search for "eeprog 24Cxx linux" to find a maintained fork. Verify the source before building.

#### 3.3.8 PulseView and sigrok

```bash
sudo apt install pulseview sigrok sigrok-cli -y

# Verify
pulseview --version
sigrok-cli --version
```

`pulseview` is the graphical waveform viewer from the sigrok project. `sigrok-cli` provides command-line capture capabilities. Both are used in Chapter 12 for logic analyzer integration.

#### 3.3.9 Additional Useful Tools

```bash
sudo apt install -y \
    hexedit \
    file \
    python3-pip \
    python3-serial \
    libssl-dev \
    openssl \
    xxd
```

- `hexedit` — interactive hex editor for binary file modification
- `file` — determines file type from magic bytes
- `python3-pip` — Python package manager for additional tools
- `openssl` — SSL/TLS toolkit for inspecting certificates extracted from firmware
- `xxd` — hex dump utility (already included with vim package, but install explicitly)

---

### 3.4 USB Device Recognition

#### 3.4.1 Connecting the CH341A

1. Ensure no chip is connected to the clip or ZIF socket
2. Plug the CH341A into a USB port on your host PC
3. Wait 2–3 seconds for USB enumeration
4. Verify detection:

```bash
lsusb | grep -i "1a86"
```

**Expected output:**
```
Bus 001 Device 003: ID 1a86:5512 QinHeng Electronics CH341 in programmer mode
```

If you see `1a86:5512` — the device is in programmer mode and ready for Flashrom. If you see `1a86:7523` — the device is in serial mode and the `ch341` kernel serial driver has claimed it.

#### 3.4.2 Kernel Module Behavior

Flashrom accesses the CH341A in programmer mode (`1a86:5512`) directly via libusb. It does **not** use a kernel driver. However, if the `ch341` kernel module is loaded, it may attempt to claim the device, interfering with libusb access.

```bash
# Check if the ch341 serial module is loaded
lsmod | grep ch341
```

If `ch341` appears in the output:

```bash
# Unload the ch341 serial driver
sudo modprobe -r ch341

# Retry flashrom
sudo flashrom -p ch341a_spi
```

To prevent the `ch341` module from loading automatically on boot (recommended if you only use the CH341A as a programmer, not as a serial adapter):

```bash
# Create a blacklist entry
echo 'blacklist ch341' | sudo tee /etc/modprobe.d/blacklist-ch341.conf

# Regenerate initramfs to apply at next boot
sudo update-initramfs -u
```

---

### 3.5 Permissions and udev Rules

#### 3.5.1 Why Permission Configuration Is Needed

USB devices on Linux are owned by `root` by default. Without configuration, accessing the CH341A requires `sudo` for every Flashrom command. For a cleaner workflow — particularly in team lab environments — configure udev rules to grant access to members of the `plugdev` group.

#### 3.5.2 Creating the udev Rule

```bash
# Create the udev rule file
sudo tee /etc/udev/rules.d/60-ch341a.rules << 'EOF'
# CH341A USB Programmer — Programmer Mode (VID:PID 1a86:5512)
SUBSYSTEM=="usb", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="5512", MODE="0664", GROUP="plugdev"
EOF
```

**Field explanations:**
- `SUBSYSTEM=="usb"` — match only USB devices
- `ATTRS{idVendor}=="1a86"` — match QinHeng Electronics (CH341A manufacturer)
- `ATTRS{idProduct}=="5512"` — match programmer mode specifically
- `MODE="0664"` — set file permissions: owner read/write, group read/write, others read
- `GROUP="plugdev"` — assign device to the `plugdev` group

```bash
# Add your user to the plugdev group
sudo usermod -aG plugdev $USER

# Reload udev rules without rebooting
sudo udevadm control --reload-rules
sudo udevadm trigger

# Verify group membership
groups $USER
```

> **[!IMPORTANT]**
> Group membership changes do not take effect in the current session. You must log out and log back in (or start a new login shell) for the group change to apply.

#### 3.5.3 Testing Non-Root Access

After logging back in:

```bash
# Test without sudo
flashrom -p ch341a_spi --flash-name
```

If this succeeds without `sudo`, udev permissions are working correctly.

---

### 3.6 Verifying the Complete Installation

Run this verification script to confirm all components are correctly installed:

```bash
#!/bin/bash
# CH341A Installation Verification Script
# Save as verify_ch341a_setup.sh and run with: bash verify_ch341a_setup.sh

echo "=================================================="
echo "  CH341A Ubuntu Setup Verification"
echo "=================================================="
echo ""

# flashrom
echo "--- flashrom ---"
if command -v flashrom &>/dev/null; then
    flashrom --version 2>&1 | head -1
    echo "Status: INSTALLED ✅"
else
    echo "Status: NOT FOUND ❌"
fi
echo ""

# CH341A USB device
echo "--- CH341A USB Device ---"
if lsusb | grep -q "1a86:5512"; then
    lsusb | grep "1a86:5512"
    echo "Status: DETECTED ✅"
else
    echo "CH341A not detected in lsusb"
    echo "Status: NOT CONNECTED or NOT DETECTED ⚠️"
fi
echo ""

# binwalk
echo "--- binwalk ---"
if command -v binwalk &>/dev/null; then
    binwalk --version 2>&1 | head -1
    echo "Status: INSTALLED ✅"
else
    echo "Status: NOT FOUND ❌"
fi
echo ""

# i2c-tools
echo "--- i2c-tools ---"
if command -v i2cdetect &>/dev/null; then
    echo "i2cdetect: $(i2cdetect --version 2>&1)"
    echo "Status: INSTALLED ✅"
else
    echo "Status: NOT FOUND ❌"
fi
echo ""

# PulseView
echo "--- PulseView ---"
if command -v pulseview &>/dev/null; then
    echo "Status: INSTALLED ✅"
else
    echo "Status: NOT FOUND ❌ (install: sudo apt install pulseview)"
fi
echo ""

# Kernel module check
echo "--- Kernel Module Status ---"
if lsmod | grep -q "^ch341"; then
    echo "ch341 serial module is LOADED ⚠️"
    echo "This may conflict with Flashrom. Run: sudo modprobe -r ch341"
else
    echo "ch341 serial module not loaded ✅"
fi
echo ""

echo "=================================================="
echo "  Verification Complete"
echo "=================================================="
```

---

### 3.7 Common Installation Errors

| Error | Cause | Solution |
|---|---|---|
| `flashrom: command not found` | Package not installed | `sudo apt install flashrom -y` |
| `Permission denied` accessing USB | No udev rule or not in plugdev group | Set up udev rule per Section 3.5; log out/in |
| CH341A shows as `1a86:7523` in lsusb | Device in serial mode, `ch341` module claimed it | `sudo modprobe -r ch341`, replug device |
| `meson: command not found` during source build | meson not installed | `sudo apt install meson ninja-build` |
| `libusb-1.0-0-dev: package not found` | Incorrect package name | Use `libusb-1.0-0-dev` (with hyphens exactly) |
| `binwalk: No module named 'magic'` | Missing python3-magic | `sudo apt install python3-magic` |

---

### 3.8 Chapter Summary

A complete CH341A installation on Ubuntu 24.04 LTS requires: flashrom (from apt or source), usbutils, binutils, binwalk, i2c-tools, pulseview, and sigrok. The CH341A appears as `1A86:5512` in programmer mode. The `ch341` kernel module must not be loaded when using Flashrom. Configure udev rules to allow non-root access, and verify group membership takes effect by logging out and back in. Run the verification script to confirm everything is working before connecting to any hardware.

---

## Chapter 4: Flashrom Complete Reference

### Purpose

Flashrom is the central tool for all SPI NOR flash operations with the CH341A. This chapter documents every flag, every command combination, every expected output, and every error condition relevant to CH341A use. Read this chapter in full, then use it as a reference during operations.

### Theory

Flashrom is an open-source flash chip programming tool that supports a large number of programmer hardware backends and an extensive database of flash chip definitions. Its internal architecture consists of three layers:

1. **Programmer backend:** The code that communicates with the physical programmer hardware. For CH341A, this is the `ch341a_spi` backend, which uses libusb to communicate directly with the CH341A USB device without requiring a kernel driver.

2. **Chip database:** A compiled-in database of JEDEC IDs and chip parameters for thousands of flash chips. When Flashrom detects a chip's JEDEC ID, it looks up the matching entry in this database to determine the chip's size, sector layout, and command set.

3. **Operations pipeline:** The read-erase-write-verify pipeline that Flashrom uses for all programming operations. Flashrom never writes to a sector it has not first determined needs to be changed (unless forced), and always verifies writes by reading back.

---

### 4.1 The Programmer String

Every Flashrom command begins with specifying the programmer using the `-p` flag:

```bash
sudo flashrom -p ch341a_spi
```

**Flag explanation:**
- `-p` (or `--programmer`): specifies which programmer hardware to use
- `ch341a_spi`: the programmer identifier for the CH341A in SPI mode

When Flashrom receives this argument, it:
1. Searches for a USB device matching VID `0x1A86` PID `0x5512` via libusb
2. Initializes the CH341A into SPI master mode
3. Sets up the SPI clock and signal parameters

> **[!NOTE]**
> As of Flashrom 1.6, the `ch341a_spi` programmer does **not** support user-configurable SPI speed (`spispeed=` parameter). Attempting to add `spispeed=` to the programmer string will produce an error. Do not attempt: `flashrom -p ch341a_spi:spispeed=1000`. This is a known limitation of the CH341A backend.

---

### 4.2 Chip Detection

#### 4.2.1 Auto-Detection

Running Flashrom with only the programmer string performs chip detection:

```bash
sudo flashrom -p ch341a_spi
```

**Expected output (chip detected):**
```
flashrom v1.2 on Linux 6.8.0-38-generic (x86_64)
flashrom is free software, get the source code at https://flashrom.org

Using clock_gettime for delay loops (clk_id: 1, resolution: 1ns).
Found Winbond flash chip "W25Q128.V" (16384 kB, SPI) on ch341a_spi.
No operations were specified.
```

**Output line explanations:**
- Line 1: Flashrom version, OS, and architecture
- Line 2: License notice
- Line 4: Timing mechanism used for internal delays
- Line 5: Detected chip information — manufacturer, part number, size in kB, interface, programmer
- Line 6: No read/write/erase was requested, so no operations were performed

**Expected output (no chip detected):**
```
flashrom v1.2 on Linux 6.8.0-38-generic (x86_64)
flashrom is free software, get the source code at https://flashrom.org

Using clock_gettime for delay loops (clk_id: 1, resolution: 1ns).
No EEPROM/flash device found.
```

#### 4.2.2 Specifying a Chip Explicitly

When multiple chips in Flashrom's database match the detected JEDEC ID, Flashrom will list the candidates and ask you to specify:

```
Multiple flash chip definitions match the detected chip(s): "W25Q128.V" "W25Q128JV"
Please specify which chip definition to use with the -c <chipname> option.
```

Use the `-c` flag to specify:

```bash
sudo flashrom -p ch341a_spi -c "W25Q128.V"
```

**When to use `-c`:** When Flashrom reports multiple matches, or when you know the exact chip part number from the physical markings and want to ensure Flashrom uses the correct timing parameters.

#### 4.2.3 Listing All Supported Chips

```bash
sudo flashrom -p ch341a_spi -L
```

This command prints every chip in Flashrom's database, regardless of what is physically connected. Use it to verify that your target chip is supported before the physical connection.

```bash
# Filter for a specific manufacturer
sudo flashrom -p ch341a_spi -L | grep -i "winbond"
sudo flashrom -p ch341a_spi -L | grep -i "macronix"
sudo flashrom -p ch341a_spi -L | grep -i "gigadevice"

# Search for a specific part number
sudo flashrom -p ch341a_spi -L | grep -i "W25Q128"
```

**Example output snippet:**
```
Vendor           Device           Test
Winbond          W25Q128.V        OK
Winbond          W25Q128JV        OK
Winbond          W25Q128FV        OK
```

#### 4.2.4 Reading Chip Name After Detection

```bash
sudo flashrom -p ch341a_spi --flash-name
```

**Expected output:**
```
vendor="Winbond" name="W25Q128.V"
```

This is useful in scripts to capture the detected chip information.

#### 4.2.5 Reading Chip Size

```bash
sudo flashrom -p ch341a_spi --flash-size
```

**Expected output:**
```
16777216
```

This outputs the chip size in bytes. `16777216` bytes = 16,777,216 bytes = 16 MB = 128 Mbit (W25Q128).

To convert in a script:
```bash
SIZE_BYTES=$(sudo flashrom -p ch341a_spi --flash-size 2>/dev/null)
SIZE_MB=$((SIZE_BYTES / 1024 / 1024))
echo "Chip size: ${SIZE_BYTES} bytes = ${SIZE_MB} MB"
```

---

### 4.3 Verbose Mode

Flashrom supports three levels of verbosity beyond default output. These are essential for troubleshooting detection failures.

```bash
# Level 1: Verbose — additional operational details
sudo flashrom -p ch341a_spi -V

# Level 2: More verbose — internal state information
sudo flashrom -p ch341a_spi -VV

# Level 3: Maximum verbose — raw SPI transaction data including JEDEC ID bytes
sudo flashrom -p ch341a_spi -VVV
```

**When to use each level:**

| Level | Flag | Use When |
|---|---|---|
| Default | (none) | Normal operations |
| Verbose | `-V` | Want to see erase/write sector progress |
| More verbose | `-VV` | Debugging programmer initialization issues |
| Maximum | `-VVV` | Debugging chip detection; need to see raw JEDEC ID bytes |

**Example -VVV output showing JEDEC ID detection:**
```
...
Probing for Winbond W25Q128.V, 16384 kB: probe_spi_rdid_generic: id1 0xef, id2 0x4018
...
```

In this example: `id1=0xEF` is the Winbond manufacturer ID, `id2` combines memory type (`0x40`) and capacity (`0x18`). This is the raw JEDEC ID data used for chip identification.

```bash
# Practical: capture verbose output to a log file for analysis
sudo flashrom -p ch341a_spi -VVV 2>&1 | tee flashrom_verbose.log

# Extract JEDEC ID from log
grep -i "rdid\|jedec\|id1\|id2" flashrom_verbose.log
```

---

### 4.4 Reading Firmware

Reading is the most common CH341A operation and the foundation of all firmware security work.

#### 4.4.1 Basic Read

```bash
sudo flashrom -p ch341a_spi -r backup.bin
```

**Flag explanations:**
- `-p ch341a_spi` — specifies the CH341A SPI programmer
- `-r backup.bin` — read the entire chip contents and save to `backup.bin`

**Expected output:**
```
flashrom v1.2 on Linux 6.8.0-38-generic (x86_64)
flashrom is free software, get the source code at https://flashrom.org

Using clock_gettime for delay loops (clk_id: 1, resolution: 1ns).
Found Winbond flash chip "W25Q128.V" (16384 kB, SPI) on ch341a_spi.
Reading flash... done.
```

The read of a 16 MB chip at ~1 MHz SPI speed takes approximately 2–3 minutes.

#### 4.4.2 Read with Chip Specification

```bash
sudo flashrom -p ch341a_spi -c "W25Q128.V" -r backup.bin
```

Use `-c` when Flashrom reports multiple chip matches. The chip name must exactly match one of the names printed by `flashrom -L`.

#### 4.4.3 The Double-Read Protocol

A single read is never sufficient for security work. SPI clip connections are inherently less reliable than soldered connections, and a single read may contain subtle bit errors that are impossible to detect without a comparison read.

```bash
# First read
sudo flashrom -p ch341a_spi -r firmware_read1.bin
echo "Read 1 complete."
sha256sum firmware_read1.bin

# Second read (do not disturb the clip between reads)
sudo flashrom -p ch341a_spi -r firmware_read2.bin
echo "Read 2 complete."
sha256sum firmware_read2.bin

# Compare
echo ""
echo "Comparing reads..."
if cmp -s firmware_read1.bin firmware_read2.bin; then
    echo "✅ READS MATCH — connection is reliable"
    echo "Verified SHA256:"
    sha256sum firmware_read1.bin
else
    echo "❌ READS DIFFER — check clip connection before proceeding"
    # Find the first differing byte
    cmp firmware_read1.bin firmware_read2.bin | head -5
fi
```

**If reads differ:** Re-seat the clip. Verify the target board is powered off. Check for bus contention. Run three reads and use the majority if two of three match. If reads continue to differ after fixing connections, remove the chip from the PCB and program out-of-circuit.

---

### 4.5 Writing Firmware

> **⚠️ CAUTION:** Writing firmware to a chip that does not have a verified backup is one of the most dangerous operations you can perform. If the write fails or the new firmware is corrupt, you may permanently lose the ability to recover the device without a backup.

#### 4.5.1 Basic Write

```bash
sudo flashrom -p ch341a_spi -w new_firmware.bin
```

**What Flashrom does during a write:**
1. Reads the entire current chip contents
2. Compares current contents with `new_firmware.bin` byte by byte
3. Identifies sectors that contain differences
4. Erases only those sectors (NOR flash requires erase before write)
5. Writes the new data to the erased sectors
6. Reads back and verifies every written byte

**Expected output:**
```
flashrom v1.2 on Linux 6.8.0-38-generic (x86_64)
flashrom is free software, get the source code at https://flashrom.org

Using clock_gettime for delay loops (clk_id: 1, resolution: 1ns).
Found Winbond flash chip "W25Q128.V" (16384 kB, SPI) on ch341a_spi.
Reading old flash chip contents... done.
Erasing and writing flash chip... Erase/write done.
Verifying flash... VERIFIED.
```

The "VERIFIED" message confirms that Flashrom read back all written sectors and they match the new firmware file exactly.

#### 4.5.2 Skipping Verification After Write

```bash
# WARNING: Not recommended. Only use if you understand the risk.
sudo flashrom -p ch341a_spi -w new_firmware.bin --noverify
```

`--noverify` skips the post-write verification step. This saves time but eliminates confirmation that the write succeeded. Only appropriate when a chip has known verification quirks that cause false failures, or in time-critical recovery scenarios where you intend to verify manually afterward.

#### 4.5.3 Skipping Verification of Unwritten Regions

```bash
sudo flashrom -p ch341a_spi -w new_firmware.bin --noverify-all
```

`--noverify-all` skips verification for regions that were not changed (Flashrom normally verifies all regions, not just the ones it wrote). This is slightly safer than `--noverify` because changed regions are still verified.

---

### 4.6 Verifying Firmware

Standalone verification reads the chip and compares it byte-for-byte against a reference file, without writing anything.

```bash
sudo flashrom -p ch341a_spi -v firmware.bin
```

**Expected output on success:**
```
Reading flash... done.
Verifying flash... VERIFIED.
```

**Expected output on mismatch:**
```
Reading flash... done.
Verifying flash... FAILED at 0x00000000! Expected=0xAB, Got=0xFF
Your flash chip is in an unknown state.
```

Use this command:
- After every write operation, as an independent confirmation
- To confirm that a chip's contents match a known-good backup before disassembling the device
- To verify that a "read" you performed matches what is on the chip

---

### 4.7 Erasing

```bash
sudo flashrom -p ch341a_spi -E
```

> **⚠️ CAUTION:** Full chip erase destroys ALL data on the chip. Every byte is set to `0xFF`. Ensure you have a verified backup before erasing. There is no undo.

**Expected output:**
```
Found Winbond flash chip "W25Q128.V" (16384 kB, SPI) on ch341a_spi.
Erasing and writing flash chip... Erase/write done.
```

Note: Flashrom reports "Erase/write done" even for a standalone erase operation — this is normal. The chip is erased (all sectors set to 0xFF).

**Typical erase time:** 30–120 seconds for a 16 MB chip, depending on the chip's erase speed.

After erasing, you can verify the chip is erased:
```bash
# Read the erased chip
sudo flashrom -p ch341a_spi -r erased_verify.bin

# Confirm all bytes are 0xFF
python3 -c "
data = open('erased_verify.bin', 'rb').read()
ff_count = data.count(b'\xff')
total = len(data)
print(f'Total bytes: {total}')
print(f'0xFF bytes: {ff_count}')
print('Chip fully erased: ' + ('YES ✅' if ff_count == total else 'NO ❌'))
"
```

---

### 4.8 Layout Files and Partial Operations

Flash chips used in real devices are almost always divided into functional regions: bootloader, kernel, root filesystem, NVRAM/configuration. Layout files allow Flashrom to operate on specific regions rather than the entire chip.

#### 4.8.1 Layout File Format

A layout file is a plain text file with one region per line:

```
# Format: start_address:end_address region_name
# All addresses are in hexadecimal
# Addresses are byte offsets from the start of the flash chip

00000000:0003ffff bootloader
00040000:007effff firmware
007f0000:007fffff nvram
```

**Format rules:**
- Start and end addresses are inclusive, in hex
- The region name must be a single word (no spaces)
- Lines beginning with `#` are comments
- Addresses refer to byte offsets from the start of the flash chip (address 0x00000000)

#### 4.8.2 Reading a Specific Region

```bash
sudo flashrom -p ch341a_spi -l router.layout -i bootloader -r bootloader_backup.bin
```

**Flag explanations:**
- `-l router.layout` — use the specified layout file
- `-i bootloader` — operate only on the region named "bootloader"
- `-r bootloader_backup.bin` — read that region to file

The output file will contain only the bytes from the specified region (in this example, 0x40000 = 262,144 bytes).

#### 4.8.3 Writing a Specific Region

```bash
sudo flashrom -p ch341a_spi -l router.layout -i bootloader -w new_bootloader.bin
```

> **🔴 DANGER:** Writing the wrong data to the bootloader region will brick the device. The bootloader cannot recover itself if it is corrupt. Triple-check layout file offsets against the device's documented memory map before writing to the bootloader region.

#### 4.8.4 Obtaining Layout Information

Layout information typically comes from:
- The device manufacturer's SDK or GPL source code release
- Reverse engineering the bootloader (U-Boot environment variables often contain MTD partition tables)
- OpenWrt's wiki (for supported routers)
- The Linux kernel's MTD partition definitions for the specific device

Example: reading U-Boot environment to find partition layout:
```bash
# After extracting firmware, search strings for MTD partition definitions
strings firmware_read1.bin | grep -i "mtdparts\|partition\|0x0000"
```

---

### 4.9 JEDEC ID Reading

The JEDEC ID is the 3-byte identifier that every SPI NOR flash chip responds with when sent the `RDID` (Read Identification, opcode `0x9F`) command. Flashrom sends this command automatically during chip detection.

#### 4.9.1 Reading JEDEC ID with Maximum Verbosity

```bash
sudo flashrom -p ch341a_spi -VVV 2>&1 | grep -A3 -i "rdid\|id1\|id2"
```

**Example output:**
```
Probing for Winbond W25Q128.V, 16384 kB: probe_spi_rdid_generic: id1 0xef, id2 0x4018
```

#### 4.9.2 JEDEC ID Structure

The JEDEC ID consists of 3 bytes:

```
Byte 0: Manufacturer ID
Byte 1: Memory Type
Byte 2: Capacity (log2 encoding in most cases)

Example: EF 40 18
  EF = Winbond (manufacturer)
  40 = SPI NOR, standard (memory type)
  18 = 0x18 = 24 decimal → 2^24 = 16,777,216 bytes = 16 MB (capacity)
```

#### 4.9.3 JEDEC Manufacturer ID Reference Table

| Manufacturer ID (hex) | Manufacturer | Common Families |
|---|---|---|
| 0xEF | Winbond | W25Qxxx, W25Xxx |
| 0xC2 | Macronix | MX25Lxxx, MX25Rxxx |
| 0xC8 | GigaDevice | GD25Qxxx |
| 0x9D | ISSI | IS25LPxxx, IS25WPxxx |
| 0x20 | Micron/ST | N25Qxxx, M25Pxxx, MT25Qxxx |
| 0x1C | EON | EN25Qxxx, EN25Fxxx |
| 0x0B | XMC | XM25QHxxx |
| 0xBF | Microchip/SST | SST25VFxxx |
| 0x01 | Spansion/Infineon | S25FLxxx |
| 0xAD | Hyundai/Hynix | HY25Pxxx |

#### 4.9.4 Capacity Byte Decoding Table

| Capacity Byte (hex) | Capacity (bytes) | Size (MB) | Size (Mbit) |
|---|---|---|---|
| 0x10 | 65,536 | 0.0625 | 0.5 |
| 0x11 | 131,072 | 0.125 | 1 |
| 0x12 | 262,144 | 0.25 | 2 |
| 0x13 | 524,288 | 0.5 | 4 |
| 0x14 | 1,048,576 | 1 | 8 |
| 0x15 | 2,097,152 | 2 | 16 |
| 0x16 | 4,194,304 | 4 | 32 |
| 0x17 | 8,388,608 | 8 | 64 |
| 0x18 | 16,777,216 | 16 | 128 |
| 0x19 | 33,554,432 | 32 | 256 |
| 0x20 | 67,108,864 | 64 | 512 |

> **[!NOTE]**
> The capacity byte encoding (`2^N` bytes where N is the decimal value of the byte) is a convention followed by most manufacturers. However, some manufacturers use proprietary encoding for the capacity byte. If the decoded size does not match the physical chip markings, consult the chip datasheet directly.

---

### 4.10 Status Register Operations

SPI NOR flash chips have one or more 8-bit status registers that control write protection, busy state, and other operational flags.

> **[!IMPORTANT]**
> Flashrom does **not** expose direct status register read/write commands via CLI flags in v1.6. Status register operations happen internally during Flashrom's probe, erase, and write sequences. Flashrom reads the status register to check the Write In Progress (WIP) bit after each operation, and it handles write enable/disable automatically.
>
> If you need to directly read or manipulate status registers for chip-specific write protection scenarios, this is beyond Flashrom's CLI interface. In such cases, manufacturer-specific tools or Python scripts using pyftdi or pylibftdi are required. This handbook does not document those approaches as they are chip-specific and outside CH341A/Flashrom scope.

**What Flashrom does handle automatically:**
- Write Enable (WREN, opcode 0x06) — sent before every write operation
- Write Disable (WRDI, opcode 0x04) — sent after write completion
- Polling Write In Progress (RDSR bit 0) — waits for erase/write to complete
- For supported chips: clearing software write-protect bits before writing

---

### 4.11 Complete Flashrom Flag Reference Table

| Short Flag | Long Form | Purpose | Example |
|---|---|---|---|
| `-p` | `--programmer` | Specify programmer | `-p ch341a_spi` |
| `-r` | `--read` | Read chip to file | `-r backup.bin` |
| `-w` | `--write` | Write file to chip | `-w firmware.bin` |
| `-v` | `--verify` | Verify chip against file | `-v firmware.bin` |
| `-E` | `--erase` | Erase entire chip | `-E` |
| `-c` | `--chip` | Specify chip by name | `-c "W25Q128.V"` |
| `-l` | `--layout` | Use layout file | `-l flash.layout` |
| `-i` | `--image` | Select region from layout | `-i bootloader` |
| `-L` | `--list-supported` | List all supported chips | `-L` |
| `-V` | (no long form) | Increase verbosity (stackable) | `-VVV` |
| `-n` / `--noverify` | `--noverify` | Skip verify after write | `--noverify` |
| | `--noverify-all` | Skip verify of all regions | `--noverify-all` |
| | `--flash-name` | Print detected chip name | `--flash-name` |
| | `--flash-size` | Print chip size in bytes | `--flash-size` |
| | `--ifd` | Use Intel Flash Descriptor layout | `--ifd` |
| | `--fmap` | Use FMAP layout (coreboot/Chromebook) | `--fmap` |

> **[!NOTE]**
> `--ifd` and `--fmap` are primarily used with the `internal` programmer (reading BIOS chip from a running system via `/dev/mem`). Their behavior when used with `ch341a_spi` may be limited or differ from documented behavior. Test carefully on non-critical hardware.

---

### 4.12 Chapter Summary

Flashrom's `ch341a_spi` programmer backend communicates with the CH341A via libusb. The core operations — detect, read, write, verify, erase — are invoked with `-p`, `-r`, `-w`, `-v`, and `-E` respectively. Chip specification with `-c` resolves ambiguous detection. Layout files with `-l` and `-i` enable partial operations on specific firmware regions. The JEDEC ID is the 3-byte identifier decoded from Manufacturer ID + Memory Type + Capacity. Verbose mode (`-VVV`) exposes raw JEDEC ID bytes for troubleshooting. Status register operations are handled internally by Flashrom and are not exposed as CLI commands. Always read twice and compare SHA256 before performing any write operations.

---

## Chapter 5: SPI NOR Flash Deep Dive

### Purpose

Understanding SPI NOR flash at the protocol level is essential for diagnosing detection failures, interpreting logic analyzer captures, and making informed decisions during firmware operations. This chapter documents the SPI protocol, JEDEC ID structure, major manufacturers, physical packages, and voltage requirements.

### 5.1 SPI Protocol Theory

#### 5.1.1 SPI Bus Overview

SPI (Serial Peripheral Interface) is a synchronous serial communication protocol invented by Motorola in the 1980s. It uses four primary signals:

| Signal | Direction | Description |
|---|---|---|
| SCLK (CLK) | Master → Slave | Serial clock, generated by the master (CH341A) |
| CS# (SS#) | Master → Slave | Chip Select (active low) — selects the target device |
| MOSI | Master → Slave | Master Out Slave In — commands and write data |
| MISO | Master → Slave | Master In Slave Out — read data from chip |

SPI NOR flash adds two additional signals:

| Signal | Direction | Description |
|---|---|---|
| WP# | Master → Slave | Write Protect (active low) — hardware write protection |
| HOLD# | Master → Slave | Hold (active low) — pauses transfer without deselecting |

#### 5.1.2 SPI Communication Timing

```text
A READ command transaction:

CLK  ______|‾|_|‾|_|‾|_|‾|_|‾|_|‾|_|‾|_|‾|_|‾|_|‾|_|‾|_|‾|_|‾|_|‾|_|____
CS#  ‾‾‾‾‾|______________________________________________|‾‾‾‾‾‾‾‾‾‾‾‾
MOSI -----< 0x03 cmd >< addr[23:16] >< addr[15:8] >< addr[7:0] >----------
MISO -------< hi-Z  >< hi-Z        >< hi-Z        >< hi-Z     >< data >---

Sequence:
1. CS# pulled LOW by master (CH341A) to select chip
2. Master sends READ command (0x03) on MOSI
3. Master sends 3-byte address (24-bit) on MOSI
4. Chip begins responding with data bytes on MISO
5. Master continues clocking; chip streams data
6. CS# pulled HIGH to end transaction
```

#### 5.1.3 SPI Modes

SPI defines four operating modes based on Clock Polarity (CPOL) and Clock Phase (CPHA):

| Mode | CPOL | CPHA | Clock Idle | Data Sampled |
|---|---|---|---|---|
| Mode 0 | 0 | 0 | LOW | Rising edge |
| Mode 1 | 0 | 1 | LOW | Falling edge |
| Mode 2 | 1 | 0 | HIGH | Falling edge |
| Mode 3 | 1 | 1 | HIGH | Rising edge |

Most SPI NOR flash chips support **Mode 0** and **Mode 3**. Flashrom selects the appropriate mode automatically based on the chip definition in its database. You do not need to configure this manually.

#### 5.1.4 SPI NOR vs SPI NAND

| Feature | SPI NOR | SPI NAND |
|---|---|---|
| Access model | Byte-addressable (random read) | Page-based (sequential) |
| Read speed | Good | Good |
| Write speed | Slow (erase + write) | Moderate |
| Erase unit | 4 KB sector or 64 KB block | 128 KB block |
| Capacity range | 64 KB – 128+ MB | 64 MB – 16 GB |
| Bad blocks | None | Present (requires management) |
| ECC required | No | Yes |
| CH341A/Flashrom support | Excellent | Limited/unreliable |
| Typical use | BIOS, bootloader, small firmware | Large filesystem storage |

**The CH341A is suitable for SPI NOR only.** If your target device uses SPI NAND, you need a different programmer.

#### 5.1.5 Common SPI NOR Commands

| Command | Opcode | Description |
|---|---|---|
| RDID | 0x9F | Read JEDEC ID (3 bytes) |
| RDSR | 0x05 | Read Status Register 1 |
| WRSR | 0x01 | Write Status Register |
| WREN | 0x06 | Write Enable |
| WRDI | 0x04 | Write Disable |
| READ | 0x03 | Read data bytes |
| FAST_READ | 0x0B | Read with dummy byte (higher speed) |
| PP | 0x02 | Page Program (write 256 bytes) |
| SE | 0x20 | Sector Erase (4 KB) |
| BE32K | 0x52 | Block Erase (32 KB) |
| BE64K | 0xD8 | Block Erase (64 KB) |
| CE | 0xC7 or 0x60 | Chip Erase |
| DP | 0xB9 | Deep Power Down |
| RES | 0xAB | Release from Deep Power Down |

---

### 5.2 JEDEC ID Structure in Depth

#### 5.2.1 Decoding a JEDEC ID

```text
Example: Chip returns JEDEC ID bytes: C8 40 18

Byte 0: 0xC8 → Manufacturer = GigaDevice
Byte 1: 0x40 → Memory Type = SPI NOR, standard voltage
Byte 2: 0x18 → Capacity code = 0x18 = 24 decimal
                2^24 = 16,777,216 bytes = 16 MB = 128 Mbit
                → Part number: GD25Q128

Complete identification: GigaDevice GD25Q128 (16 MB SPI NOR)
```

#### 5.2.2 When JEDEC ID Cannot Identify the Chip

Some chips have identical JEDEC IDs to other chips (manufacturer reuse of IDs). In these cases, Flashrom's database may have multiple entries matching the same ID. Use the physical chip markings as the authoritative source and specify the chip with `-c`.

---

### 5.3 Major Manufacturers Reference

#### 5.3.1 Winbond (W25Q Series)

Winbond is the most common SPI NOR flash manufacturer in consumer electronics. Their W25Q series dominates router, camera, and BIOS applications.

| Part Number | Size | JEDEC ID | Common Devices |
|---|---|---|---|
| W25Q16JV | 2 MB | EF 40 15 | Small routers, MCUs |
| W25Q32FV | 4 MB | EF 40 16 | Budget routers |
| W25Q64FV | 8 MB | EF 40 17 | Mid-range routers, cameras |
| W25Q128FV | 16 MB | EF 40 18 | BIOS, high-end routers |
| W25Q128JV | 16 MB | EF 70 18 | Newer BIOS chips |
| W25Q256FV | 32 MB | EF 40 19 | Modern BIOS, complex systems |
| W25Q256JV | 32 MB | EF 70 19 | Modern BIOS |

#### 5.3.2 Macronix (MX25L Series)

Common in laptops, industrial equipment, and network hardware.

| Part Number | Size | JEDEC ID | Notes |
|---|---|---|---|
| MX25L3206E | 4 MB | C2 20 16 | Common in older routers |
| MX25L6406E | 8 MB | C2 20 17 | Routers, cameras |
| MX25L12835F | 16 MB | C2 20 18 | Laptop BIOS |
| MX25L25635F | 32 MB | C2 20 19 | High-capacity BIOS |

#### 5.3.3 GigaDevice (GD25Q Series)

Increasingly common in Chinese-manufactured IoT devices and routers.

| Part Number | Size | JEDEC ID | Notes |
|---|---|---|---|
| GD25Q32C | 4 MB | C8 40 16 | Budget IoT devices |
| GD25Q64C | 8 MB | C8 40 17 | Routers |
| GD25Q128C | 16 MB | C8 40 18 | Cameras, routers |

#### 5.3.4 ISSI (IS25LP Series)

| Part Number | Size | JEDEC ID | Notes |
|---|---|---|---|
| IS25LP032 | 4 MB | 9D 60 16 | Industrial |
| IS25LP064 | 8 MB | 9D 60 17 | Industrial |
| IS25LP128 | 16 MB | 9D 60 18 | Industrial, some BIOS |

#### 5.3.5 Micron/ST (N25Q, M25P Series)

| Part Number | Size | JEDEC ID | Notes |
|---|---|---|---|
| M25P80 | 1 MB | 20 20 14 | Legacy |
| M25P16 | 2 MB | 20 20 15 | Legacy |
| N25Q064 | 8 MB | 20 BA 17 | Xilinx FPGA config |
| N25Q128 | 16 MB | 20 BA 18 | Server BIOS |

#### 5.3.6 XMC (XM25QH Series)

XMC (Wuhan Xinxin Semiconductor) chips are increasingly common in Chinese-manufactured networking and camera equipment. Not all XMC chips are in Flashrom's database.

| Part Number | Size | JEDEC ID | Notes |
|---|---|---|---|
| XM25QH64A | 8 MB | 0B 40 17 | IP cameras |
| XM25QH128A | 16 MB | 0B 40 18 | IP cameras, routers |

> **[!NOTE]**
> If you encounter an XMC chip that Flashrom does not recognize, check the latest Flashrom source for added support. Some XMC chips share JEDEC IDs with equivalent Winbond parts and may be detectable using a Winbond chip definition — but verify chip dimensions against the datasheet before proceeding.

---

### 5.4 Physical Package Reference

#### 5.4.1 SOIC-8 (Standard)

The standard package for most SPI NOR flash. Pin pitch: 1.27mm. Package width: 3.9mm (narrow) or 5.3mm (wide body). The CH341A SOIC-8 clip is designed for this package.

#### 5.4.2 SOIC-16 (Wide)

Used for dual-die chips or chips with Quad SPI interfaces that bring out DQ2/DQ3 lines. Pin pitch: 1.27mm. Requires an SOIC-16 clip (separate purchase from SOIC-8 clip).

#### 5.4.3 QFN/WSON (No-Lead Packages)

Flat packages with exposed pads on the bottom rather than leads on the sides. Cannot be accessed with standard SOIC clips. Options: solder fine-gauge wire to pads, use hot air to temporarily remove chip, or use specialized QFN clip jigs. These packages are much more difficult to work with in-circuit.

#### 5.4.4 DIP-8 (Through-Hole)

Rare in modern equipment but still found in older hardware and some industrial devices. Used in the CH341A's ZIF socket directly. Pin pitch: 2.54mm.

---

### 5.5 Detection Workflow

The complete detection workflow before any read or write operation:

```bash
# Step 1: Verify CH341A is detected by OS
lsusb | grep "1a86:5512"

# Step 2: Attempt chip detection
sudo flashrom -p ch341a_spi 2>&1 | tee detection.log

# Step 3: If detected, get chip name
sudo flashrom -p ch341a_spi --flash-name

# Step 4: Get chip size
sudo flashrom -p ch341a_spi --flash-size

# Step 5: If multiple chips match, specify
sudo flashrom -p ch341a_spi -c "EXACT_CHIP_NAME" --flash-name

# Step 6: If detection fails, try maximum verbosity
sudo flashrom -p ch341a_spi -VVV 2>&1 | tee verbose_detect.log
grep -i "rdid\|id1\|id2\|jedec" verbose_detect.log
```

---

### 5.6 Voltage Requirements by Chip Family

| Chip Family | VCC Voltage | VCC Range | I/O Voltage | Notes |
|---|---|---|---|---|
| W25Q (3.3V variants) | 3.3V | 2.7V–3.6V | 3.3V | Most common |
| W25Q (1.8V variants, W25Q...W suffix) | 1.8V | 1.7V–1.95V | 1.8V | Low-power; NOT compatible with standard CH341A |
| MX25L (3.3V) | 3.3V | 2.7V–3.6V | 3.3V | Standard |
| MX25R (wide voltage) | 1.8V–3.3V | 1.65V–3.6V | VCC level | Some are wide-voltage range |
| GD25Q (3.3V) | 3.3V | 2.7V–3.6V | 3.3V | Standard |
| IS25LP | 3.3V | 2.3V–3.6V | 3.3V | Industrial range |
| N25Q (3.3V) | 3.3V | 2.7V–3.6V | 3.3V | Standard |

> **🔴 DANGER:** 1.8V-only chip variants (identifiable by the "W" suffix in Winbond naming, or "L" vs "LP" in Macronix naming) require a 1.8V programmer. The CH341A, even after the standard 3.3V fix, cannot safely program 1.8V-only chips without additional level-shifting hardware. Always verify the specific part number against its datasheet before connecting.

---

### 5.7 Chapter Summary

SPI NOR flash uses a 4-wire synchronous bus (CLK, CS#, MOSI, MISO) plus WP# and HOLD# control signals. The CH341A acts as SPI master, controlling all signal lines. The JEDEC ID (3 bytes: Manufacturer + Type + Capacity) is the foundation of chip identification in Flashrom. The dominant manufacturers are Winbond, Macronix, GigaDevice, ISSI, and Micron. Most modern SPI NOR flash operates at 3.3V; 1.8V variants exist and require a different voltage supply. SPI NAND is not supported by the CH341A/Flashrom combination. Always verify chip voltage requirements against the datasheet before connecting.


## Chapter 6: I2C EEPROM Operations

### Purpose

While SPI NOR flash is the primary CH341A target, I2C EEPROMs appear frequently in embedded systems storing configuration data, calibration values, serial numbers, and MAC addresses. Understanding how to read and write them extends your hardware security capability to a class of chips that Flashrom cannot access.

### 6.1 I2C Protocol Fundamentals

I2C (Inter-Integrated Circuit) is a two-wire synchronous serial bus. Unlike SPI, all devices share the same SDA (data) and SCL (clock) lines. Both lines are open-drain — pulled HIGH by external resistors (typically 4.7 kΩ to VCC) and pulled LOW by devices to communicate.

Key concepts:
- **Start condition:** SDA falls while SCL is HIGH
- **Stop condition:** SDA rises while SCL is HIGH
- **7-bit addressing:** Up to 127 devices per bus
- **ACK/NACK:** After each byte, receiver pulls SDA LOW (ACK) or leaves HIGH (NACK)
- **Speed modes:** Standard 100 kHz, Fast 400 kHz

### 6.2 24Cxx I2C Address Table

The 24Cxx family uses base address 0x50. The lower 3 bits are set by hardware pins A2, A1, A0:

| A2 | A1 | A0 | 7-bit Address |
|----|----|-----|--------------|
| 0  | 0  | 0  | 0x50 (default)|
| 0  | 0  | 1  | 0x51          |
| 0  | 1  | 0  | 0x52          |
| 0  | 1  | 1  | 0x53          |
| 1  | 0  | 0  | 0x54          |
| 1  | 0  | 1  | 0x55          |
| 1  | 1  | 0  | 0x56          |
| 1  | 1  | 1  | 0x57          |

### 6.3 24Cxx Family Reference

| Part    | Capacity | Page Size | Word Addressing | Notes              |
|---------|----------|-----------|-----------------|-------------------|
| 24C01   | 128 B    | 8 B       | 1-byte          | Legacy             |
| 24C02   | 256 B    | 8 B       | 1-byte          | Most common        |
| 24C04   | 512 B    | 16 B      | 1-byte + A0     | A0 = addr MSB      |
| 24C16   | 2 KB     | 16 B      | 1-byte          | All pins = word addr|
| 24C32   | 4 KB     | 32 B      | 2-byte          | First 2-byte addr  |
| 24C64   | 8 KB     | 32 B      | 2-byte          |                    |
| 24C128  | 16 KB    | 64 B      | 2-byte          |                    |
| 24C256  | 32 KB    | 64 B      | 2-byte          | Common in TVs      |
| 24C512  | 64 KB    | 128 B     | 2-byte          |                    |

Physical package (DIP-8 / SOP-8):

```
    24Cxx DIP-8 / SOP-8

    +--O--+
A0 -|1   8|- VCC
A1 -|2   7|- WP   (pull HIGH to allow writes)
A2 -|3   6|- SCL
GND-|4   5|- SDA
    +-----+
```

### 6.4 Flashrom I2C Limitation

> **[!IMPORTANT]**
> Flashrom v1.6 does **NOT** support I2C EEPROM programming via the `ch341a_spi` programmer string. The `ch341a_spi` backend is exclusively for SPI NOR flash. Use `i2c-tools` and `eeprog` for all I2C EEPROM operations.

### 6.5 i2c-tools Workflow

#### 6.5.1 List I2C Buses

```bash
sudo i2cdetect -l
```

Expected output:
```
i2c-0   i2c      i2c-designware-0      I2C adapter
i2c-1   smbus    SMBus I801 adapter    SMBus adapter
```

#### 6.5.2 Scan Bus for Devices

```bash
# -y = suppress interactive confirmation
sudo i2cdetect -y 1
```

Expected output with device at 0x50:
```
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: 50 -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
```

Each cell = one 7-bit address. `50` = device at 0x50. `--` = no response.

#### 6.5.3 i2cdump — Dump EEPROM Contents

```bash
sudo i2cdump -y 1 0x50          # default mode
sudo i2cdump -y 1 0x50 b        # byte mode (safer for sensitive chips)
sudo i2cdump -y -r 0x00-0xff 1 0x50  # specific byte range
```

Example output:
```
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f    0123456789abcdef
00: 4d 41 43 3a 61 62 63 64 65 66 ff ff ff ff ff ff    MAC:abcdef......
10: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
```

#### 6.5.4 i2cget — Read Individual Bytes

```bash
sudo i2cget -y 1 0x50 0x00        # read byte at addr 0x00
sudo i2cget -y 1 0x50 0x00 w      # read 16-bit word
```

Flags: `-y` = no prompt; `1` = bus; `0x50` = device; `0x00` = register; `b`/`w` = byte/word.

Expected output: `0x4d`

#### 6.5.5 i2cset — Write Individual Bytes

```bash
sudo i2cset -y 1 0x50 0x00 0xAB   # write 0xAB to address 0x00
```

> **CAUTION:** i2cset writes immediately with no undo. Always dump before writing.

After each write, add `sleep 0.01` — 24Cxx chips need 5–10 ms write cycle time before accepting new commands.

#### 6.5.6 i2ctransfer — 2-Byte Addressed Chips (24C32 and larger)

```bash
# Write 2-byte address 0x0000, then read 32 bytes
sudo i2ctransfer -y 1 w2@0x50 0x00 0x00 r32@0x50

# Read from offset 0x100 (= 0x01 0x00)
sudo i2ctransfer -y 1 w2@0x50 0x01 0x00 r32@0x50
```

### 6.6 eeprog Workflow

```bash
sudo apt install eeprog -y
eeprog --help
```

Reading:
```bash
# 24C02 — 256 bytes, 8-bit addressing
sudo eeprog /dev/i2c-1 0x50 -8 -r 0:256 -o eeprom_24c02.bin

# 24C256 — 32768 bytes, 16-bit addressing
sudo eeprog /dev/i2c-1 0x50 -16 -r 0:32768 -o eeprom_24c256.bin
```

Flags: `-8` = 1-byte word addressing (24C01–24C16); `-16` = 2-byte (24C32–24C512).

Writing:
```bash
sudo eeprog /dev/i2c-1 0x50 -16 -w modified_eeprom.bin
```

### 6.7 Common I2C Errors

| Error | Cause | Solution |
|-------|-------|---------|
| `/dev/i2c-N` not found | Module not loaded | `sudo modprobe i2c-dev` |
| No device in i2cdetect | Wrong bus/address | Verify connections and A0-A2 pins |
| Write fails, readback = 0xFF | WP pin LOW | Connect WP (pin 7) to VCC |
| Data differs between reads | Intermittent connection | Check pull-up resistors and wiring |
| NACK after write | Still in write cycle | Add `sleep 0.01` between writes |
| `Permission denied /dev/i2c-*` | No permissions | `sudo chmod a+rw /dev/i2c-*` |

### 6.8 Chapter Summary

I2C EEPROMs (24Cxx) use SDA/SCL with 7-bit addressing (0x50–0x57). Flashrom does not support I2C — use i2c-tools and eeprog. Chips ≤2 KB use `-8` addressing; ≥4 KB use `-16`. WP pin must be HIGH to enable writes. Always double-read and compare SHA256 before trusting any dump.

---

## Chapter 7: Complete Firmware Dump Workflow

### Purpose

This chapter documents the complete, systematic procedure for acquiring a verified firmware image using the CH341A. Follow this workflow exactly for every dump.

### Theory

Firmware acquisition from SPI NOR flash is physically simple (30 seconds to clip a chip) but procedurally critical (10 minutes for verification prevents hours of recovery work). The double-read protocol exists because clip connections are mechanically unreliable — two reads with matching SHA256 hashes provide high confidence the data is correct.

### 7.1 Pre-Connection Checklist

```
PRE-CONNECTION CHECKLIST
========================
[ ] Target chip part number identified from PCB markings
[ ] Chip datasheet checked — VCC confirmed (3.3V or 1.8V?)
[ ] CH341A voltage measured with multimeter — confirmed 3.3V ± 0.1V
[ ] Correct clip selected (SOIC-8 or SOIC-16)
[ ] Clip inspected — no bent or missing spring pins
[ ] All 8 clip wires tested for continuity
[ ] ESD wrist strap worn and connected to ground
[ ] Target board POWERED OFF — mains disconnected — battery removed
[ ] Working directory created: ~/firmware_dumps/DATE_TARGET/
[ ] CH341A connected directly to USB port (not via hub)
[ ] lsusb confirms: 1a86:5512
[ ] lsmod shows ch341 is NOT loaded
```

### 7.2 Finding Pin 1

```
Three ways manufacturers mark Pin 1:

1. Dot on package body (most reliable):
   The physical dot or circular indent marks pin 1.
   When chip text reads left-to-right, pin 1 is top-left.

2. Chamfer (beveled corner):
   One corner of the package body is cut at an angle.
   Pin 1 is at the chamfered corner.

3. PCB silkscreen:
   A line, dot, or "1" printed on the PCB near pin 1
   of the chip footprint.
```

### 7.3 Clip Placement

```
CORRECT ORIENTATION:
  Clip pin 1 (marked end) aligns with chip pin 1 (dot/chamfer)
  Clip: [1][2][3][4] ... [8][7][6][5]
  Chip: [1][2][3][4] ... [8][7][6][5]

WRONG ORIENTATION (rotated 180 degrees):
  Clip: [1][2][3][4] ... [8][7][6][5]
  Chip: [8][7][6][5] ... [1][2][3][4]
  Result: VCC connected to CS# — possible immediate damage

Steps:
1. Identify pin 1 on chip (dot/chamfer)
2. Orient clip so pin 1 marker aligns with chip pin 1
3. Squeeze clip handles to open jaws
4. Lower straight down onto chip body
5. Release — spring closes onto chip leads
6. Visually verify: all clip pins contact all chip leads
```

### 7.4 In-Circuit vs Out-of-Circuit

**In-circuit** (chip remains soldered on PCB):
- SoC must be completely powered off (mains + battery removed)
- Wait 30 seconds for capacitors to discharge
- Bus contention risk: target SoC SPI controller may also be on the bus

**Out-of-circuit** (chip desoldered):
- Cleanest and most reliable approach
- Use hot air station at 330–380°C with flux to desolder
- Program in ZIF socket (DIP-8) or on breakout board (SOIC-8)

> **CAUTION:** Never power the target board while the CH341A clip is attached.

### 7.5 First Detection

```bash
TARGET="TPLINK_WR841N"
DATE=$(date +%Y%m%d_%H%M%S)
WORKDIR="$HOME/firmware_dumps/${DATE}_${TARGET}"
mkdir -p "$WORKDIR"
cd "$WORKDIR"

lsusb | grep "1a86:5512" || { echo "ERROR: CH341A not detected"; exit 1; }
echo "CH341A detected"

if lsmod | grep -q "^ch341"; then
    sudo modprobe -r ch341
    sleep 1
fi

sudo flashrom -p ch341a_spi 2>&1 | tee detection.log
```

| Output | Meaning | Action |
|--------|---------|--------|
| `Found Winbond flash chip "W25Q128.V"` | Chip detected | Proceed to read |
| `No EEPROM/flash device found` | Not detected | Troubleshoot Ch.10 |
| `Multiple flash chip definitions match` | Ambiguous | Use `-c` flag |
| `Programmer initialization failed` | Module conflict | `modprobe -r ch341` |

### 7.6 The Double-Read Protocol

```bash
echo "[*] Read 1 of 2..."
sudo flashrom -p ch341a_spi -r "${TARGET}_read1.bin" 2>&1 | tee read1.log
SHA1=$(sha256sum "${TARGET}_read1.bin" | cut -d' ' -f1)
echo "[*] SHA256 R1: $SHA1"

echo "[*] Read 2 of 2 — DO NOT MOVE THE CLIP"
sudo flashrom -p ch341a_spi -r "${TARGET}_read2.bin" 2>&1 | tee read2.log
SHA2=$(sha256sum "${TARGET}_read2.bin" | cut -d' ' -f1)
echo "[*] SHA256 R2: $SHA2"

if [ "$SHA1" = "$SHA2" ]; then
    echo "[OK] READS MATCH — dump verified"
    cp "${TARGET}_read1.bin" "${TARGET}_VERIFIED.bin"
    sha256sum *.bin > SHA256SUMS.txt
else
    echo "[ERROR] READS DIFFER — troubleshoot before proceeding"
    cmp "${TARGET}_read1.bin" "${TARGET}_read2.bin" | head -3
fi
```

**If reads differ:**
- Re-seat clip carefully → retry
- Verify target is completely powered off → retry
- Measure VCC at chip pin 8 (must be 3.3V) → retry
- Run 3 reads; use 2-of-3 majority pair if they agree
- Test all 8 clip wire continuities; replace clip if any fail
- Remove chip from PCB and program out-of-circuit

### 7.7 File Naming and Storage

```
Naming format: {VENDOR}_{MODEL}_{CHIP}_{YYYYMMDD}_{type}.bin

Examples:
  TPLINK_WR841N_W25Q64FV_20260626_read1.bin
  TPLINK_WR841N_W25Q64FV_20260626_VERIFIED.bin
  TPLINK_WR841N_W25Q64FV_20260626_MODIFIED.bin

Directory structure:
  ~/firmware_dumps/
  +-- 20260626_TPLINK_WR841N/
      +-- detection.log
      +-- read1.log, read2.log
      +-- *_read1.bin, *_read2.bin
      +-- *_VERIFIED.bin
      +-- SHA256SUMS.txt
```

```bash
sha256sum *.bin > SHA256SUMS.txt
cat SHA256SUMS.txt
```

### 7.8 Automated Dump Script

```bash
#!/bin/bash
# CH341A Firmware Dump Script
# Usage: ./dump.sh <target_name> [chip_name]
set -euo pipefail
TARGET="${1:?Usage: $0 <target_name> [chip_name]}"
CHIP_OPT=""
[ -n "${2:-}" ] && CHIP_OPT="-c $2"
DATE=$(date +%Y%m%d_%H%M%S)
WORKDIR="$HOME/firmware_dumps/${DATE}_${TARGET}"
mkdir -p "$WORKDIR"
cd "$WORKDIR"

echo "========================================"
echo "  CH341A Firmware Dump: $TARGET"
echo "  Directory: $WORKDIR"
echo "========================================"

lsusb | grep -q "1a86:5512" || { echo "[ERROR] CH341A not detected"; exit 1; }
echo "[OK] CH341A detected"
lsmod | grep -q "^ch341" && { sudo modprobe -r ch341; sleep 1; } || true

echo "[*] Detecting chip..."
sudo flashrom -p ch341a_spi $CHIP_OPT 2>&1 | tee detection.log | grep -E "Found|No EEPROM"

echo "[*] Read 1..."
sudo flashrom -p ch341a_spi $CHIP_OPT -r "${TARGET}_read1.bin" 2>&1 | tee read1.log
SHA1=$(sha256sum "${TARGET}_read1.bin" | cut -d' ' -f1)
echo "[*] SHA256 R1: $SHA1"

echo "[*] Read 2..."
sudo flashrom -p ch341a_spi $CHIP_OPT -r "${TARGET}_read2.bin" 2>&1 | tee read2.log
SHA2=$(sha256sum "${TARGET}_read2.bin" | cut -d' ' -f1)
echo "[*] SHA256 R2: $SHA2"

if [ "$SHA1" = "$SHA2" ]; then
    cp "${TARGET}_read1.bin" "${TARGET}_VERIFIED.bin"
    sha256sum *.bin > SHA256SUMS.txt
    echo ""
    echo "[OK] DUMP VERIFIED: ${TARGET}_VERIFIED.bin"
    echo "[*] SHA256: $SHA1"
else
    echo "[ERROR] READS MISMATCH — check clip, power, voltage"
    exit 1
fi
```

### 7.9 Common Mistakes

| Mistake | Consequence | Prevention |
|---------|-------------|------------|
| Skipping voltage verification | Chip damaged by 5V | Measure every session |
| Target board powered on | Bus contention, bad reads | Remove ALL power |
| Single read without comparison | Undetected bit errors | Always double-read |
| Wrong clip orientation | No detection or corrupt data | Verify pin 1 first |
| Not logging flashrom output | No troubleshooting record | Always use `tee` |

### 7.10 Chapter Summary

The dump workflow: pre-connection checklist → pin 1 identification → clip placement → detection → double-read with SHA256 comparison → organized storage. No firmware is reliable without two matching reads. Target must be completely powered off. Log all output with `tee`.

---

## Chapter 8: Writing Firmware Safely

### Purpose

Writing firmware is the highest-risk CH341A operation. A failed write can permanently brick a device. This chapter documents the safe write workflow, recovery procedures, and write protection handling.

### Theory

NOR flash writing is a three-phase process that Flashrom handles automatically:
1. **Erase** — sectors reset to 0xFF (required before programming)
2. **Program** — data written page-by-page (typically 256 bytes/page)
3. **Verify** — read-back confirms correctness

Flashrom only erases and rewrites sectors that differ from the new firmware, preserving flash cell lifespan.

### 8.1 Pre-Write Checklist

```
PRE-WRITE CHECKLIST — complete ALL items before writing
========================================================
[ ] Verified backup from double-read EXISTS with confirmed SHA256
[ ] Backup stored in at least 2 separate locations
[ ] New firmware SHA256 documented
[ ] New firmware size verified <= chip size
[ ] New firmware is NOT all-0xFF (empty/erased image)
[ ] Target board powered off and disconnected
[ ] CH341A voltage confirmed 3.3V with multimeter
[ ] Clip securely seated with pin 1 aligned
[ ] WP# pin state verified — must be HIGH to allow writes
```

### 8.2 Pre-Write Preparation

```bash
NEW_FIRMWARE="new_firmware.bin"

# Final backup immediately before write
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
sudo flashrom -p ch341a_spi -r "prewrite_backup_${TIMESTAMP}.bin" 2>&1
echo "Backup SHA256: $(sha256sum prewrite_backup_${TIMESTAMP}.bin)"

# Validate size
FW_SIZE=$(stat -c%s "$NEW_FIRMWARE")
CHIP_SIZE=$(sudo flashrom -p ch341a_spi --flash-size 2>/dev/null)
echo "Firmware: $FW_SIZE bytes | Chip: $CHIP_SIZE bytes"

if [ "$FW_SIZE" -gt "$CHIP_SIZE" ]; then
    echo "ERROR: Firmware is LARGER than chip — cannot write"
    exit 1
fi

# Pad to chip size if needed (fill with 0xFF)
if [ "$FW_SIZE" -lt "$CHIP_SIZE" ]; then
    PAD_BYTES=$(( CHIP_SIZE - FW_SIZE ))
    dd if=/dev/zero bs=1 count="$PAD_BYTES" 2>/dev/null | tr '\000' '\377' > pad.bin
    cat "$NEW_FIRMWARE" pad.bin > firmware_padded.bin
    NEW_FIRMWARE="firmware_padded.bin"
    echo "Padded firmware to $CHIP_SIZE bytes"
fi

echo "Final SHA256: $(sha256sum $NEW_FIRMWARE)"
```

### 8.3 The Write Command

```bash
sudo flashrom -p ch341a_spi -w "$NEW_FIRMWARE" 2>&1 | tee write.log
```

Flags: `-p ch341a_spi` = CH341A programmer; `-w file` = write this file to flash.

Expected output (successful write):
```
Found Winbond flash chip "W25Q128.V" (16384 kB, SPI) on ch341a_spi.
Reading old flash chip contents... done.
Erasing and writing flash chip... Erase/write done.
Verifying flash... VERIFIED.
```

Total time for a 16 MB write: typically 5–20 minutes depending on how much content changes.

What Flashrom does automatically:
1. Reads entire current chip
2. Compares with new firmware byte-by-byte
3. Erases only differing sectors
4. Writes new data
5. Reads back all written sectors to verify

### 8.4 Post-Write Verification

```bash
# Independent standalone verify
sudo flashrom -p ch341a_spi -v "$NEW_FIRMWARE" 2>&1 | tee verify.log
grep "VERIFIED\|FAILED" verify.log

# Read back chip and compare SHA256
sudo flashrom -p ch341a_spi -r post_write_readback.bin
SHA_FW=$(sha256sum "$NEW_FIRMWARE" | cut -d' ' -f1)
SHA_CHIP=$(sha256sum post_write_readback.bin | cut -d' ' -f1)

if [ "$SHA_FW" = "$SHA_CHIP" ]; then
    echo "[OK] WRITE VERIFIED — chip matches new firmware"
else
    echo "[ERROR] SHA256 MISMATCH — write may have failed"
fi
```

### 8.5 Writing Specific Regions with Layout Files

```bash
# Create layout file — verify all offsets against device docs first
cat > flash.layout << 'EOF'
00000000:0001ffff bootloader
00020000:001effff firmware
001f0000:001fffff nvram
EOF

# Write only the firmware region
sudo flashrom -p ch341a_spi \
    -l flash.layout \
    -i firmware \
    -w new_firmware_region.bin \
    2>&1 | tee region_write.log
```

> **DANGER:** Incorrect layout offsets can overwrite the bootloader. Triple-check hex addresses before any region write.

### 8.6 Write Protection

**Hardware WP# (pin 3 of SOIC-8):**
- WP# = HIGH (VCC): writes allowed
- WP# = LOW (GND): writes blocked

Diagnose with multimeter (target powered off):
- WP# to GND ≈ 0 Ω: hardwired LOW = hardware write protected
- WP# to GND shows pull-up resistor value: write enabled by default

To override: apply 3.3V to WP# pin during programming.  
> **CAUTION:** Confirm pin identity from chip datasheet before applying voltage.

**Software write protection:** Flashrom handles clearing status register BP bits automatically for supported chips.

### 8.7 Recovery from Failed Write

```bash
# Step 1: Assess current chip state
sudo flashrom -p ch341a_spi -r damage_check.bin
echo "Current SHA256: $(sha256sum damage_check.bin)"

# Step 2: Erase chip
sudo flashrom -p ch341a_spi -E

# Step 3: Restore original backup
sudo flashrom -p ch341a_spi -w "${TARGET}_VERIFIED.bin"

# Step 4: Verify restoration
sudo flashrom -p ch341a_spi -v "${TARGET}_VERIFIED.bin"
echo "Recovery complete"
```

If no backup exists: check the manufacturer website, FCC ID database (fcc.io), or extract firmware from an identical device. This situation is entirely avoidable with pre-write backup discipline.

### 8.8 Chapter Summary

Safe writing requires: verified backup before every write, new firmware validation (size, SHA256, content check), Flashrom's erase/write/verify pipeline, and independent post-write SHA256 read-back. Write protection appears as WP# pin (hardware) or BP bits (software). Recovery from failed writes uses the original backup. Without a backup, recovery may be impossible.

---

## Chapter 9: Firmware Analysis

### Purpose

Extracting firmware is only the beginning. Security value lies in what the firmware contains: credentials, private keys, API endpoints, and vulnerable code. This chapter documents systematic triage and analysis.

### 9.1 Initial Triage

#### 9.1.1 file — Format Identification

```bash
file firmware.bin
```

| Output | Meaning |
|--------|---------|
| `data` | Unrecognized — may be raw, compressed, or encrypted |
| `Linux kernel ARM boot executable zImage` | Compressed kernel |
| `gzip compressed data` | gzip-compressed blob |
| `Squashfs filesystem, little endian` | SquashFS root filesystem |
| `ASCII text` | Scripts, configuration files |
| `ELF 32-bit LSB executable, ARM` | ARM binary |

#### 9.1.2 xxd — Hex Inspection

```bash
xxd firmware.bin | head -16            # First 256 bytes
xxd -s 0x1000 -l 256 firmware.bin     # At specific offset
xxd firmware.bin | grep '1f8b'         # Search for gzip magic
```

Common magic bytes:

| Hex | Format |
|-----|--------|
| `7f 45 4c 46` | ELF binary |
| `1f 8b` | gzip compressed |
| `fd 37 7a 58 5a 00` | xz compressed |
| `68 73 71 73` | SquashFS (little-endian) |
| `27 05 19 56` | U-Boot uImage |
| `d0 0d fe ed` | Device Tree Blob |
| `42 5a 68` | bzip2 compressed |

#### 9.1.3 strings — Extracting Printable Text

```bash
strings firmware.bin | head -50          # Basic (min 4 chars)
strings -n 8 firmware.bin               # Minimum 8 chars (less noise)
strings -o firmware.bin | head -20      # With byte offset of each string
strings -e l firmware.bin               # UTF-16 LE (UEFI/Windows)
strings -e b firmware.bin               # UTF-16 BE
```

### 9.2 Security-Targeted String Searches

```bash
# Credentials
strings firmware.bin | grep -iE '(password|passwd|pwd|secret|credential)' | sort -u

# Private keys
strings firmware.bin | grep -E '-----BEGIN'

# URLs and API endpoints
strings firmware.bin | grep -iE 'https?://[^[:space:]]+'

# IP addresses
strings firmware.bin | grep -E '\b([0-9]{1,3}\.){3}[0-9]{1,3}\b'

# Default credential patterns
strings firmware.bin | grep -iE '(admin|root|guest|default|login)' | sort -u

# Filesystem paths (potential backdoors)
strings firmware.bin | grep -E '^/[a-zA-Z]' | sort -u

# Version information
strings firmware.bin | grep -iE '(version|firmware|build|v[0-9]+\.[0-9]+)'

# WiFi and network configuration
strings firmware.bin | grep -iE '(ssid|wpa|psk|wifi|mqtt)' | sort -u

# Cloud / IoT endpoints
strings firmware.bin | grep -iE '(amazonaws|azure|googleapis|api\.)' | sort -u
```

### 9.3 binwalk Analysis (v2.x)

#### 9.3.1 Signature Scan

```bash
binwalk firmware.bin
```

Example output for a router firmware:
```
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             TRX firmware header, little endian, size: 5242880
28            0x1C            LZMA compressed data, properties: 0x5D
1048576       0x100000        Squashfs filesystem, little endian, v4.0, lzma
```

Column meanings:
- `DECIMAL` — byte offset from start of file
- `HEXADECIMAL` — same offset in hex  
- `DESCRIPTION` — detected format and extracted metadata

#### 9.3.2 Entropy Analysis

```bash
binwalk -E firmware.bin
pip3 install matplotlib && binwalk -E firmware.bin  # generates PNG graph
```

| Entropy Range | Interpretation |
|---------------|----------------|
| 0.0 – 0.2 | Empty/padding (0xFF or 0x00) |
| 0.3 – 0.7 | Code, structured data, headers |
| 0.7 – 0.9 | Compressed data (lzma, gzip) |
| 0.95 – 1.0 | Encrypted or truly random data |

Uniformly high entropy throughout the file indicates encryption.

#### 9.3.3 Extraction

```bash
binwalk -e firmware.bin         # Extract to _firmware.bin.extracted/
binwalk -Me firmware.bin        # Recursive extraction
binwalk -ev firmware.bin        # Verbose extraction
```

> **NOTE:** binwalk -Me can generate large amounts of data. Monitor disk space.

#### 9.3.4 Raw String and Byte Search

```bash
binwalk -R '\x1f\x8b' firmware.bin    # Search for gzip magic bytes
binwalk -R 'admin' firmware.bin          # Search for string
binwalk -R 'password' firmware.bin
```

### 9.4 Analyzing the Extracted Filesystem

```bash
cd _firmware.bin.extracted/
ls -la

SQFS=$(ls *.squashfs 2>/dev/null | head -1)
mkdir /tmp/router_fs
sudo mount -t squashfs -o loop "$SQFS" /tmp/router_fs
# Alternative if mount fails:
unsquashfs -d /tmp/router_fs "$SQFS"
```

#### 9.4.1 Credential Extraction

```bash
FSROOT="/tmp/router_fs"
cat "$FSROOT/etc/passwd" 2>/dev/null
cat "$FSROOT/etc/shadow" 2>/dev/null
grep -r --include="*.conf" --include="*.cfg" \
    -iE '(password|passwd|secret)\s*[=:]' \
    "$FSROOT/etc/" 2>/dev/null
```

#### 9.4.2 Certificates and Private Keys

```bash
find "$FSROOT" \( -name "*.pem" -o -name "*.crt" -o -name "*.key" \) 2>/dev/null
grep -rl '-----BEGIN' "$FSROOT/" 2>/dev/null
find "$FSROOT/etc" -name "ssh_host_*" 2>/dev/null
```

#### 9.4.3 Binary Security Properties

```bash
file "$FSROOT/bin/busybox"
readelf -l "$FSROOT/bin/busybox" 2>/dev/null | grep -E "GNU_STACK|GNU_RELRO"
find "$FSROOT" -perm -4000 2>/dev/null   # SUID binaries
```

### 9.5 Encrypted Firmware

Signs: uniformly high entropy (>0.95), no recognizable headers, binwalk finds nothing.

Do **NOT** invent decryption approaches. Instead:
1. Analyze the accessible bootloader section for key material
2. Search for older unencrypted firmware from the manufacturer
3. Check CVE databases for disclosed encryption keys for this device model
4. Document the limitation explicitly — encrypted firmware cannot be statically analyzed without the key

### 9.6 Initial Triage Script

```bash
#!/bin/bash
# Usage: ./triage.sh <firmware.bin>
FW="${1:?Usage: $0 <firmware.bin>}"
OUT="triage_$(basename "$FW" .bin)_$(date +%Y%m%d_%H%M%S)"
mkdir -p "$OUT"

echo "=== FIRMWARE TRIAGE: $FW ==="
file "$FW" | tee "$OUT/01_file_type.txt"
sha256sum "$FW" | tee "$OUT/02_sha256.txt"
stat -c "Size: %s bytes" "$FW"
xxd "$FW" | head -2 | tee "$OUT/03_magic.txt"
strings -n 8 "$FW" | grep -iE \
    '(password|passwd|BEGIN.*KEY|https?://|admin|root|version)' \
    | sort -u | tee "$OUT/04_strings.txt"
binwalk "$FW" | tee "$OUT/05_binwalk.txt"
echo "=== Results in: $OUT ==="
```

### 9.7 Chapter Summary

Firmware analysis: `file` and `xxd` for format identification; `strings` for credential/endpoint hunting; `binwalk` for component identification and extraction. The extracted filesystem provides /etc/shadow, SSL keys, configs, and binaries. High entropy throughout indicates encryption. Document every finding with its byte offset and the command used.

---

## Chapter 10: Troubleshooting

### Purpose

Systematic diagnosis and resolution for every common failure mode with CH341A and Flashrom on Ubuntu 24.04 LTS.

### 10.1 Baseline Diagnostic Sequence

Always run this first before investigating specific errors:

```bash
lsusb | grep "1a86"                                          # USB detection
lsmod | grep ch341                                            # Module status
sudo flashrom -p ch341a_spi -VVV 2>&1 | tee "diag_$(date +%Y%m%d).log"
dmesg | grep -i "usb\|ch341\|xhci" | tail -20
```

### 10.2 JEDEC ID = 0xFF 0xFF 0xFF

**Symptom:** `No EEPROM/flash device found`. Verbose output shows `id1 0xff`.
**Meaning:** MISO line is floating or stuck HIGH — no chip response received.

| Cause | Check | Fix |
|-------|-------|-----|
| Clip not making contact | Wiggle test | Re-seat clip |
| Wrong pin 1 orientation | Visual check of dot | Rotate clip 180° |
| Target board powered on | Check power LEDs | Remove all power |
| 5V on 3.3V chip | Measure VCC at pin 8 | Apply 3.3V fix |
| Broken clip wire | Continuity test all 8 wires | Replace clip |
| ch341 module loaded | `lsmod \| grep ch341` | `sudo modprobe -r ch341` |

### 10.3 JEDEC ID = 0x00 0x00 0x00

**Symptom:** Verbose shows `id1 0x00`. MISO stuck LOW.
**Meaning:** Short circuit on SPI bus or dead chip.

Before applying power: measure resistance between VCC (pin 8) and GND (pin 4):
- High resistance (>1 kΩ) = OK
- Near 0 Ω = short circuit — **DO NOT POWER UP**

Fix: inspect for solder bridges under magnification; replace clip if pins are shorting; replace damaged chip.

### 10.4 Random or Garbage JEDEC ID

**Symptom:** Different chip detected on each run.
**Cause:** Bus contention — target SoC's SPI controller also responding.

```bash
for i in {1..5}; do
    echo -n "Run $i: "
    sudo flashrom -p ch341a_spi --flash-name 2>&1 | grep -E "Found|No EEPROM"
done
# Changing output = bus contention
```

Fix: remove ALL power from target board (mains + battery). If still inconsistent in-circuit, desolder chip for out-of-circuit programming.

### 10.5 Multiple Chip Matches

```
Multiple flash chip definitions match: "W25Q128.V" "W25Q128JV"
Please specify with -c <chipname>
```

This is NOT an error — it is a disambiguation request.

```bash
sudo flashrom -p ch341a_spi -L | grep -i "W25Q128"
sudo flashrom -p ch341a_spi -c "W25Q128.V" -r backup.bin
```

Match the chip name to your physical PCB markings.

### 10.6 Permission Denied

```bash
sudo flashrom -p ch341a_spi                              # Option 1: use sudo
cat /etc/udev/rules.d/60-ch341a.rules                   # Verify rule exists
sudo udevadm control --reload-rules && sudo udevadm trigger
sudo usermod -aG plugdev $USER                            # Add to group
# Log out and log back in for group change to take effect
groups $USER | grep plugdev                               # Verify membership
```

### 10.7 ch341 Kernel Module Conflict

```bash
lsmod | grep ch341           # Check if loaded
sudo modprobe -r ch341       # Unload it
sudo flashrom -p ch341a_spi  # Retry

# Permanent blacklist (disables USB-serial functionality for CH341A)
echo 'blacklist ch341' | sudo tee /etc/modprobe.d/blacklist-ch341.conf
sudo update-initramfs -u
```

### 10.8 Inconsistent Reads

```bash
for i in 1 2 3; do
    sudo flashrom -p ch341a_spi -r "r${i}.bin" 2>/dev/null
    echo "r$i: $(sha256sum r${i}.bin | cut -d' ' -f1)"
done
cmp r1.bin r2.bin && echo "r1=r2" || echo "DIFFER"
```

Causes in order of likelihood:
1. Loose clip — re-seat
2. Bus contention — power off target completely
3. Insufficient USB power — try a different port, avoid hubs
4. Long or noisy cable — use cables under 30 cm
5. Defective clip — replace
6. Damaged chip — if all above clear, chip has failed cells

### 10.9 Write Verification Failed

```
Verifying flash... FAILED at 0x00040000! Expected=0xAB, Got=0xFF
```

| Cause | Solution |
|-------|----------|
| Hardware WP# pin LOW | Override WP# with 3.3V (Section 8.6) |
| Software write protection | Flashrom should clear; check verbose output |
| Clip lost contact during write | Re-seat; restore backup; retry |
| Power interruption | Use direct USB port; retry |
| Damaged chip | Replace chip |

### 10.10 USB Disconnect During Operation

```bash
dmesg | grep -i "disconnect\|reset\|error" | tail -10
```

Solutions:
- Replace USB cable with a quality data cable (not a charge-only cable)
- Connect CH341A directly to a motherboard USB 2.0 port
- Eliminate all USB hubs from the chain
- Try USB 2.0 port if currently using USB 3.0 (known compatibility issues)

### 10.11 Complete Decision Tree

```
"No EEPROM/flash device found"
               |
     +---------+---------+
     |                   |
CH341A in lsusb?     Not in lsusb
(lsusb | grep 1a86)       |
     |                Check cable/port
    YES               dmesg | grep usb
     |
     +-- lsmod | grep ch341 shows module?
     |   YES --> modprobe -r ch341 --> retry
     |
     +-- Clip pin 1 aligned with chip pin 1 (dot)?
     |   NO  --> rotate clip 180 degrees --> retry
     |
     +-- Target completely powered off (mains + battery)?
     |   NO  --> remove all power --> retry
     |
     +-- VCC at chip pin 8 = 3.3V +/- 0.1V?
     |   NO  --> fix CH341A voltage (Chapter 2) --> retry
     |
     +-- All 8 clip wires pass continuity test?
     |   NO  --> replace clip --> retry
     |
     +-- Run: sudo flashrom -p ch341a_spi -VVV
     |   JEDEC = ff ff ff --> clip / power / orientation issue
     |   JEDEC = 00 00 00 --> short circuit on SPI bus
     |   JEDEC = random   --> bus contention (power off target board)
     |   JEDEC = valid, unknown --> chip not in Flashrom database
     |
     +-- Remove chip from PCB, program out-of-circuit
```

### 10.12 Error Reference Table

| Error / Symptom | Probable Cause | Diagnostic Command | Solution |
|-----------------|----------------|-------------------|---------|
| `No EEPROM/flash device found` | Clip, voltage, orientation, contention | `flashrom -VVV` | Sections 10.2–10.4 |
| `Permission denied` | No udev rule or wrong group | `groups $USER` | Set up udev rules |
| `Programmer initialization failed` | ch341 module conflict | `lsmod \| grep ch341` | `modprobe -r ch341` |
| Inconsistent reads | Loose clip, bus contention | SHA256 of 3 reads | Re-seat, power off target |
| `Verification FAILED` | Write protection, clip issue | `flashrom -VV` | Check WP# pin |
| USB disconnect mid-operation | Cable, hub, power | `dmesg \| grep usb` | Direct port, quality cable |
| JEDEC = `ff ff ff` | No chip response | `flashrom -VVV` | Clip, orientation, voltage |
| JEDEC = `00 00 00` | Short circuit or dead chip | Resistance test | Inspect for shorts |
| Multiple chip matches | Ambiguous JEDEC ID | `flashrom -L` | Use `-c` to specify |
| Chip hot to touch | 5V on 3.3V chip | Measure VCC | Disconnect; fix voltage |

### 10.13 Chapter Summary

Diagnostic order: USB detection → module status → physical clip → voltage → Flashrom verbose output. JEDEC 0xFF = no response; 0x00 = short; random = contention. Permission denied resolves with udev rules. Module conflicts resolve with `modprobe -r ch341`. Inconsistent reads indicate clip or contention issues. Log every diagnostic session to a file.


---

## Chapter 11: Electrical Troubleshooting with a Multimeter

### Purpose

A digital multimeter (DMM) is an indispensable companion to the CH341A. This chapter documents every measurement procedure relevant to CH341A work: voltage verification, continuity testing, resistance measurement on SPI bus lines, and diagnosing write protection.

### 11.1 Multimeter Modes Relevant to CH341A Work

| Mode | Symbol | Use |
|------|--------|-----|
| DC Voltage | VDC or V⎓ | Verify 3.3V supply, check logic levels |
| Continuity | Ω with beep | Test clip wires, verify connections |
| Resistance | Ω | Measure pull-up/pull-down resistors |
| Diode | →| | Check ESD protection diodes on chip |

### 11.2 Verifying CH341A Output Voltage

> **This is the single most important measurement.** Perform before every session.

```
Procedure:
1. Connect CH341A to USB (no chip in clip or ZIF socket)
2. Set multimeter to DC Voltage, 20V range
3. Insert RED probe into VCC pin position of SOIC-8 header
4. Insert BLACK probe into GND pin position (pin 4)
5. Read display

SAFE:    3.2V – 3.4V     → Proceed
WARNING: 3.5V – 4.4V     → Check LDO regulator — may be marginal
DANGER:  4.5V – 5.3V     → DO NOT CONNECT CHIP — apply 3.3V fix first
```

On the SOIC-8 header on the CH341A board:
- **VCC = pin 8 position** (rightmost top pin)
- **GND = pin 4 position** (leftmost bottom pin)

### 11.3 Clip Wire Continuity Testing

A broken clip wire is one of the most common causes of failed reads. Test every wire before each session.

```bash
# Procedure (with CH341A DISCONNECTED from USB):
# 1. Set multimeter to continuity mode (beep or <5Ω)
# 2. Place RED probe on pin N of SOIC-8 clip jaws (with jaws open, touching contacts)
# 3. Place BLACK probe on pin N of the header connector on the other end
# 4. Repeat for all 8 pins
# 5. All 8 should beep/show low resistance
# 6. Wiggle the cable while testing — intermittent failures will break continuity momentarily
```

| Pin | Signal | Expected Continuity |
|-----|--------|-------------------|
| 1 | CS# | Yes |
| 2 | MISO | Yes |
| 3 | WP# | Yes |
| 4 | GND | Yes |
| 5 | MOSI | Yes |
| 6 | CLK | Yes |
| 7 | HOLD# | Yes |
| 8 | VCC | Yes |

### 11.4 Measuring WP# Pin State (Write Protection Diagnosis)

```
Procedure (target board powered OFF):
1. Identify WP# pin (pin 3 of SOIC-8 chip)
2. Set multimeter to DC Voltage or Resistance
3. Measure WP# to GND

Result interpretation:
  ~0 Ω to GND:        WP# is hardwired LOW = write protection ENABLED
  Pull-up resistor:   WP# defaults HIGH = write protection DISABLED
  3.3V to GND:        WP# is driven HIGH explicitly = writes allowed

To enable writes when WP# is hardwired LOW:
  - Carefully apply 3.3V to WP# pin during programming
  - Verify with multimeter that VCC is applied to the correct pin
```

### 11.5 Diagnosing Short Circuits Before Powering Up

Before connecting power to any SPI bus, verify there are no short circuits:

```
Procedure (power OFF, no clip attached):
1. Set multimeter to Resistance (20 kΩ range)
2. Measure between VCC (pin 8) and GND (pin 4) of the CH341A SOIC-8 header
   Expected: HIGH resistance (>10 kΩ) — no short
   Dangerous: <100 Ω — short circuit present, find cause before powering

3. With SOIC-8 clip attached to chip (target powered OFF):
   Measure between VCC and GND through the clip
   Expected: chip's internal resistance visible (typically 10 kΩ – 1 MΩ)
   Dangerous: <100 Ω — short on chip or clip
```

### 11.6 Verifying SPI Signal Quality

For intermittent issues not resolved by clip re-seating, measure signal integrity:

```
Procedure with oscilloscope (if available):
  - Connect scope probe to CLK line
  - Run flashrom detection: sudo flashrom -p ch341a_spi
  - Observe clock signal: should be clean square waves at ~1 MHz
  - Check for ringing, excessive noise, or absent signal
  - Repeat for MOSI during a read operation

Without oscilloscope (use multimeter AC voltage):
  - During flashrom read, briefly touch AC voltage probe to CLK
  - Should show non-zero AC reading (~0.5-1.5V AC typical for toggling line)
  - Zero reading = CLK not toggling = clock issue
```

### 11.7 Chapter Summary

The multimeter is the primary diagnostic tool for hardware-layer CH341A issues. Before every session: verify VCC is 3.3V ± 0.1V. Before every clip connection: test all 8 wires for continuity. When writes fail: measure WP# pin state. When short circuits are suspected: measure VCC-to-GND resistance before applying power.

---

## Chapter 12: Logic Analyzer Integration

### Purpose

A logic analyzer captures the actual SPI transactions occurring between the CH341A and the target chip. This is the definitive diagnostic tool when Flashrom reports errors that cannot be explained by clip, voltage, or module issues, and when you need to verify that commands and responses are being transmitted correctly.

### 12.1 When to Use a Logic Analyzer

- Flashrom detects a JEDEC ID but the chip is not in the database — capture the raw ID bytes
- Reads produce apparently valid data but analysis shows corruption — verify data integrity at the signal level
- Writing appears to succeed but verification fails — check if write enable commands are being sent
- Debugging SPI timing issues with unusual chips
- Documenting SPI traffic for reverse engineering or vulnerability research

### 12.2 PulseView (sigrok) Setup

PulseView is the GUI frontend for the sigrok logic analyzer framework. It supports a wide range of hardware including the popular 8-channel 24 MHz USB logic analyzers based on the Cypress FX2 chip (often sold as "Logic Analyzer 24 MHz" or "DSLogic" variants).

#### 12.2.1 Installation

```bash
sudo apt install pulseview sigrok sigrok-cli -y
pulseview --version
sigrok-cli --version
```

#### 12.2.2 Connecting the Logic Analyzer

For capturing SPI traffic from the CH341A to the target chip:

```
CH341A SOIC-8 Header  Logic Analyzer
=====================  =============
Pin 1 (CS#)        --> D0 (or CH0)
Pin 2 (MISO)       --> D1 (or CH1)
Pin 3 (WP#)        --> D2 (optional, for write protection monitoring)
Pin 4 (GND)        --> GND (must share ground reference)
Pin 5 (MOSI)       --> D3 (or CH2)
Pin 6 (CLK)        --> D4 (or CH3)
```

> **IMPORTANT:** The logic analyzer must share a common ground with the CH341A and target chip. Connect logic analyzer GND to CH341A GND.

#### 12.2.3 PulseView SPI Decoder Setup

1. Open PulseView
2. Click the device icon and select your logic analyzer
3. Set sample rate to at least 8× the SPI clock frequency (CH341A ~1 MHz → use 8 MHz minimum, 24 MHz preferred)
4. Start a capture while running: `sudo flashrom -p ch341a_spi --flash-name`
5. After capture, click **Add Protocol Decoder** → search for **SPI**
6. Configure SPI decoder:
   - `CLK` → channel connected to CLK line
   - `MOSI` → channel connected to MOSI line
   - `MISO` → channel connected to MISO line  
   - `CS#` → channel connected to CS# line
   - `CPOL` = 0, `CPHA` = 0 (SPI Mode 0, used by most NOR flash)
   - `Bit order` = MSB first

7. The decoder will annotate the capture with decoded bytes and commands

#### 12.2.4 Interpreting a JEDEC ID Capture

A JEDEC ID read sequence should look like:
```
CS# goes LOW
MOSI: 0x9F  (RDID command)
MISO: 0xEF  (Manufacturer ID — Winbond)
MISO: 0x40  (Memory type)
MISO: 0x18  (Capacity code — 128 Mbit = 16 MB)
CS# goes HIGH
```

If MISO shows only 0xFF during the response phase: no chip is responding (clip issue).
If MISO shows 0x00: stuck LOW — short circuit.
If MISO shows random non-repeating data: bus contention.

### 12.3 sigrok-cli for Command-Line Capture

```bash
# List available devices
sigrok-cli --scan

# Capture 1 million samples at 4 MHz from the fx2lafw device
# Channels D0-D4, save to file
sigrok-cli -d fx2lafw:conn=1.3 --config samplerate=4m     --channels D0,D1,D2,D3,D4     --samples 1000000     --output-file spi_capture.sr     --output-format srzip

# Decode SPI protocol from a saved capture
sigrok-cli -i spi_capture.sr     -P spi:clk=D4:mosi=D3:miso=D1:cs=D0     -A spi
```

### 12.4 Saleae Logic 2 (Optional)

Saleae Logic 2 is proprietary software for Saleae logic analyzers. If you have a Saleae device:

1. Download Logic 2 from the Saleae website
2. Connect Saleae analyzer channels to CH341A SPI lines (same mapping as PulseView)
3. Add SPI analyzer: `Analyzers` → `SPI` → configure CLK/MOSI/MISO/CS channels
4. Run `sudo flashrom -p ch341a_spi --flash-name` to trigger a transaction
5. Observe decoded SPI transactions in the Analyzers panel

> **NOTE:** Saleae Logic 2 is closed-source and requires internet activation. PulseView/sigrok is the fully open-source alternative that is sufficient for all CH341A debugging needs.

### 12.5 Chapter Summary

A logic analyzer provides definitive ground-truth data about what the CH341A is actually sending and receiving. PulseView with sigrok is the recommended open-source solution. Connect the analyzer to CS#, MISO, MOSI, CLK lines with a shared GND reference. The SPI protocol decoder annotates raw signals with decoded bytes, allowing direct verification of JEDEC ID responses, write enable commands, and data transfers.

---

## Chapter 13: Real-World Examples

### Purpose

This chapter presents four complete, real-world firmware extraction and modification scenarios. Each example covers the complete workflow from chip identification through analysis.

### 13.1 Example 1: TP-Link Router Firmware Extraction

**Target:** TP-Link TL-WR841N v14  
**Flash chip:** Winbond W25Q64FV (8 MB, SOIC-8)  
**Goal:** Extract firmware for security analysis

#### Step 1: Physical Setup

```bash
# Visual inspection shows chip marked "W25Q64FV" in SOIC-8 package
# Dot marking pin 1 visible on chip body
# Multimeter confirms 3.3V on CH341A output
# Target router powered off, AC adapter disconnected
```

#### Step 2: Detection and Extraction

```bash
mkdir ~/firmware_dumps/20260626_TPLINK_WR841N
cd ~/firmware_dumps/20260626_TPLINK_WR841N

lsusb | grep 1a86:5512
sudo flashrom -p ch341a_spi 2>&1 | tee detection.log
# Output: Found Winbond flash chip "W25Q64FV" (8192 kB, SPI) on ch341a_spi.

sudo flashrom -p ch341a_spi -r TPLINK_WR841N_read1.bin 2>&1 | tee read1.log
sudo flashrom -p ch341a_spi -r TPLINK_WR841N_read2.bin 2>&1 | tee read2.log

SHA1=$(sha256sum TPLINK_WR841N_read1.bin | cut -d' ' -f1)
SHA2=$(sha256sum TPLINK_WR841N_read2.bin | cut -d' ' -f1)
[ "$SHA1" = "$SHA2" ] && echo "MATCH" && cp TPLINK_WR841N_read1.bin TPLINK_WR841N_VERIFIED.bin
```

#### Step 3: Analysis

```bash
file TPLINK_WR841N_VERIFIED.bin
# Output: data

binwalk TPLINK_WR841N_VERIFIED.bin
# Typical output shows:
# 0x00000:  U-Boot header
# 0x00040:  LZMA compressed data (U-Boot itself)
# 0x20000:  TRX or TP-Link header
# 0x21000:  LZMA compressed kernel
# 0x200000: SquashFS filesystem

binwalk -e TPLINK_WR841N_VERIFIED.bin
cd _TPLINK_WR841N_VERIFIED.bin.extracted/

SQFS=$(ls *.squashfs 2>/dev/null | head -1)
unsquashfs -d /tmp/wr841n_fs "$SQFS"

cat /tmp/wr841n_fs/etc/passwd
# root:$1$salt$hash:0:0:root:/:/bin/sh

strings /tmp/wr841n_fs/usr/bin/httpd | grep -iE '(password|admin|user)'
```

### 13.2 Example 2: Laptop BIOS/UEFI Extraction

**Target:** Generic x86 laptop with Winbond W25Q128JV BIOS chip (16 MB)  
**Goal:** Extract BIOS firmware for backup before modification

#### Step 1: Locating the BIOS Chip

The BIOS chip is typically:
- Near the CPU or PCH (Platform Controller Hub) on the motherboard
- An 8-pin SOIC package
- Marked with manufacturer code (W25Q, MX25L, etc.)
- Often covered by a heat shield or located under the keyboard

#### Step 2: Connection Considerations

BIOS chips are frequently connected to the Intel ME (Management Engine), meaning multiple devices share the SPI bus. For in-circuit reads:
- Laptop must be **completely powered off** (remove AC adapter and battery)
- Hold the power button for 10 seconds after disconnecting power (discharges capacitors)
- Some BIOS chips are in shallow power-down and may respond inconsistently in-circuit

#### Step 3: Extraction

```bash
mkdir ~/firmware_dumps/$(date +%Y%m%d)_LAPTOP_BIOS
cd ~/firmware_dumps/$(date +%Y%m%d)_LAPTOP_BIOS

# Detection — chip may report as W25Q128.V, W25Q128FV, or W25Q128JV
sudo flashrom -p ch341a_spi 2>&1 | tee detection.log

# If multiple matches, specify:
sudo flashrom -p ch341a_spi -c "W25Q128JV" -r bios_read1.bin 2>&1 | tee read1.log
sudo flashrom -p ch341a_spi -c "W25Q128JV" -r bios_read2.bin 2>&1 | tee read2.log

# Compare and verify
sha256sum bios_read1.bin bios_read2.bin
cmp bios_read1.bin bios_read2.bin && echo "READS MATCH" || echo "DIFFER"
```

#### Step 4: BIOS Analysis

```bash
# Check for Intel ME region
binwalk bios_read1.bin
# Typical BIOS structure:
#   0x0000000:  Intel Flash Descriptor (magic: 5A A5 F0 0F)
#   0x0001000:  Intel ME region
#   0x0500000:  BIOS region

strings bios_read1.bin | grep -iE '(vendor|manufacturer|bios|uefi|version)' | sort -u
strings bios_read1.bin | grep -E 'www\.[a-z]+'  # Vendor URLs
```

### 13.3 Example 3: IP Camera Firmware Dump and Credential Extraction

**Target:** Generic Chinese IP camera with GD25Q128C (16 MB, SOIC-8)  
**Goal:** Extract hardcoded credentials and certificates

```bash
mkdir ~/firmware_dumps/$(date +%Y%m%d)_IPCAM
cd ~/firmware_dumps/$(date +%Y%m%d)_IPCAM

sudo flashrom -p ch341a_spi 2>&1 | tee detection.log
# Output: Found GigaDevice flash chip "GD25Q128C" (16384 kB, SPI)

sudo flashrom -p ch341a_spi -r ipcam_read1.bin 2>&1 | tee read1.log
sudo flashrom -p ch341a_spi -r ipcam_read2.bin 2>&1 | tee read2.log

SHA1=$(sha256sum ipcam_read1.bin | cut -d' ' -f1)
SHA2=$(sha256sum ipcam_read2.bin | cut -d' ' -f1)
[ "$SHA1" = "$SHA2" ] && cp ipcam_read1.bin ipcam_VERIFIED.bin

# Credential hunting
strings ipcam_VERIFIED.bin | grep -iE '(password|passwd|admin)' | sort -u
strings ipcam_VERIFIED.bin | grep -E '-----BEGIN'

# Extract filesystem
binwalk -e ipcam_VERIFIED.bin
cd _ipcam_VERIFIED.bin.extracted/
unsquashfs -d /tmp/ipcam_fs $(ls *.squashfs | head -1)

# Real credential extraction
cat /tmp/ipcam_fs/etc/passwd
cat /tmp/ipcam_fs/etc/shadow
find /tmp/ipcam_fs -name "*.pem" -o -name "*.crt" 2>/dev/null
```

### 13.4 Example 4: Bricked Router Recovery

**Target:** TP-Link router bricked by failed OTA update  
**Goal:** Restore original firmware directly to flash chip

```bash
# Step 1: Obtain original firmware for exact hardware version
# Download from manufacturer site or extract from another identical device

ORIG_FW="tl-wr841n-v14-original.bin"
sha256sum "$ORIG_FW"

# Step 2: Verify file is valid
file "$ORIG_FW"
binwalk "$ORIG_FW" | head -10

# Step 3: Connect CH341A to brick's flash chip
mkdir ~/firmware_dumps/$(date +%Y%m%d)_RECOVERY_WR841N
cd ~/firmware_dumps/$(date +%Y%m%d)_RECOVERY_WR841N

lsusb | grep 1a86:5512
sudo flashrom -p ch341a_spi 2>&1 | tee detection.log
# Confirm chip is detected

# Step 4: Backup current (bricked) flash content
sudo flashrom -p ch341a_spi -r bricked_backup.bin 2>&1 | tee bricked_backup.log
sha256sum bricked_backup.bin

# Step 5: Validate firmware size
CHIP_SIZE=$(sudo flashrom -p ch341a_spi --flash-size 2>/dev/null)
FW_SIZE=$(stat -c%s "$ORIG_FW")
echo "Chip: $CHIP_SIZE bytes | Firmware: $FW_SIZE bytes"

# Step 6: Write recovery firmware
sudo flashrom -p ch341a_spi -w "$ORIG_FW" 2>&1 | tee recovery_write.log
grep "VERIFIED\|FAILED" recovery_write.log

# Step 7: Independent post-write verify
sudo flashrom -p ch341a_spi -v "$ORIG_FW" 2>&1 | tee recovery_verify.log
grep "VERIFIED\|FAILED" recovery_verify.log

echo "Recovery complete — reconnect router power to test"
```

### 13.5 Chapter Summary

Real-world CH341A operations follow the same systematic workflow regardless of target: physical identification, double-read with SHA256 verification, analysis with file/strings/binwalk, and organized file storage. Router, BIOS, IP camera, and recovery scenarios are all handled with the same Flashrom core commands. The complexity lies in chip identification, filesystem extraction, and security analysis — not in the programming tool itself.

---

## Chapter 14: Best Practices for Professional Hardware Security Work

### Purpose

This chapter consolidates the professional practices that distinguish reliable, reproducible hardware security work from ad-hoc operations. These practices matter for professional engagements, lab reproducibility, and responsible disclosure.

### 14.1 Documentation Practices

#### 14.1.1 Engagement Log Template

Create a file for each target before touching hardware:

```markdown
# Hardware Security Assessment Log
Date: 2026-06-26
Target: TPLINK_WR841N
Assessor: [Name]
Authorization: [Reference to written authorization]

## Device Information
- Model: TP-Link TL-WR841N v14
- FCC ID: 2AXJ4WR841NV14
- Flash chip: Winbond W25Q64FV (8 MB SOIC-8)
- Firmware version (from device): 3.16.9

## Hardware Access
- Access method: In-circuit SOIC-8 clip
- CH341A voltage verified: 3.3V (measured at 3.31V)
- Clip pin 1 verification: Confirmed via dot on package

## Operations Performed
| Time  | Operation | SHA256 | Result |
|-------|-----------|--------|--------|
| 10:05 | Read 1    | abc... | OK     |
| 10:08 | Read 2    | abc... | MATCH  |

## Findings
[Security findings documented here]
```

#### 14.1.2 Always Log Everything

```bash
# Every flashrom command should use tee
sudo flashrom -p ch341a_spi -r backup.bin 2>&1 | tee "$(date +%Y%m%d_%H%M%S)_read.log"

# Log full system state at session start
{
    echo "=== Session Start: $(date) ==="
    echo "--- lsusb ---"
    lsusb
    echo "--- lsmod ---"
    lsmod | grep ch341
    echo "--- flashrom version ---"
    flashrom --version
} | tee session_$(date +%Y%m%d_%H%M%S).log
```

### 14.2 Backup Discipline

These rules are non-negotiable on professional engagements:

1. **Always read before writing.** No exceptions. No "quick writes" without a backup.
2. **Double-read every chip.** Two reads with matching SHA256 before any analysis or write.
3. **SHA256 everything immediately.** Run `sha256sum` on every file the moment it is created.
4. **Store backups in multiple locations.** Local working directory + a second location (encrypted USB, secure cloud storage, or another machine). The backup on the same machine you are working on does not count as a "second copy."
5. **Label backups with device info.** A file named `backup.bin` is useless. A file named `TPLINK_WR841N_W25Q64FV_20260626_VERIFIED.bin` is recoverable.

### 14.3 Voltage and Safety Protocol

These steps must happen in this order, every time:

```
1. MEASURE voltage before connecting any chip
2. Inspect clip for physical damage
3. Test clip continuity for all 8 wires
4. Identify pin 1 on target chip
5. Orient clip correctly
6. Connect clip with target POWERED OFF
7. THEN connect CH341A to USB
8. Run detection
9. After session: disconnect USB FIRST, then remove clip
```

### 14.4 Chain of Custody for Firmware Evidence

For legal or formal security assessments, firmware evidence may need to be defensible:

```bash
# Create an evidence record immediately after dump
EVIDENCE_DIR="evidence_$(date +%Y%m%d_%H%M%S)"
mkdir -p "$EVIDENCE_DIR"
cp *_VERIFIED.bin "$EVIDENCE_DIR/"

# Generate a formal evidence record
{
    echo "FIRMWARE EVIDENCE RECORD"
    echo "========================"
    echo "Date/Time: $(date -u) UTC"
    echo "Assessor: $(whoami)@$(hostname)"
    echo "Target: $TARGET"
    echo ""
    echo "SHA256 checksums:"
    sha256sum "$EVIDENCE_DIR/"*.bin
    echo ""
    echo "MD5 checksums (for legacy compatibility):"
    md5sum "$EVIDENCE_DIR/"*.bin
    echo ""
    echo "File details:"
    ls -la "$EVIDENCE_DIR/"*.bin
} | tee "$EVIDENCE_DIR/EVIDENCE_RECORD.txt"

# Create a tarball for archival
tar -czf "${EVIDENCE_DIR}.tar.gz" "$EVIDENCE_DIR/"
sha256sum "${EVIDENCE_DIR}.tar.gz" > "${EVIDENCE_DIR}.tar.gz.sha256"
```

### 14.5 Responsible Disclosure Workflow

When you find security vulnerabilities in firmware:

1. **Document the finding** with exact evidence (byte offsets, commands used, extracted data)
2. **Do not publish** any credentials, private keys, or proof-of-concept exploits until the vendor has been notified and a fix is available
3. **Contact the vendor** via their security disclosure channel (security@vendor.com, HackerOne, BugCrowd, etc.)
4. **Provide a reasonable timeline:** 90 days is the industry standard (Google Project Zero policy). Communicate this timeline explicitly.
5. **Coordinate disclosure:** Work with the vendor on an agreed disclosure date
6. **Publish responsibly:** After the fix is available, publish a CVE-referenced disclosure

### 14.6 Chip Identification Reference Checklist

Before every session, confirm:

```bash
# 1. Identify chip from PCB markings
# (photograph the chip marking for records)

# 2. Look up the datasheet
# Search: "{CHIP_PART_NUMBER} datasheet pdf"
# Confirm: VCC voltage, absolute maximum ratings, package

# 3. Verify chip is in Flashrom database
sudo flashrom -p ch341a_spi -L | grep -i "{CHIP_PART_NUMBER}"

# 4. If not found, check latest Flashrom source
# git log --oneline --all -- flashchips.c | head -5

# 5. Document JEDEC ID when detected
sudo flashrom -p ch341a_spi -VVV 2>&1 | grep "id1"
```

### 14.7 Lab Environment Setup

Recommended lab setup for professional CH341A work:

```
Physical:
  - ESD-safe mat on work surface
  - Anti-static wrist strap grounded to mat
  - Good lighting (LED work light or daylight bulb)
  - 10x loupe or USB microscope for chip marking reading
  - SOIC-8 clip (multiple spares)
  - SOIC-16 clip
  - Multimeter
  - CH341A programmer (with 3.3V modification applied and verified)

Software (Ubuntu 24.04 LTS):
  - Flashrom (verified working)
  - binwalk 2.3.x
  - i2c-tools
  - PulseView/sigrok (if logic analyzer available)
  - xxd, strings, hexdump
  - openssl
  - squashfs-tools

Organizational:
  - ~/firmware_dumps/ with date-named subdirectories
  - Template engagement log
  - SHA256SUMS file for every session
```

### 14.8 Chapter Summary

Professional CH341A work requires: structured documentation (engagement log, SHA256 for every file), non-negotiable backup discipline (double-read before analysis, backup before write), a systematic safety protocol (voltage check → clip continuity → pin 1 → connect), and responsible disclosure workflows for discovered vulnerabilities. An unorganized dump from a poorly documented session has limited evidentiary and reproducibility value.

---

## Chapter 15: Flashrom Error Reference

### Purpose

This chapter provides a comprehensive reference for every Flashrom error message that may appear during CH341A operations, with explanation and resolution for each.

### 15.1 Initialization Errors

```
Error: Programmer initialization failed.
```
**Cause:** Flashrom cannot access the CH341A via libusb.  
**Resolution:**
- Verify CH341A is connected: `lsusb | grep 1a86:5512`
- Unload ch341 module: `sudo modprobe -r ch341`
- Use `sudo` or configure udev rules (Section 3.5)

---

```
Error: Could not find USB device with VID 1a86 PID 5512.
```
**Cause:** CH341A not detected as programmer.  
**Resolution:** Verify USB connection. Check if device appears as `1a86:7523` (serial mode). If so, unload `ch341` module and replug.

### 15.2 Detection Errors

```
No EEPROM/flash device found.
```
**Cause:** Chip not responding to JEDEC ID probe.  
**Resolution:** Per troubleshooting decision tree in Chapter 10.

---

```
Multiple flash chip definitions match the detected chip(s): ...
Please specify which chip definition to use with the -c <chipname> option.
```
**Cause:** Two or more chips in Flashrom's database have the same JEDEC ID.  
**Resolution:** Use `-c "EXACT_CHIP_NAME"` where the name matches one of the listed candidates. Use physical chip markings to determine the correct one.

---

```
Warning: Found a flash chip which is not natively supported by the currently used programmer.
```
**Cause:** Chip is in database but has flags indicating it needs specific programmer capabilities that ch341a_spi may not provide.  
**Resolution:** This is a warning, not an error. Proceed with caution. Test read first to verify data integrity before writing.

### 15.3 Read Errors

```
Error: Timeout waiting for read to complete.
```
**Cause:** Chip did not complete a read operation within the expected time.  
**Resolution:** Verify clip contact. Check for intermittent connections. Try USB 2.0 port.

---

```
Read appeared to succeed, but -v verification shows differences.
```
**Cause:** Data read from chip does not match on re-read (unstable connection) or chip is damaged.  
**Resolution:** Per Chapter 10 inconsistent reads troubleshooting.

### 15.4 Write Errors

```
Verifying flash... FAILED at 0xXXXXXXXX! Expected=0xXX, Got=0xFF
Your flash chip is in an unknown state.
```
**Cause:** Data was erased (sectors are 0xFF) but program operation failed.  
**Resolution:** Likely write protection (WP# LOW) or lost clip contact. Restore from backup.

---

```
Verifying flash... FAILED at 0xXXXXXXXX! Expected=0xXX, Got=0xXX (different non-FF value)
```
**Cause:** Read-back data does not match expected — write succeeded partially or verification has a connection issue.  
**Resolution:** Re-seat clip; retry verification: `sudo flashrom -p ch341a_spi -v firmware.bin`

---

```
Error: Block erase failed at 0xXXXXXX
```
**Cause:** Erase command was sent but the chip did not erase the block.  
**Resolution:** Most commonly write protection. Check WP# pin state.

---

```
Error: chip.write_256 failed
```
**Cause:** Page program command failed.  
**Resolution:** Check WP# pin; verify clip contact is stable; try a fresh erase followed by write.

### 15.5 Size Mismatch Errors

```
Error: Image size (XXXXXX bytes) doesn't match the flash chip's size (YYYYYY bytes).
```
**Cause:** The firmware file size does not equal the detected chip size.  
**Resolution:**
- If file is smaller: pad to chip size (Section 8.2)
- If file is larger: wrong chip detected or wrong firmware — use `-c` to verify chip identification
- Use layout files for partial region writes

### 15.6 Layout File Errors

```
Error: Region "REGIONNAME" not found in layout file.
```
**Cause:** The region name specified with `-i` does not exist in the layout file.  
**Resolution:** Check spelling; run `cat layout.file` to verify region names.

---

```
Error: Layout file "file.layout" could not be opened.
```
**Cause:** Layout file path is wrong or file does not exist.  
**Resolution:** Verify path; use absolute path: `-l /full/path/to/layout.file`

### 15.7 Complete Error Quick-Reference

| Error Fragment | Likely Cause | Quick Fix |
|----------------|-------------|-----------|
| `Programmer initialization failed` | Module conflict or permissions | `modprobe -r ch341`; use `sudo` |
| `No EEPROM/flash device found` | Chip not responding | Chapter 10 decision tree |
| `Multiple flash chip definitions match` | Ambiguous JEDEC ID | Use `-c` to specify chip |
| `FAILED at 0xXXX, Got=0xFF` | Write protection, erase succeeded | Check WP# pin |
| `FAILED at 0xXXX, Got=non-FF` | Partial write, verification failure | Re-seat clip; retry verify |
| `Image size doesn't match` | Wrong firmware or chip | Verify chip size; pad or use layout |
| `Region not found in layout` | Typo in region name | Check `-i` name vs layout file |
| `Block erase failed` | Write protection active | Hardware WP# override needed |
| `Timeout waiting for read` | Intermittent connection | Check USB port and clip |

---

## Chapter 16: Quick Reference

### 16.1 Essential Commands Cheat Sheet

```bash
# ── Detection ──────────────────────────────────────────────────────
sudo flashrom -p ch341a_spi                      # Detect chip
sudo flashrom -p ch341a_spi --flash-name          # Print chip name
sudo flashrom -p ch341a_spi --flash-size          # Print chip size (bytes)
sudo flashrom -p ch341a_spi -c "W25Q128.V"        # Specify chip explicitly
sudo flashrom -p ch341a_spi -L                    # List all supported chips
sudo flashrom -p ch341a_spi -L | grep -i winbond  # Filter chip list

# ── Reading ────────────────────────────────────────────────────────
sudo flashrom -p ch341a_spi -r backup.bin                       # Read chip
sudo flashrom -p ch341a_spi -c "W25Q128.V" -r backup.bin        # With chip spec
sudo flashrom -p ch341a_spi -l flash.layout -i region -r r.bin  # Read region

# ── Verification ───────────────────────────────────────────────────
sudo flashrom -p ch341a_spi -v firmware.bin         # Verify vs file

# ── Writing ────────────────────────────────────────────────────────
sudo flashrom -p ch341a_spi -w new_firmware.bin                  # Write full chip
sudo flashrom -p ch341a_spi -l flash.layout -i firmware -w f.bin # Write region

# ── Erasing ────────────────────────────────────────────────────────
sudo flashrom -p ch341a_spi -E                      # Erase entire chip

# ── Verbose / Debug ────────────────────────────────────────────────
sudo flashrom -p ch341a_spi -V                      # Verbose
sudo flashrom -p ch341a_spi -VVV 2>&1 | tee diag.log  # Max verbose + log

# ── USB & System ───────────────────────────────────────────────────
lsusb | grep 1a86                                   # Check CH341A detected
lsmod | grep ch341                                  # Check module status
sudo modprobe -r ch341                              # Unload ch341 module
```

### 16.2 Analysis Quick Reference

```bash
# ── File identification ────────────────────────────────────────────
file firmware.bin
xxd firmware.bin | head -16
hexdump -C firmware.bin | head -16

# ── String extraction ──────────────────────────────────────────────
strings -n 8 firmware.bin
strings -n 8 firmware.bin | grep -iE '(password|admin|key|BEGIN)'
strings -o firmware.bin | grep '-----BEGIN'

# ── binwalk ────────────────────────────────────────────────────────
binwalk firmware.bin              # Signature scan
binwalk -E firmware.bin           # Entropy analysis
binwalk -e firmware.bin           # Extract components
binwalk -Me firmware.bin          # Recursive extraction

# ── Hash verification ──────────────────────────────────────────────
sha256sum firmware.bin
sha256sum -c SHA256SUMS.txt
cmp read1.bin read2.bin && echo "MATCH" || echo "DIFFER"
```

### 16.3 Common Chip Quick Reference

| Chip | Size | JEDEC ID | Flashrom Name |
|------|------|----------|---------------|
| W25Q16JV | 2 MB | EF 40 15 | W25Q16JV |
| W25Q32FV | 4 MB | EF 40 16 | W25Q32FV |
| W25Q64FV | 8 MB | EF 40 17 | W25Q64FV |
| W25Q128FV | 16 MB | EF 40 18 | W25Q128.V |
| W25Q128JV | 16 MB | EF 70 18 | W25Q128JV |
| W25Q256FV | 32 MB | EF 40 19 | W25Q256FV |
| MX25L3206E | 4 MB | C2 20 16 | MX25L3206E |
| MX25L6406E | 8 MB | C2 20 17 | MX25L6406E |
| MX25L12835F | 16 MB | C2 20 18 | MX25L12835F/MX25L12836F |
| GD25Q64C | 8 MB | C8 40 17 | GD25Q64C |
| GD25Q128C | 16 MB | C8 40 18 | GD25Q128C |

### 16.4 JEDEC Manufacturer ID Reference

| Hex | Manufacturer | Common Families |
|-----|-------------|----------------|
| 0xEF | Winbond | W25Qxxx |
| 0xC2 | Macronix | MX25Lxxx |
| 0xC8 | GigaDevice | GD25Qxxx |
| 0x9D | ISSI | IS25LPxxx |
| 0x20 | Micron/ST | N25Qxxx, M25Pxxx |
| 0x1C | EON | EN25Qxxx |
| 0x0B | XMC | XM25QHxxx |
| 0xBF | Microchip/SST | SST25VFxxx |
| 0x01 | Spansion | S25FLxxx |

### 16.5 Pre-Session Safety Checklist

```
[ ] CH341A voltage measured — confirmed 3.3V ± 0.1V
[ ] Clip inspected — no bent or missing spring pins
[ ] All 8 clip wires tested for continuity
[ ] ESD wrist strap worn and grounded
[ ] Target board completely powered off (mains + battery)
[ ] Pin 1 identified on target chip
[ ] Clip oriented correctly (pin 1 to pin 1)
[ ] Working directory created with date and target name
[ ] lsusb confirms 1a86:5512
[ ] lsmod shows ch341 NOT loaded
```

---

## Appendix A: Converting This Handbook to PDF and DOCX

### A.1 Installing Pandoc

```bash
sudo apt install pandoc -y
pandoc --version
```

For high-quality PDF output, install a LaTeX engine:

```bash
sudo apt install texlive-xetex texlive-fonts-recommended -y
# or for full LaTeX suite (larger download):
sudo apt install texlive-full -y
```

### A.2 Converting to PDF

#### A.2.1 Basic PDF Conversion

```bash
pandoc CH341A_Ubuntu_Handbook.md \
    --pdf-engine=xelatex \
    -o CH341A_Ubuntu_Handbook.pdf
```

#### A.2.2 Professional PDF with Custom Styling

Create a metadata file for document properties:

```bash
cat > handbook_metadata.yaml << 'EOF'
---
title: "CH341A USB Programmer: The Complete Ubuntu Handbook"
subtitle: "Firmware Extraction, Analysis, and Recovery for Hardware Security Engineers"
author: "Hardware Security Reference Series"
date: "2026"
documentclass: book
fontsize: 11pt
geometry: margin=2.5cm
colorlinks: true
linkcolor: blue
urlcolor: blue
toc: true
toc-depth: 3
numbersections: true
papersize: a4
---
EOF
```

```bash
pandoc CH341A_Ubuntu_Handbook.md \
    --pdf-engine=xelatex \
    --metadata-file=handbook_metadata.yaml \
    --highlight-style=tango \
    --toc \
    --toc-depth=3 \
    --number-sections \
    -V geometry:margin=2.5cm \
    -V fontsize=11pt \
    -V colorlinks=true \
    -V linkcolor=blue \
    -o CH341A_Ubuntu_Handbook.pdf
```

#### A.2.3 With Custom Fonts

```bash
pandoc CH341A_Ubuntu_Handbook.md \
    --pdf-engine=xelatex \
    -V mainfont="DejaVu Serif" \
    -V monofont="DejaVu Sans Mono" \
    -V sansfont="DejaVu Sans" \
    --toc \
    -o CH341A_Ubuntu_Handbook.pdf
```

Check available fonts:
```bash
fc-list | grep -i "dejavu\|liberation\|ubuntu"
```

### A.3 Converting to DOCX (Microsoft Word)

```bash
# Basic DOCX conversion
pandoc CH341A_Ubuntu_Handbook.md \
    -o CH341A_Ubuntu_Handbook.docx
```

#### A.3.1 With Custom Reference Document

Create a Word document with your preferred styling (fonts, heading styles, etc.) and use it as a reference template:

```bash
# First generate a default reference document to customize
pandoc --print-default-data-file reference.docx > reference_template.docx

# Edit reference_template.docx in LibreOffice or Word to set styles
# Then use it for conversion:
pandoc CH341A_Ubuntu_Handbook.md \
    --reference-doc=reference_template.docx \
    -o CH341A_Ubuntu_Handbook.docx
```

### A.4 Converting to EPUB

```bash
pandoc CH341A_Ubuntu_Handbook.md \
    --epub-cover-image=cover.png \
    -o CH341A_Ubuntu_Handbook.epub
```

### A.5 Converting to HTML

```bash
pandoc CH341A_Ubuntu_Handbook.md \
    --standalone \
    --toc \
    --highlight-style=github \
    -o CH341A_Ubuntu_Handbook.html
```

### A.6 Troubleshooting Pandoc Conversions

| Error | Cause | Fix |
|-------|-------|-----|
| `xelatex not found` | LaTeX not installed | `sudo apt install texlive-xetex` |
| Missing unicode characters in PDF | Font doesn't support them | Use `-V mainfont="DejaVu Serif"` |
| `! Package inputenc Error` | Encoding issue | Use `--pdf-engine=xelatex` (not pdflatex) |
| Code blocks overflow page | Long lines in code | Add `-V geometry:margin=1.5cm` or wrap long lines |
| Tables too wide | Wide markdown tables | Use `-V geometry:margin=1cm` or simplify tables |

---

## Appendix B: Flashrom Command Verification Notes

This appendix documents the verification status of all Flashrom commands used in this handbook.

### B.1 Verification Methodology

Commands in this handbook were verified against:
- Ubuntu 24.04 LTS (Flashrom 1.2 from apt repository)
- Flashrom source at https://review.coreboot.org/flashrom.git (v1.4 release tag)
- Official Flashrom documentation at https://flashrom.org

### B.2 Commands Verified

| Command | Verified | Notes |
|---------|----------|-------|
| `flashrom -p ch341a_spi` | ✅ | Detection |
| `flashrom -p ch341a_spi -r file.bin` | ✅ | Read |
| `flashrom -p ch341a_spi -w file.bin` | ✅ | Write |
| `flashrom -p ch341a_spi -v file.bin` | ✅ | Verify |
| `flashrom -p ch341a_spi -E` | ✅ | Erase |
| `flashrom -p ch341a_spi -c "CHIP"` | ✅ | Chip specify |
| `flashrom -p ch341a_spi -L` | ✅ | List chips |
| `flashrom -p ch341a_spi --flash-name` | ✅ | Chip name |
| `flashrom -p ch341a_spi --flash-size` | ✅ | Chip size |
| `flashrom -p ch341a_spi -V/-VV/-VVV` | ✅ | Verbose |
| `flashrom -p ch341a_spi -l layout -i region` | ✅ | Layout |
| `flashrom -p ch341a_spi --noverify` | ✅ | Skip verify |
| `flashrom -p ch341a_spi:spispeed=N` | ❌ | **NOT supported** for ch341a_spi backend |

### B.3 Known ch341a_spi Backend Limitations

The following are documented limitations of the `ch341a_spi` programmer backend as of Flashrom 1.6:

1. **SPI speed is not user-configurable.** The `spispeed=` programmer option is not accepted by `ch341a_spi`. Do not attempt to use it.

2. **JEDEC ID probe is automatic.** There is no flag to manually specify a JEDEC ID to force a particular chip detection.

3. **Dual/Quad SPI is not supported.** The CH341A only implements standard single-bit SPI (MOSI/MISO).

4. **Direct status register read/write is not exposed via CLI.** Flashrom manages this internally.

5. **IFD (Intel Flash Descriptor) mode** with `ch341a_spi` is not officially tested. Use with the `internal` programmer for IFD operations on supported systems.

### B.4 Flashrom Version Compatibility

| Feature | v1.2 (Ubuntu 24.04 apt) | v1.4 | v1.6 |
|---------|------------------------|------|------|
| ch341a_spi backend | ✅ | ✅ | ✅ |
| `--flash-name` | ✅ | ✅ | ✅ |
| `--flash-size` | ✅ | ✅ | ✅ |
| `--noverify-all` | ✅ | ✅ | ✅ |
| Extended chip database | Older | Newer | Latest |

For maximum chip database coverage, compile from source (Section 3.2.3).

---

## Appendix C: Supported Chip Reference

### C.1 Most Common Chips in Security Research

The following chips are the most frequently encountered in router, camera, and BIOS firmware extraction work. All are confirmed supported in Flashrom's ch341a_spi backend.

#### Winbond W25Q Series (Most Common)

| Part Number | Capacity | JEDEC ID | Package | Applications |
|-------------|----------|----------|---------|-------------|
| W25Q16JV | 2 MB | EF 40 15 | SOIC-8 | Budget routers, MCUs |
| W25Q32FV | 4 MB | EF 40 16 | SOIC-8 | Budget routers |
| W25Q64FV | 8 MB | EF 40 17 | SOIC-8 | Mid-range routers, cameras |
| W25Q64JV | 8 MB | EF 40 17 | SOIC-8 | Mid-range routers |
| W25Q128FV | 16 MB | EF 40 18 | SOIC-8 | BIOS, high-end routers |
| W25Q128JV | 16 MB | EF 70 18 | SOIC-8 | Modern BIOS chips |
| W25Q256FV | 32 MB | EF 40 19 | SOIC-16 | Large BIOS, NAS devices |

#### Macronix MX25L Series

| Part Number | Capacity | JEDEC ID | Package | Applications |
|-------------|----------|----------|---------|-------------|
| MX25L3206E | 4 MB | C2 20 16 | SOIC-8 | Older routers |
| MX25L6406E | 8 MB | C2 20 17 | SOIC-8 | Routers, cameras |
| MX25L12835F | 16 MB | C2 20 18 | SOIC-8 | Laptop BIOS |
| MX25L25635F | 32 MB | C2 20 19 | SOIC-16 | Large BIOS |

#### GigaDevice GD25Q Series

| Part Number | Capacity | JEDEC ID | Package | Applications |
|-------------|----------|----------|---------|-------------|
| GD25Q32C | 4 MB | C8 40 16 | SOIC-8 | Budget IoT |
| GD25Q64C | 8 MB | C8 40 17 | SOIC-8 | Routers |
| GD25Q128C | 16 MB | C8 40 18 | SOIC-8 | Cameras, routers |

### C.2 Checking If a Chip Is Supported

```bash
# Search by part number
sudo flashrom -p ch341a_spi -L | grep -i "W25Q128"

# Search by manufacturer
sudo flashrom -p ch341a_spi -L | grep -i "gigadevice"

# Get full chip count
sudo flashrom -p ch341a_spi -L | wc -l
```

### C.3 Chips NOT Supported by CH341A/Flashrom

| Chip Type | Reason | Alternative |
|-----------|--------|-------------|
| SPI NAND flash | Different command set, ECC required | Dedicated NAND programmer |
| 1.8V SPI NOR (W25Q...W suffix) | Voltage too high | 1.8V programmer |
| eMMC | Completely different interface | ISP adapter + dedicated tool |
| UFS | Not SPI-based | Specialized hardware |
| JTAG-only devices | Different protocol | OpenOCD + JTAG adapter |

---

*End of CH341A Ubuntu Handbook v1.0*

*For the latest version, check the project source. For errata and corrections, consult official Flashrom documentation at https://flashrom.org*

