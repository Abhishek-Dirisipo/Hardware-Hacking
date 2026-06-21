# Hardware Hacking Handbook - Abbreviations Cheat Sheet

A complete, chapter-wise reference guide of abbreviations, acronyms, and technical terms used in the book.

## Table of Contents
- [Copyright](#copyright)
- [Dedication](#dedication)
- [About the Authors](#about-the-authors)
- [Foreword](#foreword)
- [Acknowledgments](#acknowledgments)
- [Introduction](#introduction)
- [Chapter 1: Dental Hygiene: Introduction to Embedded Security](#chapter-1-dental-hygiene-introduction-to-embedded-security)
- [Chapter 2: Reaching Out, Touching Me, Touching You: Hardware Peripheral Interfaces](#chapter-2-reaching-out-touching-me-touching-you-hardware-peripheral-interfaces)
- [Chapter 3: Casing the Joint: Identifying Components and Gathering Information](#chapter-3-casing-the-joint-identifying-components-and-gathering-information)
- [Chapter 4: Bull in a Porcelain Shop: Introducing Fault Injection](#chapter-4-bull-in-a-porcelain-shop-introducing-fault-injection)
- [Chapter 5: Don’t Lick the Probe: How to Inject Faults](#chapter-5-don’t-lick-the-probe-how-to-inject-faults)
- [Chapter 6: Bench Time: Fault Injection Lab](#chapter-6-bench-time-fault-injection-lab)
- [Chapter 7: X Marks the Spot: Trezor One Wallet Memory Dump](#chapter-7-x-marks-the-spot-trezor-one-wallet-memory-dump)
- [Chapter 8: I’ve Got the Power: Introduction to Power Analysis](#chapter-8-i’ve-got-the-power-introduction-to-power-analysis)
- [Chapter 9: Bench Time: Simple Power Analysis](#chapter-9-bench-time-simple-power-analysis)
- [Chapter 10: Splitting the Difference: Differential Power Analysis](#chapter-10-splitting-the-difference-differential-power-analysis)
- [Chapter 11: Gettin’ Nerdy with It: Advanced Power Analysis](#chapter-11-gettin’-nerdy-with-it-advanced-power-analysis)
- [Chapter 12: Bench Time: Differential Power Analysis](#chapter-12-bench-time-differential-power-analysis)
- [Chapter 13: No Kiddin’: Real-Life Examples](#chapter-13-no-kiddin’-real-life-examples)
- [Chapter 14: Think of the Children: Countermeasures, Certifications, and Goodbytes](#chapter-14-think-of-the-children-countermeasures-certifications-and-goodbytes)
- [Appendix A: Maxing Out Your Credit Card: Setting Up a Test Lab](#appendix-a-maxing-out-your-credit-card-setting-up-a-test-lab)
- [Appendix B: All Your Base Are Belong to Us: Popular Pinouts](#appendix-b-all-your-base-are-belong-to-us-popular-pinouts)
- [Index](#index)

## Copyright

| Abbreviation | Definition / Full Form |
|---|---|
| **CA** | Certificate Authority |
| **DDC** | Display Data Channel (monitor identification over I2C) |
| **ISBN** | International Standard Book Number |
| **ISBN-13** | International Standard Book Number - 13 digit format |
| **LC** | Inductor-Capacitor (resonant circuit / filter) |
| **LCC** | Leadless Chip Carrier |
| **LCCN** | Library of Congress Control Number |
| **LCSH** | Library of Congress Subject Headings |
| **TK7895** | Texas Instruments TK7895 - MOSFET used in switching circuits |

## Dedication

| Abbreviation | Definition / Full Form |
|---|---|
| **us** | Microsecond (10⁻⁶ s) |

## About the Authors

| Abbreviation | Definition / Full Form |
|---|---|
| **ChipWhisperer** | NewAE Technology open-source side-channel analysis and fault injection tool |
| **CTO** | Chief Technology Officer |
| **IMEC** | Interuniversity Microelectronics Centre (Belgian research institute) |
| **NewAE** | NewAE Technology Inc. - hardware security research tools |
| **PhD** | Doctor of Philosophy (doctoral academic degree) |

## Foreword

| Abbreviation | Definition / Full Form |
|---|---|
| **ASCII** | American Standard Code for Information Interchange (7-bit text encoding) |
| **us** | Microsecond (10⁻⁶ s) |

## Acknowledgments

| Abbreviation | Definition / Full Form |
|---|---|
| **NewAE** | NewAE Technology Inc. - hardware security research tools |
| **us** | Microsecond (10⁻⁶ s) |

## Introduction

| Abbreviation | Definition / Full Form |
|---|---|
| **ASIC** | Application-Specific Integrated Circuit |
| **CPU** | Central Processing Unit |
| **ECU** | Electronic Control Unit (automotive computer module) |
| **FreeRTOS** | FreeRTOS - open-source real-time operating system for MCUs |
| **MRI** | PC inside a magnetic resonance imaging |
| **OS** | Operating System |
| **PC** | Program Counter |
| **PCB** | Printed Circuit Board |
| **PLC** | Programmable Logic Controller |
| **SPA** | Simple Power Analysis |
| **TV** | Television |

## Chapter 1: Dental Hygiene: Introduction to Embedded Security

| Abbreviation | Definition / Full Form |
|---|---|
| **AES** | Advanced Encryption Standard (NIST FIPS 197, symmetric block cipher) |
| **AES-128** | AES with 128-bit key length |
| **APD** | Avalanche Photodiode (high-sensitivity light detector) |
| **ARM** | Advanced RISC Machines (processor architecture) |
| **BootStomp** | BootStomp - automated bootloader security analysis tool (research) |
| **BY** | Byte (sometimes used in part numbers) |
| **CBC** | Cipher Block Chaining (symmetric cipher mode) |
| **CC** | Common Criteria (ISO/IEC 15408 - IT security evaluation standard) |
| **CCD** | Charge-Coupled Device (image sensor) |
| **CEO** | Chief Executive Officer |
| **CLKSCREW** | CLKscrew - voltage/frequency fault injection attack exploiting DVFS |
| **CMOS** | Complementary Metal-Oxide-Semiconductor |
| **COTS** | Commercial Off-The-Shelf |
| **CPU** | Central Processing Unit |
| **CRC** | Cyclic Redundancy Check (error detection code) |
| **CSME** | Intel Converged Security and Management Engine |
| **CVSS** | Common Vulnerability Scoring System |
| **CWSS** | Common Weakness Scoring System |
| **DC** | Direct Current |
| **DM** | Data Minus (USB D- differential data line) |
| **DPA** | Differential Power Analysis |
| **DRAM** | Dynamic Random Access Memory |
| **DRM** | Digital Rights Management |
| **ECB** | Electronic Code Book (insecure symmetric cipher mode) |
| **EEPROM** | Electrically Erasable Programmable Read-Only Memory |
| **EM** | Electromagnetic |
| **EMVCo** | EMVco - organization managing EMV chip card standards |
| **EPS** | Endorsement Primary Seed (TPM 2.0 key derivation seed) |
| **FIB** | Focused Ion Beam (nanoscale milling/imaging tool) |
| **FIPS** | Federal Information Processing Standard (NIST standard series) |
| **FreeRTOS** | FreeRTOS - open-source real-time operating system for MCUs |
| **GPS** | Global Positioning System |
| **GPU** | Graphics Processing Unit |
| **HIPAA** | Health Insurance Portability and Accountability Act |
| **I2C** | Inter-Integrated Circuit |
| **IC** | Integrated Circuit |
| **ICE** | In-Circuit Emulator |
| **iOS** | iPhone Operating System (Apple mobile OS) |
| **IoT** | Internet of Things |
| **IP** | Internet Protocol / Intellectual Property |
| **IR** | Infrared |
| **JavaScript** | JavaScript - high-level scripting programming language for web/general use |
| **JIL** | Joint Interpretation Library (CC community guidelines) |
| **JTAG** | Joint Test Action Group (IEEE 1149.1 boundary scan standard) |
| **NSA** | National Security Agency (US intelligence / cybersecurity agency) |
| **OEM** | Original Equipment Manufacturer |
| **OS** | Operating System |
| **OTP** | One-Time Programmable (memory that can only be written once) |
| **PC** | Program Counter |
| **PCB** | Printed Circuit Board |
| **PCI** | Peripheral Component Interconnect |
| **PCIe** | Peripheral Component Interconnect Express |
| **PII** | Personally Identifiable Information |
| **PoC** | Proof of Concept |
| **RAM** | Random Access Memory |
| **REE** | Rich Execution Environment (normal OS alongside TEE) |
| **ROM** | Read-Only Memory |
| **RQ-170** | Lockheed Martin RQ-170 Sentinel - stealth reconnaissance drone |
| **RSA** | Rivest–Shamir–Adleman (public-key asymmetric encryption) |
| **SCADA** | Supervisory Control and Data Acquisition |
| **SD** | Secure Digital (memory card standard) |
| **SEM** | Scanning Electron Microscope |
| **SEU** | Single Event Upset (bit-flip caused by radiation/particles) |
| **SoC** | System on Chip |
| **SOG-IS** | Senior Officials Group for the Security of Information Systems (EU CC scheme) |
| **SPI** | Serial Peripheral Interface |
| **SRAM** | Static Random Access Memory |
| **SWD** | Serial Wire Debug (ARM 2-pin debug interface) |
| **TA** | Trusted Application (runs inside TEE) |
| **TAs** | Trusted Applications (run inside TEE) |
| **TCP** | Transmission Control Protocol |
| **TEE** | Trusted Execution Environment |
| **TPM** | Trusted Platform Module (TCG standard secure crypto chip) |
| **TV** | Television |
| **us** | Microsecond (10⁻⁶ s) |
| **US** | United States |
| **USA** | United States of America |
| **USB** | Universal Serial Bus |
| **VxWorks** | Wind River VxWorks - commercial real-time operating system |
| **XOR** | Exclusive OR (fundamental bitwise operation in cryptography) |

## Chapter 2: Reaching Out, Touching Me, Touching You: Hardware Peripheral Interfaces

| Abbreviation | Definition / Full Form |
|---|---|
| **AC** | Alternating Current |
| **ADC** | Analog-to-Digital Converter |
| **ARM** | Advanced RISC Machines (processor architecture) |
| **ATA** | Advanced Technology Attachment (storage interface) |
| **AVR** | Alf and Vegard's RISC processor (Atmel/Microchip 8-bit MCU) |
| **BGA** | Ball Grid Array |
| **BIOS** | Basic Input/Output System |
| **BSDL** | Boundary Scan Description Language (IEEE 1149.1) |
| **BY** | Byte (sometimes used in part numbers) |
| **CAN** | Controller Area Network (automotive/industrial serial bus) |
| **CC** | Common Criteria (ISO/IEC 15408 - IT security evaluation standard) |
| **CIPO** | Controller In Peripheral Out (modern SPI terminology) |
| **CMOS** | Complementary Metal-Oxide-Semiconductor |
| **COPI** | Controller Out Peripheral In (modern SPI terminology) |
| **CPU** | Central Processing Unit |
| **CS** | Chip Select |
| **DC** | Direct Current |
| **DDR** | Double Data Rate (SDRAM clocking both edges) |
| **DDR4** | Double Data Rate 4 (4th generation DDR SDRAM) |
| **DI** | Data Input |
| **DR** | and the data register |
| **DRAM** | Dynamic Random Access Memory |
| **DVI** | Digital Visual Interface |
| **EEPROM** | Electrically Erasable Programmable Read-Only Memory |
| **EFI** | Extensible Firmware Interface (predecessor to UEFI) |
| **EM** | Electromagnetic |
| **eMMC** | embedded MultiMediaCard |
| **EN1** | Enable 1 (first enable signal / chip enable pin) |
| **EN2** | Enable 2 (second enable signal) |
| **EXTEST** | External Test (JTAG boundary scan instruction) |
| **FD** | updated version of CAN called CAN flexible data-rate |
| **GDB** | the GNU Debugger |
| **GHz** | Gigahertz (10⁹ cycles per second) |
| **GND** | Ground (electrical reference / 0V) |
| **GPIO** | General Purpose Input/Output |
| **GPS** | Global Positioning System |
| **GT** | Gigabit Transceiver |
| **GUI** | Graphical User Interface |
| **HDMI** | High-Definition Multimedia Interface |
| **HID** | An example is the USB Human Interface Device |
| **I2C** | Inter-Integrated Circuit |
| **IC** | Integrated Circuit |
| **IEEE** | Institute of Electrical and Electronics Engineers |
| **IIC** | Inter-Integrated Circuit (alternate name for I2C) |
| **IR** | Infrared |
| **JTAG** | Joint Test Action Group (IEEE 1149.1 boundary scan standard) |
| **JTAGSEL** | JTAG Select (pin to switch device into JTAG mode) |
| **kHz** | Kilohertz (10³ Hz) |
| **LV** | Low Voltage |
| **LVCMOS** | Low-Voltage CMOS (logic family at 1.8V–3.3V) |
| **mA** | Milliampere (10⁻³ A) |
| **ME** | Intel Management Engine (embedded security co-processor) |
| **MHz** | Megahertz (10⁶ Hz) |
| **MIPS** | Microprocessor without Interlocked Pipeline Stages / Millions of Instructions Per Second |
| **MISO** | Master In Slave Out (SPI) |
| **MMC** | MultiMediaCard |
| **MOSI** | Master Out Slave In (SPI) |
| **MX6** | NXP i.MX6 - ARM Cortex-A9 applications processor |
| **NACK** | the controller will send a Not Acknowledge |
| **NC** | is No Connect |
| **nCS** | Negative Chip Select (active-low) |
| **nIRQ** | Negative Interrupt Request (active-low IRQ signal) |
| **NMEA** | National Marine Electronics Association (GPS sentence format standard) |
| **OD** | Open Drain (output type; pulls low only, needs external pull-up) |
| **OpenGarages** | Open Garages - open vehicle research community and Car Hacker's Handbook |
| **OpenOCD** | Open On-Chip Debugger (open-source debug software) |
| **OS** | Operating System |
| **OTG** | originally used for USB On-The-Go |
| **OTP** | One-Time Programmable (memory that can only be written once) |
| **PC** | Program Counter |
| **PCB** | Printed Circuit Board |
| **PCI** | Peripheral Component Interconnect |
| **PCIe** | Peripheral Component Interconnect Express |
| **PDF** | Portable Document Format |
| **PDI** | Program and Debug Interface (Atmel AVR XMEGA debug) |
| **PP** | Protection Profile (CC security requirements document) |
| **RAM** | Random Access Memory |
| **RG58U** | RG-58 50-ohm coaxial cable (U = universal jacket) |
| **ROM** | Read-Only Memory |
| **RS-232** | Recommended Standard 232 - serial communication standard |
| **RX** | Receive (data line) |
| **SAM3U** | Microchip SAM3U - ARM Cortex-M3 USB microcontroller (used in ChipWhisperer) |
| **SCA** | Side-Channel Attack |
| **SCK** | Serial Clock (SPI clock signal) |
| **SCL** | Serial Clock Line (I2C clock signal) |
| **SCLK** | Serial Clock |
| **SD** | Secure Digital (memory card standard) |
| **SDA** | Serial Data Line (I2C data signal) |
| **SDI** | and serial data in |
| **SDIO** | Secure Digital Input/Output |
| **SDO** | devices use only the notation serial data out |
| **SoC** | System on Chip |
| **SPI** | Serial Peripheral Interface |
| **SPIDEN** | Secure Privileged Invasive Debug Enable (ARM CoreSight signal) |
| **SPNIDEN** | Secure Privileged Non-Invasive Debug Enable (ARM CoreSight signal) |
| **SRST** | System Reset (JTAG target system reset) |
| **SS** | Slave Select (SPI chip select) |
| **SSRX** | SuperSpeed Receive pair (USB 3.0) |
| **SSTX** | SuperSpeed Transmit pair (USB 3.0) |
| **TAP** | control happens through the JTAG test access port |
| **TCK** | Test Clock (JTAG clock signal) |
| **TDI** | Test Data In (JTAG serial data input) |
| **TDO** | Test Data Out (JTAG serial data output) |
| **TI** | Texas Instruments |
| **TMS** | Test Mode Select (JTAG state machine control) |
| **TopJTAG** | JTAG Boundary Scan Analysis software tool |
| **TRST** | Test Reset (JTAG optional reset signal) |
| **TTL** | re likely to see is the transistor-transistor logic |
| **TTY** | TeleTYpe - Unix terminal device interface |
| **TWI** | Two-Wire Interface (Atmel's name for I2C) |
| **TX** | Transmit (data line) |
| **UART** | Universal Asynchronous Receiver/Transmitter |
| **UrJTAG** | open source |
| **us** | Microsecond (10⁻⁶ s) |
| **USART** | Universal Synchronous/Asynchronous Receiver/Transmitter |
| **USB** | Universal Serial Bus |
| **VBUS** | USB Bus Voltage (+5V supply from USB host) |
| **VCC** | Voltage Common Collector (positive supply voltage) |
| **VDD** | Voltage Drain Drain (positive supply in CMOS) |
| **VGA** | Video Graphics Array |
| **VHDL** | VHSIC Hardware Description Language (Very High Speed IC HDL) |
| **VIH** | Voltage Input High (minimum voltage for logic HIGH input) |
| **VIL** | Voltage Input Low (maximum voltage for logic LOW input) |
| **VOH** | Voltage Output High (guaranteed output HIGH voltage) |
| **VOL** | Voltage Output Low (guaranteed output LOW voltage) |
| **VSS** | Voltage Source Source (ground/negative supply in CMOS) |
| **XMEGA** | Atmel AVR XMEGA - extended AVR microcontroller family |

## Chapter 3: Casing the Joint: Identifying Components and Gathering Information

| Abbreviation | Definition / Full Form |
|---|---|
| **AES** | Advanced Encryption Standard (NIST FIPS 197, symmetric block cipher) |
| **AES-128** | AES with 128-bit key length |
| **AES-256** | AES with 256-bit key (highest standard AES security level) |
| **ARM** | Advanced RISC Machines (processor architecture) |
| **BCM4354KKUBG** | Broadcom BCM4354 - 802.11ac WiFi + Bluetooth combo chip |
| **BGA** | Ball Grid Array |
| **BOM** | or a bill of materials |
| **BSDL** | Boundary Scan Description Language (IEEE 1149.1) |
| **CAN** | Controller Area Network (automotive/industrial serial bus) |
| **CBC** | Cipher Block Chaining (symmetric cipher mode) |
| **CJ** | ChipJabber (fault injection platform) |
| **CKIL** | Clock Input Low (i.MX low-frequency 32kHz oscillator input) |
| **CMAC** | or cipher-based message authentication code |
| **CMD** | Command (prompt / command line interpreter) |
| **CPU** | Central Processing Unit |
| **CRC** | Cyclic Redundancy Check (error detection code) |
| **CRC32** | 32-bit Cyclic Redundancy Check |
| **CRYSTALS** | CRYSTALS - lattice-based post-quantum cryptography suite (Kyber + Dilithium) |
| **CSP** | Chip Scale Package |
| **CTR** | Counter Mode (stream cipher mode from block cipher) |
| **DFU** | Some devices support the USB Direct Firmware Update |
| **DIP** | Dual In-line Package |
| **DIY** | Do It Yourself |
| **DRAM** | Dynamic Random Access Memory |
| **ECB** | Electronic Code Book (insecure symmetric cipher mode) |
| **ECC** | Elliptic Curve Cryptography |
| **ECDSA** | Elliptic Curve Digital Signature Algorithm |
| **ECU** | Electronic Control Unit (automotive computer module) |
| **eFUSE** | Electronic Fuse (one-time programmable bit in silicon) |
| **EM** | Electromagnetic |
| **EM3587** | The main microcontroller on this device |
| **eMMC** | embedded MultiMediaCard |
| **ESDHC** | Enhanced Secure Digital Host Controller (NXP SD/MMC controller) |
| **EXTAL** | External Crystal input pin (clock) |
| **EXTEST** | External Test (JTAG boundary scan instruction) |
| **FBGA** | Fine-pitch Ball Grid Array |
| **FC** | Flip Chip / Fiber Channel |
| **FC-BGA** | Flip Chip Ball Grid Array |
| **FCC** | Federal Communications Commission |
| **FCCID** | FCC Identification Number (device radio certification ID) |
| **FPBGA** | Fine-Pitch Ball Grid Array |
| **FPGA** | Field-Programmable Gate Array |
| **FT2232H** | FTDI FT2232H - dual-channel USB to serial/JTAG chip |
| **FTDI** | Future Technology Devices International (USB interface chips) |
| **GCM** | Galois/Counter Mode (authenticated encryption with GHASH) |
| **GitHub** | GitHub - web-based platform for Git version control and open-source code |
| **GND** | Ground (electrical reference / 0V) |
| **GPHY** | Gigabit PHY (Ethernet physical layer transceiver) |
| **GPIO** | General Purpose Input/Output |
| **HDMI** | High-Definition Multimedia Interface |
| **HMAC** | Hash-based Message Authentication Code |
| **I2C** | Inter-Integrated Circuit |
| **IC** | Integrated Circuit |
| **IDA** | running on the device using the Interactive Disassembler |
| **iMX53RM** | NXP i.MX53 Reference Manual (applications processor docs) |
| **IoT** | Internet of Things |
| **IP** | Internet Protocol / Intellectual Property |
| **JP** | Jumper (PCB solder bridge or pin header) |
| **JP1** | Jumper 1 (first PCB jumper connector) |
| **JTAG** | Joint Test Action Group (IEEE 1149.1 boundary scan standard) |
| **KiCAD** | KiCad - open-source PCB design / EDA software |
| **LD1117** | STMicroelectronics LD1117 - low-dropout 800mA linear regulator |
| **LSI** | Large Scale Integration |
| **LTC3589** | Analog Devices LTC3589 - 8-output power management IC |
| **LZMA** | Lempel-Ziv-Markov chain Algorithm (high-ratio compression, used in 7-Zip) |
| **MD5** | Message Digest 5 (128-bit hash, cryptographically broken) |
| **MHz** | Megahertz (10⁶ Hz) |
| **MIPS** | Microprocessor without Interlocked Pipeline Stages / Millions of Instructions Per Second |
| **MPC5676R** | NXP MPC5676R - Power Architecture dual-core automotive MCU |
| **MT** | Micron Technology (memory chip prefix) / Mega-Transistor |
| **MX53** | NXP i.MX53 - ARM Cortex-A8 applications processor |
| **MX53UG** | NXP i.MX53 User's Guide (document reference) |
| **MX6** | NXP i.MX6 - ARM Cortex-A9 applications processor |
| **NAND** | NAND Flash (used in SSDs, eMMC, USB drives) |
| **NC51** | The product code part of the reference |
| **NDA** | Non-Disclosure Agreement |
| **nRST** | Negative Reset (active-low system reset pin) |
| **nTRST** | Negative Test Reset (active-low JTAG reset) |
| **NXP** | NXP Semiconductors (formerly Philips Semiconductors) |
| **OS** | Operating System |
| **PBGA** | Plastic BGA and Fine Pitch BGA Plastic BGA |
| **PCB** | Printed Circuit Board |
| **PD** | Pull-Down / Power Delivery (USB) |
| **PDF** | Portable Document Format |
| **PLCC** | relatively outdated technology is plastic leaded chip carrier |
| **PLL** | Phase-Locked Loop |
| **PMIC** | power management IC |
| **PQFP** | and plastic quad flat pack |
| **QFN** | Quad Flat No-lead package |
| **QFP** | Quad Flat Package |
| **RCA** | combinatorial logic could implement an n-bit ripple-carry adder |
| **ROM** | Read-Only Memory |
| **RSA** | Rivest–Shamir–Adleman (public-key asymmetric encryption) |
| **RSA-1024** | RSA with 1024-bit key (considered weak, deprecated) |
| **RSA-2048** | RSA with 2048-bit key (current minimum recommended) |
| **RTL8304MB** | Realtek RTL8304MB - 4-port Gigabit Ethernet switch |
| **RTL8881AB** | Realtek RTL8881AB - 802.11n WiFi SoC |
| **SD** | Secure Digital (memory card standard) |
| **SDRAM** | Synchronous Dynamic Random Access Memory |
| **SEC** | Security / Securities and Exchange Commission |
| **SMD** | Surface Mount Device |
| **SMT** | Surface Mount Technology |
| **SoC** | System on Chip |
| **SOIC** | Small Outline Integrated Circuit |
| **SOIC-8** | Small Outline IC - 8 pin package |
| **SON** | No-lead package The small outline no-lead |
| **SOP** | Small Outline Package |
| **SOT-353** | Small Outline Transistor (5-pin SOT package) |
| **SPDIF** | Sony/Philips Digital Interface Format (audio) |
| **SPI** | Serial Peripheral Interface |
| **TAP** | control happens through the JTAG test access port |
| **TCK** | Test Clock (JTAG clock signal) |
| **TD** | Transmit Data / Time Delay |
| **TDI** | Test Data In (JTAG serial data input) |
| **TDO** | Test Data Out (JTAG serial data output) |
| **TEBGA** | Grid Array The thermally enhanced ball grid array |
| **TMS** | Test Mode Select (JTAG state machine control) |
| **TopJTAG** | JTAG Boundary Scan Analysis software tool |
| **TP** | Test Point (PCB exposed pad for probing) |
| **TQFP** | Thin Quad Flat Package |
| **TQFP-64** | Thin Quad Flat Package - 64 pins |
| **TSON** | Thin Small Outline No-lead package |
| **TSOP** | pin thin small outline package |
| **TSSOP** | or thin-shrink SOP |
| **TV** | Television |
| **UART** | Universal Asynchronous Receiver/Transmitter |
| **us** | Microsecond (10⁻⁶ s) |
| **US** | United States |
| **USA** | United States of America |
| **USB** | Universal Serial Bus |
| **USENIX** | USENIX Association - non-profit computing/security research organization |
| **USPTO** | on the United States Patent and Trademark Office |
| **VCC** | Voltage Common Collector (positive supply voltage) |
| **VDDAL1** | If you want to glitch L1 cache |
| **VDDGP** | ARM core voltage |
| **VFI** | We discuss voltage fault injection |
| **VR** | SPI-based protocol |
| **WLCSP** | such as the wafer-level CSP |
| **WSON** | wide small outline no lead |
| **XTAL1** | Crystal Oscillator Input Pin 1 |
| **XTAL2** | Crystal Oscillator Input Pin 2 (output) |
| **ZigBee** | IEEE 802.15.4-based low-power wireless mesh protocol |

## Chapter 4: Bull in a Porcelain Shop: Introducing Fault Injection

| Abbreviation | Definition / Full Form |
|---|---|
| **3DES** | Triple Data Encryption Standard (3× DES for stronger security) |
| **AES** | Advanced Encryption Standard (NIST FIPS 197, symmetric block cipher) |
| **ARM** | Advanced RISC Machines (processor architecture) |
| **ASIL-D** | Automotive Safety Integrity Level D (highest ISO 26262 level) |
| **BNE** | Branch if Not Equal (assembly instruction, MIPS) |
| **CD** | not required for this experiment |
| **CPU** | Central Processing Unit |
| **CRT** | Chinese Remainder Theorem (used in fast RSA decryption) |
| **DFA** | Differential Fault Analysis |
| **DMA** | Direct Memory Access |
| **EB** | Error Bit / Erasable Block |
| **ECC** | Elliptic Curve Cryptography |
| **ECDSA** | Elliptic Curve Digital Signature Algorithm |
| **EM** | Electromagnetic |
| **FPGA** | Field-Programmable Gate Array |
| **GHz** | Gigahertz (10⁹ cycles per second) |
| **GPIO** | General Purpose Input/Output |
| **IDA** | running on the device using the Interactive Disassembler |
| **ISB** | support an instruction synchronization barrier |
| **JTAG** | Joint Test Action Group (IEEE 1149.1 boundary scan standard) |
| **LED** | Light-Emitting Diode |
| **MCU** | Microcontroller Unit |
| **MHz** | Megahertz (10⁶ Hz) |
| **OpenSSH** | OpenSSH - open-source implementation of Secure Shell (SSH) protocol |
| **OpenSSL** | Open-source implementation of SSL/TLS cryptographic protocols |
| **OS** | Operating System |
| **PC** | Program Counter |
| **PCB** | Printed Circuit Board |
| **PhD** | Doctor of Philosophy (doctoral academic degree) |
| **RAM** | Random Access Memory |
| **RS232** | Recommended Standard 232 - serial communication standard |
| **RSA** | Rivest–Shamir–Adleman (public-key asymmetric encryption) |
| **RSA-CRT** | Chinese Remainder Theorem |
| **SRAM** | Static Random Access Memory |
| **us** | Microsecond (10⁻⁶ s) |
| **USB** | Universal Serial Bus |
| **VCC** | Voltage Common Collector (positive supply voltage) |
| **VHDL** | VHSIC Hardware Description Language (Very High Speed IC HDL) |

## Chapter 5: Don’t Lick the Probe: How to Inject Faults

| Abbreviation | Definition / Full Form |
|---|---|
| **AVR** | Alf and Vegard's RISC processor (Atmel/Microchip 8-bit MCU) |
| **BADFET** | Transistor-based crowbar fault injection circuit |
| **BBI** | Body Biasing Injection (substrate voltage fault injection) |
| **BGA** | Ball Grid Array |
| **BY** | Byte (sometimes used in part numbers) |
| **CARDIS** | Smart Card Research and Advanced Application Conference |
| **CC** | Common Criteria (ISO/IEC 15408 - IT security evaluation standard) |
| **CHES** | The workshop on Cryptographic Hardware and Embedded Systems |
| **ChipJabber** | or NewAE Technology |
| **ChipSHOUTER** | NewAE Technology electromagnetic fault injection (EMFI) tool |
| **ChipWhisperer** | NewAE Technology open-source side-channel analysis and fault injection tool |
| **CPU** | Central Processing Unit |
| **DDR** | Double Data Rate (SDRAM clocking both edges) |
| **DEFCON** | DEF CON - annual hacker security conference (Las Vegas) |
| **DIP** | Dual In-line Package |
| **DMN2056U** | Diodes Inc. DMN2056U - N-channel MOSFET (small signal) |
| **ECU** | Electronic Control Unit (automotive computer module) |
| **EDA** | Electronic Design Automation |
| **EM** | Electromagnetic |
| **EMC** | Electromagnetic Compatibility |
| **EMFI** | Electromagnetic Fault Injection |
| **ESCAR** | Embedded Security in Cars - automotive security conference |
| **EU** | European Union |
| **FA** | were pioneered in the field of failure analysis |
| **FDTC** | is normally co-located with a conference on fault-tolerance |
| **FI** | Fault Injection |
| **FIB** | Focused Ion Beam (nanoscale milling/imaging tool) |
| **FPGA** | Field-Programmable Gate Array |
| **GCC** | GNU Compiler Collection |
| **GHz** | Gigahertz (10⁹ cycles per second) |
| **GiANT** | GiANT - Generic IP-Agnostic Netlist Testing (logic testing) |
| **GND** | Ground (electrical reference / 0V) |
| **GPIO** | General Purpose Input/Output |
| **HDMI** | High-Definition Multimedia Interface |
| **IACR** | International Association for Cryptologic Research |
| **IC** | Integrated Circuit |
| **ICMC** | International Cryptographic Module Conference |
| **IRF7807** | Infineon IRF7807 - N-channel power MOSFET |
| **JTAG** | Joint Test Action Group (IEEE 1149.1 boundary scan standard) |
| **LPC** | Low Pin Count bus (Intel platform bus replacing ISA) |
| **LPC-** | Low Pin Count bus (prefix for LPC signals) |
| **LPC1114** | NXP LPC1114 - ARM Cortex-M0 microcontroller |
| **LVDS** | a type of signaling called low-voltage differential signaling |
| **mA** | Milliampere (10⁻³ A) |
| **MAX4619** | Maxim MAX4619 - CMOS 8-channel analog multiplexer |
| **MHz** | Megahertz (10⁶ Hz) |
| **MOSFET** | Metal-Oxide-Semiconductor Field-Effect Transistor |
| **MSc** | Master of Science |
| **NAND** | NAND Flash (used in SSDs, eMMC, USB drives) |
| **NewAE** | NewAE Technology Inc. - hardware security research tools |
| **ns** | Nanosecond (10⁻⁹ s) |
| **NXP** | NXP Semiconductors (formerly Philips Semiconductors) |
| **PCB** | Printed Circuit Board |
| **PKCS** | Public Key Cryptography Standards (RSA Security standards series) |
| **PLL** | Phase-Locked Loop |
| **PoC** | Proof of Concept |
| **RC** | shows the use of resistor-capacitor |
| **RCA** | combinatorial logic could implement an n-bit ripple-carry adder |
| **RECON** | REcon - reverse engineering and vulnerability research conference |
| **RSA** | Rivest–Shamir–Adleman (public-key asymmetric encryption) |
| **RSA-2048** | RSA with 2048-bit key (current minimum recommended) |
| **SEM** | Scanning Electron Microscope |
| **SMA** | SubMiniature version A RF connector (50-ohm coaxial) |
| **SoC** | System on Chip |
| **STM32** | STMicroelectronics STM32 - ARM Cortex-M microcontroller family |
| **us** | Microsecond (10⁻⁶ s) |
| **VCC** | Voltage Common Collector (positive supply voltage) |
| **VCORE** | Core Voltage (CPU/FPGA internal supply voltage) |
| **WLCSP** | such as the wafer-level CSP |
| **WOOT** | USENIX Workshop on Offensive Technologies |
| **XMEGA** | Atmel AVR XMEGA - extended AVR microcontroller family |
| **XOR** | Exclusive OR (fundamental bitwise operation in cryptography) |
| **XY** | Two-axis coordinate system (X horizontal, Y vertical) |

## Chapter 6: Bench Time: Fault Injection Lab

| Abbreviation | Definition / Full Form |
|---|---|
| **API** | Application Programming Interface |
| **ARM** | Advanced RISC Machines (processor architecture) |
| **BBQ** | Barbeque (in book context: BBQ lighter used as crude fault injection source) |
| **CB** | Electronic Code Book |
| **ChipWhisperer** | NewAE Technology open-source side-channel analysis and fault injection tool |
| **CPU** | Central Processing Unit |
| **CRP** | Code Read Protection (NXP LPC flash read-back protection) |
| **CRP1** | Code Read Protection Level 1 - debug disabled, SWD allowed |
| **CRP2** | Code Read Protection Level 2 - most debug disabled |
| **CRP3** | Code Read Protection Level 3 - maximum protection, no recovery |
| **CRT** | Chinese Remainder Theorem (used in fast RSA decryption) |
| **CW** | ChipWhisperer (NewAE hardware security tool) |
| **CW-** | ChipWhisperer product line prefix |
| **CWNANO** | ChipWhisperer Nano - entry-level SCA/fault injection board |
| **DC** | Direct Current |
| **DFA** | Differential Fault Analysis |
| **DIP** | Dual In-line Package |
| **EF** | Enable Flag / Error Flag |
| **FPGA** | Field-Programmable Gate Array |
| **GCD** | efficient algorithm for calculating the greatest common divisor |
| **GitHub** | GitHub - web-based platform for Git version control and open-source code |
| **GND** | Ground (electrical reference / 0V) |
| **IACR** | International Association for Cryptologic Research |
| **ISP** | function enters the in- system programming |
| **JTAG** | Joint Test Action Group (IEEE 1149.1 boundary scan standard) |
| **LPC** | Low Pin Count bus (Intel platform bus replacing ISA) |
| **LPC-** | Low Pin Count bus (prefix for LPC signals) |
| **LPC1114** | NXP LPC1114 - ARM Cortex-M0 microcontroller |
| **MBED-TLS** | ARM Mbed TLS - lightweight TLS/SSL library for embedded devices |
| **MOSFET** | Metal-Oxide-Semiconductor Field-Effect Transistor |
| **MS** | Microsoft / Millisecond |
| **NAE** | NewAE Technology Inc. (hardware security research) |
| **NAE-CWLITE-ARM** | bit ARM |
| **NXP** | NXP Semiconductors (formerly Philips Semiconductors) |
| **OAEP** | because padding schemes like Optimal Asymmetric Encryption Padding |
| **PC** | Program Counter |
| **PCB** | Printed Circuit Board |
| **PKCS** | Public Key Cryptography Standards (RSA Security standards series) |
| **QFN** | Quad Flat No-lead package |
| **RAM** | Random Access Memory |
| **RSA** | Rivest–Shamir–Adleman (public-key asymmetric encryption) |
| **RSA-CRT** | Chinese Remainder Theorem |
| **RXD** | Receive Data |
| **SHA** | Secure Hash Algorithm |
| **SHA-256** | Secure Hash Algorithm 256-bit (SHA-2 family, widely used) |
| **SMA** | SubMiniature version A RF connector (50-ohm coaxial) |
| **SRAM** | Static Random Access Memory |
| **STM32F0** | STM32F0 - ARM Cortex-M0 entry-level STM32 series |
| **SWD** | Serial Wire Debug (ARM 2-pin debug interface) |
| **TLS** | Transport Layer Security |
| **TS12A4514** | Texas Instruments TS12A4514 - 4-channel SPST analog switch |
| **TS12A4515** | Texas Instruments TS12A4515 - 4-channel SPST analog switch (inverted enable) |
| **TXD** | Transmit Data |
| **UEXT** | Universal EXTension connector (Olimex open hardware standard) |
| **UEXT-1** | UEXT Connector Pin 1 (3.3V power) |
| **UEXT-2** | UEXT Connector Pin 2 (GND) |
| **UEXT-3** | UEXT Connector Pin 3 (TX - UART) |
| **UEXT-4** | UEXT Connector Pin 4 (RX - UART) |
| **us** | Microsecond (10⁻⁶ s) |
| **USB** | Universal Serial Bus |
| **VCC** | Voltage Common Collector (positive supply voltage) |
| **VDD** | Voltage Drain Drain (positive supply in CMOS) |
| **VR1** | so instead we put a variable resistor |

## Chapter 7: X Marks the Spot: Trezor One Wallet Memory Dump

| Abbreviation | Definition / Full Form |
|---|---|
| **CCC** | presentation at Chaos Computer Club |
| **ChipSHOUTER** | NewAE Technology electromagnetic fault injection (EMFI) tool |
| **DUT** | Device Under Test |
| **EM** | Electromagnetic |
| **EMFI** | Electromagnetic Fault Injection |
| **FaceWhisperer** | USB glitching and fault injection tool |
| **FjFS** | Fault Injection Filesystem (research) |
| **GitHub** | GitHub - web-based platform for Git version control and open-source code |
| **GPIO** | General Purpose Input/Output |
| **GUID** | Globally Unique Identifier |
| **IDA** | running on the device using the Interactive Disassembler |
| **JFAF** | JTAG Fast Assertion Failure (boundary scan failure condition) |
| **JTAG** | Joint Test Action Group (IEEE 1149.1 boundary scan standard) |
| **LCD** | not required for this experiment |
| **LUNA** | Great Scott Gadgets LUNA - USB protocol analyzer and debugger |
| **MPU** | Memory Protection Unit / Microprocessor Unit |
| **OS** | Operating System |
| **PCB** | Printed Circuit Board |
| **PhyWhisperer** | NewAE Technology USB protocol analyzer and glitcher |
| **PID** | Process Identifier / Product Identifier (USB PID) |
| **PyUSB** | Python USB - Python library for USB device control |
| **RAM** | Random Access Memory |
| **RDP1** | Read-back Disable Protection level 1 (STM32 flash protection) |
| **RDP2** | which completely disables JTAG |
| **SEGGER** | SEGGER Microcontroller GmbH (maker of J-Link debug probes) |
| **SRAM** | Static Random Access Memory |
| **ST** | Security Target (CC product-specific security specification) |
| **ST-LINK** | STMicroelectronics programming and debug probe |
| **STM32** | STMicroelectronics STM32 - ARM Cortex-M microcontroller family |
| **STM32F2** | STM32F2 - ARM Cortex-M3 STM32 performance series |
| **SWO** | Serial Wire Output (ITM trace output pin) |
| **us** | Microsecond (10⁻⁶ s) |
| **USB** | Universal Serial Bus |
| **USENIX** | USENIX Association - non-profit computing/security research organization |
| **WINUSB** | WinUSB - Microsoft generic USB driver framework |
| **WinUSB** | WinUSB - Microsoft generic USB driver framework |
| **WOOT** | USENIX Workshop on Offensive Technologies |
| **XY** | Two-axis coordinate system (X horizontal, Y vertical) |

## Chapter 8: I’ve Got the Power: Introduction to Power Analysis

| Abbreviation | Definition / Full Form |
|---|---|
| **AES** | Advanced Encryption Standard (NIST FIPS 197, symmetric block cipher) |
| **ARM** | Advanced RISC Machines (processor architecture) |
| **CRT** | Chinese Remainder Theorem (used in fast RSA decryption) |
| **DPA** | Differential Power Analysis |
| **DSS** | Digital Signature Standard (NIST FIPS 186) |
| **ECC** | Elliptic Curve Cryptography |
| **ECDSA** | Elliptic Curve Digital Signature Algorithm |
| **LCD** | not required for this experiment |
| **LED** | Light-Emitting Diode |
| **MBED-TLS** | ARM Mbed TLS - lightweight TLS/SSL library for embedded devices |
| **MM** | Millimeter / Mixed Mode |
| **NumPy** | Numerical Python - scientific computing library for Python |
| **OpenSSL** | Open-source implementation of SSL/TLS cryptographic protocols |
| **PCB** | Printed Circuit Board |
| **PIN** | what happens when implementing a personal identification number |
| **PoC** | Proof of Concept |
| **RSA** | Rivest–Shamir–Adleman (public-key asymmetric encryption) |
| **SM** | State Machine / Surface Mount |
| **SPA** | Simple Power Analysis |
| **TEMPEST** | Transient ElectroMagnetic Pulse Emanation STandard - NSA program for EM emanation security |
| **TLS** | Transport Layer Security |
| **us** | Microsecond (10⁻⁶ s) |
| **USA** | United States of America |
| **VE** | Voltage Enable |

## Chapter 9: Bench Time: Simple Power Analysis

| Abbreviation | Definition / Full Form |
|---|---|
| **AC** | Alternating Current |
| **AES** | Advanced Encryption Standard (NIST FIPS 197, symmetric block cipher) |
| **API** | Application Programming Interface |
| **ARM** | Advanced RISC Machines (processor architecture) |
| **BNC** | Bayonet Neill-Concelman (coaxial RF connector) |
| **BW** | Bandwidth |
| **ChipWhisperer** | NewAE Technology open-source side-channel analysis and fault injection tool |
| **CRI** | Color Rendering Index / Critical Research Infrastructure |
| **CWNANO** | ChipWhisperer Nano - entry-level SCA/fault injection board |
| **DC** | Direct Current |
| **DPA** | Differential Power Analysis |
| **FPGA** | Field-Programmable Gate Array |
| **GPIO** | General Purpose Input/Output |
| **GS** | giga sample |
| **IC** | Integrated Circuit |
| **mA** | Milliampere (10⁻³ A) |
| **MATLAB** | MathWorks MATLAB - Matrix Laboratory numerical computing environment |
| **MHz** | Megahertz (10⁶ Hz) |
| **MS** | Microsoft / Millisecond |
| **mV** | Millivolt (10⁻³ V) |
| **nRST** | Negative Reset (active-low system reset pin) |
| **NumPy** | Numerical Python - scientific computing library for Python |
| **PicoScope** | USB digital oscilloscope by Pico Technology |
| **ps** | Picosecond (10⁻¹² s) |
| **PS2000** | Pico Technology PicoScope 2000 series USB oscilloscope |
| **PyVISA** | Python VISA - Python library for controlling lab instruments (GPIB/USB/LAN) |
| **RNG** | Random Number Generator |
| **RX** | Receive (data line) |
| **SAD** | two arrays is a sum of absolute difference |
| **SCOPETYPE** | Oscilloscope type selection variable (ChipWhisperer scripting) |
| **SoC** | System on Chip |
| **SPA** | Simple Power Analysis |
| **STM32F0** | STM32F0 - ARM Cortex-M0 entry-level STM32 series |
| **TX** | Transmit (data line) |
| **us** | Microsecond (10⁻⁶ s) |
| **US** | United States |
| **USB** | Universal Serial Bus |
| **VCC** | Voltage Common Collector (positive supply voltage) |
| **VISA** | rely instead on the Virtual Instrument Software Architecture |
| **XMEGA** | Atmel AVR XMEGA - extended AVR microcontroller family |

## Chapter 10: Splitting the Difference: Differential Power Analysis

| Abbreviation | Definition / Full Form |
|---|---|
| **AES** | Advanced Encryption Standard (NIST FIPS 197, symmetric block cipher) |
| **AES-128** | AES with 128-bit key length |
| **AES-128-ECB** | AES 128-bit in Electronic Codebook mode |
| **AES-192** | AES with 192-bit key length |
| **AES-256** | AES with 256-bit key (highest standard AES security level) |
| **ASCII** | American Standard Code for Information Interchange (7-bit text encoding) |
| **AVR** | Alf and Vegard's RISC processor (Atmel/Microchip 8-bit MCU) |
| **CBC** | Cipher Block Chaining (symmetric cipher mode) |
| **CHES** | The workshop on Cryptographic Hardware and Embedded Systems |
| **ChipWhisperer** | NewAE Technology open-source side-channel analysis and fault injection tool |
| **CMOS** | Complementary Metal-Oxide-Semiconductor |
| **CPA** | Correlation Power Analysis (Brier, Clavier, Olivier 1998) |
| **DavyLandman** | Davy Landman - software security researcher |
| **DoM** | will be called taking the difference of means |
| **DPA** | Differential Power Analysis |
| **ECB** | Electronic Code Book (insecure symmetric cipher mode) |
| **ECC** | Elliptic Curve Cryptography |
| **FET** | Field-Effect Transistor |
| **GCM** | Galois/Counter Mode (authenticated encryption with GHASH) |
| **GND** | Ground (electrical reference / 0V) |
| **HD** | Another commonly found leakage model is Hamming distance |
| **HW** | number is referred to as the Hamming weight |
| **IC** | Integrated Circuit |
| **LSB** | describe how to break the least significant bit |
| **MS** | Microsoft / Millisecond |
| **NumPy** | Numerical Python - scientific computing library for Python |
| **PIN** | what happens when implementing a personal identification number |
| **RSA** | Rivest–Shamir–Adleman (public-key asymmetric encryption) |
| **SBox** | Substitution Box (non-linear lookup table in AES and other ciphers) |
| **SPA** | Simple Power Analysis |
| **us** | Microsecond (10⁻⁶ s) |
| **US** | United States |
| **VCC** | Voltage Common Collector (positive supply voltage) |
| **XMEGA** | Atmel AVR XMEGA - extended AVR microcontroller family |
| **XOR** | Exclusive OR (fundamental bitwise operation in cryptography) |

## Chapter 11: Gettin’ Nerdy with It: Advanced Power Analysis

| Abbreviation | Definition / Full Form |
|---|---|
| **AD8129** | Analog Devices AD8129 - low-cost differential receiver amplifier |
| **ADC** | Analog-to-Digital Converter |
| **AES** | Advanced Encryption Standard (NIST FIPS 197, symmetric block cipher) |
| **AES-128** | AES with 128-bit key length |
| **AES-256** | AES with 256-bit key (highest standard AES security level) |
| **AES-ECB** | AES in Electronic Codebook mode |
| **ASIC** | Application-Specific Integrated Circuit |
| **CARDIS** | Smart Card Research and Advanced Application Conference |
| **CHES** | The workshop on Cryptographic Hardware and Embedded Systems |
| **ChipWhisperer** | NewAE Technology open-source side-channel analysis and fault injection tool |
| **COSADE** | workshop on Constructive Side-Channel Analysis and Secure Design |
| **CPA** | Correlation Power Analysis (Brier, Clavier, Olivier 1998) |
| **CPU** | Central Processing Unit |
| **CT-RSA** | Cryptographers' Track at RSA Conference |
| **DA** | interactive disassembler |
| **DPA** | Differential Power Analysis |
| **DSP** | Digital Signal Processor |
| **ECB** | Electronic Code Book (insecure symmetric cipher mode) |
| **ECC** | Elliptic Curve Cryptography |
| **EM** | Electromagnetic |
| **EMA** | Electromagnetic Analysis |
| **FDTC** | is normally co-located with a conference on fault-tolerance |
| **FFT** | Fast Fourier Transform |
| **FIR** | A finite impulse response |
| **FPGA** | Field-Programmable Gate Array |
| **GHz** | Gigahertz (10⁹ cycles per second) |
| **GS** | giga sample |
| **GSR** | will first refer to the global success rate |
| **HD** | Another commonly found leakage model is Hamming distance |
| **HW** | number is referred to as the Hamming weight |
| **IC** | Integrated Circuit |
| **IIR** | or infinite impulse response |
| **MHz** | Megahertz (10⁶ Hz) |
| **ML** | analysis must keep up with the machine learning |
| **MM** | Millimeter / Mixed Mode |
| **MS** | Microsoft / Millisecond |
| **PC** | Program Counter |
| **PCB** | Printed Circuit Board |
| **PGE** | to represent this is the partial guessing entropy |
| **PicoScope** | USB digital oscilloscope by Pico Technology |
| **PROOFS** | and security proofs |
| **RSA** | Rivest–Shamir–Adleman (public-key asymmetric encryption) |
| **SAD** | two arrays is a sum of absolute difference |
| **SCA** | Side-Channel Attack |
| **SCA-** | Side-Channel Analysis (prefix in naming conventions) |
| **SMA** | SubMiniature version A RF connector (50-ohm coaxial) |
| **SoC** | System on Chip |
| **SPA** | Simple Power Analysis |
| **SPUC** | Secure Privileged Use Control |
| **TQFP** | Thin Quad Flat Package |
| **TVLA** | intermediate round variant of test vector leakage assessment |
| **us** | Microsecond (10⁻⁶ s) |
| **US** | United States |
| **XOR** | Exclusive OR (fundamental bitwise operation in cryptography) |
| **XY** | Two-axis coordinate system (X horizontal, Y vertical) |
| **XYZ** | Three-axis coordinate system |

## Chapter 12: Bench Time: Differential Power Analysis

| Abbreviation | Definition / Full Form |
|---|---|
| **AES** | Advanced Encryption Standard (NIST FIPS 197, symmetric block cipher) |
| **AES-128** | AES with 128-bit key length |
| **AES-256** | AES with 256-bit key (highest standard AES security level) |
| **AN2462** | which was Atmel application note AVR231 |
| **CBC** | Cipher Block Chaining (symmetric cipher mode) |
| **ChipWhisperer** | NewAE Technology open-source side-channel analysis and fault injection tool |
| **CPA** | Correlation Power Analysis (Brier, Clavier, Olivier 1998) |
| **CRC** | Cyclic Redundancy Check (error detection code) |
| **CRC-16** | 16-bit Cyclic Redundancy Check |
| **CRC-CCITT** | CRC variant used in CCITT standards (x¹⁶+x¹²+x⁵+1 polynomial) |
| **DoM** | will be called taking the difference of means |
| **DPA** | Differential Power Analysis |
| **DR** | and the data register |
| **DRi** | Data Register i (indexed register notation) |
| **ECB** | Electronic Code Book (insecure symmetric cipher mode) |
| **HAL** | Hardware Abstraction Layer |
| **IV** | Initialization Vector (random nonce for cipher modes) |
| **IVi** | Initialization Vector (subscript i - iteration/index) |
| **LSB** | describe how to break the least significant bit |
| **MCU** | Microcontroller Unit |
| **MS** | Microsoft / Millisecond |
| **MSB** | followed by the most significant bit |
| **NumPy** | Numerical Python - scientific computing library for Python |
| **NXP** | NXP Semiconductors (formerly Philips Semiconductors) |
| **PT** | Plain Text (unencrypted data - used in crypto notation) |
| **PTi** | Plain Text block i (mathematical cryptography notation) |
| **RSA** | Rivest–Shamir–Adleman (public-key asymmetric encryption) |
| **RX** | Receive (data line) |
| **SCA** | Side-Channel Attack |
| **SCOPETYPE** | Oscilloscope type selection variable (ChipWhisperer scripting) |
| **SoC** | System on Chip |
| **SPA** | Simple Power Analysis |
| **STM32F3** | STM32F3 - ARM Cortex-M4 STM32 mixed-signal series |
| **TI** | Texas Instruments |
| **TX** | Transmit (data line) |
| **us** | Microsecond (10⁻⁶ s) |
| **USB** | Universal Serial Bus |
| **VM** | they are provided in the virtual machine |
| **XMEGA** | Atmel AVR XMEGA - extended AVR microcontroller family |
| **XOR** | Exclusive OR (fundamental bitwise operation in cryptography) |

## Chapter 13: No Kiddin’: Real-Life Examples

| Abbreviation | Definition / Full Form |
|---|---|
| **3DES** | Triple Data Encryption Standard (3× DES for stronger security) |
| **AES** | Advanced Encryption Standard (NIST FIPS 197, symmetric block cipher) |
| **AES-256** | AES with 256-bit key (highest standard AES security level) |
| **AES-CBC** | AES in Cipher Block Chaining mode |
| **AES-CCM** | AES in Counter with CBC-MAC authenticated encryption mode |
| **AES-CTR** | AES in Counter mode |
| **AES-ECB** | AES in Electronic Codebook mode |
| **AirTag** | Apple AirTag - Bluetooth/UWB tracking device |
| **API** | Application Programming Interface |
| **ARM** | Advanced RISC Machines (processor architecture) |
| **ASIL-D** | Automotive Safety Integrity Level D (highest ISO 26262 level) |
| **CB** | Electronic Code Book |
| **CBC** | Cipher Block Chaining (symmetric cipher mode) |
| **CBC-1** | Cipher Block Chaining step 1 (notation for first CBC operation) |
| **CBCm** | Cipher Block Chaining - m-th block (mathematical notation) |
| **CCM** | Counter with CBC-MAC (authenticated encryption mode) |
| **CD** | not required for this experiment |
| **CPA** | Correlation Power Analysis (Brier, Clavier, Olivier 1998) |
| **CPAa** | CPA result for key hypothesis a (mathematical notation) |
| **CPAb** | CPA result for key hypothesis b (mathematical notation) |
| **CPLD** | Complex Programmable Logic Device |
| **CPU** | Central Processing Unit |
| **CTm** | Ciphertext block m (mathematical notation) |
| **CTR** | Counter Mode (stream cipher mode from block cipher) |
| **CTRm** | Counter Mode - m-th block (mathematical notation) |
| **CUDA** | Compute Unified Device Architecture (NVIDIA GPU computing) |
| **DFA** | Differential Fault Analysis |
| **DQx** | DDR data bus signal group x |
| **DRAM** | Dynamic Random Access Memory |
| **DS2432** | Maxim DS2432 - 1-Wire 1Kb EEPROM with SHA-1 authentication |
| **DS28E01** | Maxim DS28E01 - 1-Wire 1Kb EEPROM with SHA-1 (low-voltage) |
| **DST80** | Texas Instruments DST80 - transponder chip used in immobilizers |
| **ECB** | Electronic Code Book (insecure symmetric cipher mode) |
| **ECDSA** | Elliptic Curve Digital Signature Algorithm |
| **EEPROM** | Electrically Erasable Programmable Read-Only Memory |
| **EFM32** | Silicon Labs EFM32 - ARM Cortex-M energy-friendly MCU family |
| **EFM32WG** | Silicon Labs EFM32 Wonder Gecko - high-performance EFM32 MCU |
| **EM** | Electromagnetic |
| **EMFI** | Electromagnetic Fault Injection |
| **FI** | Fault Injection |
| **FIDO** | Fast IDentity Online (open authentication standard alliance) |
| **FPGA** | Field-Programmable Gate Array |
| **FRAM** | Ferroelectric Random Access Memory (non-volatile, fast R/W) |
| **GeoHot** | which occurred thanks to George Hotz |
| **GliGli** | GliGli - Xbox 360 hacker who discovered RGH glitch exploit |
| **GPU** | Graphics Processing Unit |
| **HTAB** | which just means the hash table |
| **I2C** | Inter-Integrated Circuit |
| **IC** | Integrated Circuit |
| **IEEE** | Institute of Electrical and Electronics Engineers |
| **IETF** | Internet Engineering Task Force |
| **IoT** | Internet of Things |
| **ISO** | International Organization for Standardization |
| **IV** | Initialization Vector (random nonce for cipher modes) |
| **JavaCard** | Java Card - Java platform for smart cards and secure elements |
| **KeeLoq** | Microchip KeeLoq - proprietary cipher for remote keyless entry (rolling code) |
| **kHz** | Kilohertz (10³ Hz) |
| **MCU** | Microcontroller Unit |
| **MF3ICD40** | NXP MIFARE Classic 4K - contactless smart card IC (40mm) |
| **MHz** | Megahertz (10⁶ Hz) |
| **NAND** | NAND Flash (used in SSDs, eMMC, USB drives) |
| **NFC** | Near Field Communication |
| **nRF52** | Nordic Semiconductor nRF52 Series - Bluetooth 5 SoC family |
| **OpenCL** | Open Computing Language (GPU/parallel computing framework) |
| **OTA** | just looking at sample implementations of Zigbee over-the-air |
| **OTP** | One-Time Programmable (memory that can only be written once) |
| **PC** | Program Counter |
| **PIC** | Peripheral Interface Controller (Microchip MCU family) |
| **PLL** | Phase-Locked Loop |
| **PS3** | Sony PlayStation 3 |
| **PT** | Plain Text (unencrypted data - used in crypto notation) |
| **PTm** | Plain Text block m (mathematical cryptography notation) |
| **RFC** | Request For Comments (IETF document standard) |
| **ROM** | Read-Only Memory |
| **RSA** | Rivest–Shamir–Adleman (public-key asymmetric encryption) |
| **SHA** | Secure Hash Algorithm |
| **SHA-1** | Secure Hash Algorithm 1 (160-bit output, deprecated) |
| **SoC** | System on Chip |
| **SPI** | Serial Peripheral Interface |
| **TI** | Texas Instruments |
| **TV** | Television |
| **U2F** | Universal 2nd Factor (FIDO open authentication standard) |
| **us** | Microsecond (10⁻⁶ s) |
| **USB** | Universal Serial Bus |
| **XOR** | Exclusive OR (fundamental bitwise operation in cryptography) |
| **ZigBee** | IEEE 802.15.4-based low-power wireless mesh protocol |
| **ZLL** | These lights communicate with the Zigbee Light Link |

## Chapter 14: Think of the Children: Countermeasures, Certifications, and Goodbytes

| Abbreviation | Definition / Full Form |
|---|---|
| **AES** | Advanced Encryption Standard (NIST FIPS 197, symmetric block cipher) |
| **CC** | Common Criteria (ISO/IEC 15408 - IT security evaluation standard) |
| **CCC** | presentation at Chaos Computer Club |
| **CHES** | The workshop on Cryptographic Hardware and Embedded Systems |
| **CMVP** | The Cryptographic Module Verification Program |
| **COSADE** | workshop on Constructive Side-Channel Analysis and Secure Design |
| **CPU** | Central Processing Unit |
| **CRC** | Cyclic Redundancy Check (error detection code) |
| **DPA** | Differential Power Analysis |
| **EAL** | Evaluation Assurance Level (CC security assurance level 1–7) |
| **EAL5** | Evaluation Assurance Level 5 (Common Criteria) |
| **EAL6** | Evaluation Assurance Level 6 (Common Criteria) |
| **ECC** | Elliptic Curve Cryptography |
| **EMVCo** | EMVco - organization managing EMV chip card standards |
| **EU** | European Union |
| **FDTC** | is normally co-located with a conference on fault-tolerance |
| **FI** | Fault Injection |
| **FIPS** | Federal Information Processing Standard (NIST standard series) |
| **FiSim** | Fault Injection Simulator (research fault simulation tool) |
| **FPGA** | Field-Programmable Gate Array |
| **HD** | Another commonly found leakage model is Hamming distance |
| **HMAC** | Hash-based Message Authentication Code |
| **HW** | number is referred to as the Hamming weight |
| **IC** | Integrated Circuit |
| **IoT** | Internet of Things |
| **ISO** | International Organization for Standardization |
| **JHAS** | new attacks in the Joint Hardware Attack Subgroup |
| **JIL** | Joint Interpretation Library (CC community guidelines) |
| **JIT** | re using optimizing |
| **LPC55S69** | NXP LPC55S69 - ARM Cortex-M33 secure microcontroller |
| **MHz** | Megahertz (10⁶ Hz) |
| **NaCl** | Networking and Cryptography Library (libsodium; pronounced 'salt') |
| **NDA** | Non-Disclosure Agreement |
| **NXP** | NXP Semiconductors (formerly Philips Semiconductors) |
| **OpenSSL** | Open-source implementation of SSL/TLS cryptographic protocols |
| **OS** | Operating System |
| **PhD** | Doctor of Philosophy (doctoral academic degree) |
| **PIN** | what happens when implementing a personal identification number |
| **PLL** | Phase-Locked Loop |
| **PP** | Protection Profile (CC security requirements document) |
| **PSA** | has a certification program called Platform Security Architecture |
| **RSA** | Rivest–Shamir–Adleman (public-key asymmetric encryption) |
| **RSM** | In Rotating S-boxes Masking |
| **SAE** | Society of Automotive Engineers |
| **SCA** | Side-Channel Attack |
| **SPA** | Simple Power Analysis |
| **TEE** | Trusted Execution Environment |
| **TVLA** | intermediate round variant of test vector leakage assessment |
| **us** | Microsecond (10⁻⁶ s) |
| **US** | United States |
| **XOR** | Exclusive OR (fundamental bitwise operation in cryptography) |

## Appendix A: Maxing Out Your Credit Card: Setting Up a Test Lab

| Abbreviation | Definition / Full Form |
|---|---|
| **AC** | Alternating Current |
| **ADC** | Analog-to-Digital Converter |
| **AliExpress** | or an overseas supply company |
| **AmScope** | GT200 table |
| **API** | Application Programming Interface |
| **ARC** | Argonaut RISC Core (Synopsys 32-bit processor architecture) |
| **ARM** | Advanced RISC Machines (processor architecture) |
| **AVR** | Alf and Vegard's RISC processor (Atmel/Microchip 8-bit MCU) |
| **AWG** | you might consider using an arbitrary waveform generator |
| **BadFET** | Transistor-based crowbar fault injection circuit |
| **BGA** | Ball Grid Array |
| **CAN** | Controller Area Network (automotive/industrial serial bus) |
| **CDB** | Command Descriptor Block (SCSI command structure) |
| **CDS** | Correlated Double Sampling (image sensor noise reduction) |
| **ChipJabber** | or NewAE Technology |
| **ChipSHOUTER** | NewAE Technology electromagnetic fault injection (EMFI) tool |
| **ChipShover** | NewAE Technology XY positioning stage for chip probing |
| **ChipWhisperer** | NewAE Technology open-source side-channel analysis and fault injection tool |
| **CSI** | Camera Serial Interface (MIPI CSI-2 standard) |
| **CW305** | ChipWhisperer CW305 - Artix-7 FPGA target board for hardware security |
| **CW308** | The ChipWhisperer UFO |
| **dB** | Decibel (logarithmic unit for signal ratios) |
| **DC** | Direct Current |
| **DediProg** | DediProg Engineering - manufacturer of flash programmers |
| **DIY** | Do It Yourself |
| **DMA** | Direct Memory Access |
| **DP832** | Rigol DP832 - programmable DC power supply (3 channels) |
| **DS1054Z** | Rigol DS1054Z - 4-channel 50MHz digital oscilloscope |
| **DSOX1102A** | Keysight DSOX1102A - 2-channel 100MHz oscilloscope |
| **ECP3** | Lattice ECP3 FPGA family |
| **ECU** | Electronic Control Unit (automotive computer module) |
| **EDA2** | Electronic Design Automation version 2 (or specific EDA tool reference) |
| **EDUX1002A** | Keysight EDUX1002A - 2-channel 50MHz entry oscilloscope |
| **EEPROM** | Electrically Erasable Programmable Read-Only Memory |
| **EEZ** | EEZ - open-source lab power supply and bench tools project |
| **EM** | Electromagnetic |
| **EM-FI** | Electromagnetic Fault Injection (hyphenated form) |
| **EMFI** | Electromagnetic Fault Injection |
| **EMV** | Europay, Mastercard, Visa (chip payment card standard) |
| **FaceDancer** | USB emulation and fuzzing tool (Great Scott Gadgets) |
| **FI** | Fault Injection |
| **FPGA** | Field-Programmable Gate Array |
| **FT2232H** | FTDI FT2232H - dual-channel USB to serial/JTAG chip |
| **FT232H** | FTDI FT232H - single-channel USB to serial/FIFO/I2C/SPI/JTAG chip |
| **FT232R** | FTDI FT232R - USB to UART IC |
| **FTDI** | Future Technology Devices International (USB interface chips) |
| **GDB** | the GNU Debugger |
| **GHz** | Gigahertz (10⁹ cycles per second) |
| **GNU** | GNU's Not Unix (free software operating system/tools) |
| **GoLogicXL** | Saleae GoLogic XL logic analyzer |
| **GoodFET** | Open-source JTAG/SPI/I2C programmer and protocol bridge |
| **GS** | giga sample |
| **GT200** | NVIDIA GT200 - GPU chip used in GeForce GTX 280 |
| **GUI** | Graphical User Interface |
| **HDL** | Hardware Description Language |
| **HDMI** | High-Definition Multimedia Interface |
| **HP** | Hewlett-Packard / High Performance |
| **HPROBE-15** | NewAE NAE-HPROBE-15 near-field EM probe (15mm loop) |
| **I2C** | Inter-Integrated Circuit |
| **IC** | Integrated Circuit |
| **IPA** | Clean the area well with isopropyl alcohol |
| **IR** | Infrared |
| **JBC** | JBC Tools - professional soldering station brand |
| **JTAG** | Joint Test Action Group (IEEE 1149.1 boundary scan standard) |
| **kHz** | Kilohertz (10³ Hz) |
| **LA1034** | Saleae Logic Pro 16 - 16-channel logic analyzer |
| **LAN** | Local Area Network |
| **LeCroy** | Teledyne LeCroy - oscilloscope and protocol analyzer manufacturer |
| **LNA** | as it contains a low-noise amplifier |
| **LPC** | Low Pin Count bus (Intel platform bus replacing ISA) |
| **LPC-** | Low Pin Count bus (prefix for LPC signals) |
| **LUNA** | Great Scott Gadgets LUNA - USB protocol analyzer and debugger |
| **MAX4619** | Maxim MAX4619 - CMOS 8-channel analog multiplexer |
| **MDO3000** | Tektronix MDO3000 series mixed domain oscilloscope |
| **MFA** | Multi-Factor Authentication |
| **MG** | Milligrams / mixed glitch (notation) |
| **MHz** | Megahertz (10⁶ Hz) |
| **MPC5777C** | found in some ECUs |
| **MS** | Microsoft / Millisecond |
| **MT** | Micron Technology (memory chip prefix) / Mega-Transistor |
| **mV** | Millivolt (10⁻³ V) |
| **NAE** | NewAE Technology Inc. (hardware security research) |
| **NCI** | Network and Communication Infrastructure / Non-Contact Interface |
| **NewAE** | NewAE Technology Inc. - hardware security research tools |
| **NF** | the noise figure |
| **NL** | NL for lead-free |
| **NXP** | NXP Semiconductors (formerly Philips Semiconductors) |
| **OCD** | On-Chip Debugger |
| **OpenGarages** | Open Garages - open vehicle research community and Car Hacker's Handbook |
| **OpenOCD** | Open On-Chip Debugger (open-source debug software) |
| **PA** | simple power analysis |
| **PC** | Program Counter |
| **PCB** | Printed Circuit Board |
| **PCI** | Peripheral Component Interconnect |
| **PCIe** | Peripheral Component Interconnect Express |
| **PhyWhisperer** | NewAE Technology USB protocol analyzer and glitcher |
| **PicoEVB** | Xilinx Artix-7 FPGA evaluation board in M.2 form factor |
| **PicoScope** | USB digital oscilloscope by Pico Technology |
| **PuTTY** | PuTTY - free SSH and Telnet terminal emulator for Windows |
| **QFN** | Quad Flat No-lead package |
| **RF** | Radio Frequency |
| **RISC** | Reduced Instruction Set Computer |
| **RISC-V** | Reduced Instruction Set Computer V (open-standard ISA, 5th generation) |
| **ROHS** | Restriction of Hazardous Substances (EU electronics directive) |
| **RPM** | Revolutions Per Minute |
| **RS232** | Recommended Standard 232 - serial communication standard |
| **SAD** | two arrays is a sum of absolute difference |
| **SAKURA** | Side-channel Attack standard evaluation board (AIST Japan) |
| **SAKURA-G** | SAKURA board variant with Xilinx Spartan-6 FPGA |
| **SASEBO** | Side-channel Attack Standard Evaluation Board (RCIS Japan) |
| **SCAPACK** | Side-Channel Analysis Pack (NewAE tool bundle) |
| **SDG6022X** | Siglent SDG6022X - arbitrary waveform / function generator |
| **SEGGER** | SEGGER Microcontroller GmbH (maker of J-Link debug probes) |
| **SLOTSCREAMER** | Slotscreamer - an open-source PCIe DMA hardware attack tool |
| **SMA** | SubMiniature version A RF connector (50-ohm coaxial) |
| **SMD** | Surface Mount Device |
| **SMD1L** | SMD1L series - 1A surface-mount Schottky diode |
| **SMD1NL** | SMD1NL - 1A surface-mount Schottky diode (no-lead variant) |
| **SMD291** | Kester SMD291 - no-clean flux pen for SMT soldering |
| **SMD291NL** | NL for lead-free |
| **SNR** | Signal-to-Noise Ratio |
| **SOIC** | Small Outline Integrated Circuit |
| **SPI** | Serial Peripheral Interface |
| **StarProg** | Universal device programmer by Starlabs |
| **TBS1000** | The Tektronix low-cost model |
| **TekBox** | TekBox Digital Solutions EMC/EMI measurement equipment |
| **TL866II** | MiniPro TL866II Plus - universal IC EEPROM/Flash programmer |
| **TopJTAG** | JTAG Boundary Scan Analysis software tool |
| **TQFP** | Thin Quad Flat Package |
| **TQFP-144** | Thin Quad Flat Package - 144 pins |
| **TS100** | Miniware TS100 - portable programmable soldering iron |
| **TTL** | re likely to see is the transistor-transistor logic |
| **UART** | Universal Asynchronous Receiver/Transmitter |
| **us** | Microsecond (10⁻⁶ s) |
| **US** | United States |
| **USB** | Universal Serial Bus |
| **USB3380** | PLX/Broadcom USB 3380 - USB 3.0 to PCIe bridge chip |
| **USB3880** | USB 3880 PCIe bridge chip |
| **VC** | Voltage Comparator / Version Control |
| **VCC** | Voltage Common Collector (positive supply voltage) |
| **VGA** | Video Graphics Array |
| **VISA** | rely instead on the Virtual Instrument Software Architecture |
| **WireShark** | Wireshark - open-source network protocol analyzer |
| **XMEGA** | Atmel AVR XMEGA - extended AVR microcontroller family |
| **XY** | Two-axis coordinate system (X horizontal, Y vertical) |
| **XYZ** | Three-axis coordinate system |
| **ZFL** | Mini-Circuits ZFL series low-noise RF amplifier |

## Appendix B: All Your Base Are Belong to Us: Popular Pinouts

| Abbreviation | Definition / Full Form |
|---|---|
| **ECU** | Electronic Control Unit (automotive computer module) |
| **JCOMP** | JTAG Compliance Enable (Xilinx specific pin) |
| **JTAG** | Joint Test Action Group (IEEE 1149.1 boundary scan standard) |
| **NC** | is No Connect |
| **NXP** | NXP Semiconductors (formerly Philips Semiconductors) |
| **OpenOCD** | Open On-Chip Debugger (open-source debug software) |
| **RDY** | Ready (handshake signal indicating device is ready) |
| **SEGGER** | SEGGER Microcontroller GmbH (maker of J-Link debug probes) |
| **SOIC** | Small Outline Integrated Circuit |
| **SPI** | Serial Peripheral Interface |
| **SWD** | Serial Wire Debug (ARM 2-pin debug interface) |
| **TAP** | control happens through the JTAG test access port |
| **US** | United States |
| **VDDE7** | I/O Supply Voltage for Bank 7 (FPGA I/O bank supply) |
| **WSON** | wide small outline no lead |

## Index

| Abbreviation | Definition / Full Form |
|---|---|
| **AC** | Alternating Current |
| **ADC** | Analog-to-Digital Converter |
| **AES** | Advanced Encryption Standard (NIST FIPS 197, symmetric block cipher) |
| **AES-256** | AES with 256-bit key (highest standard AES security level) |
| **ANN** | artificial neural network |
| **AWG** | you might consider using an arbitrary waveform generator |
| **BadFET** | Transistor-based crowbar fault injection circuit |
| **BBI** | Body Biasing Injection (substrate voltage fault injection) |
| **BBQ** | Barbeque (in book context: BBQ lighter used as crude fault injection source) |
| **BGA** | Ball Grid Array |
| **CAN** | Controller Area Network (automotive/industrial serial bus) |
| **CBC** | Cipher Block Chaining (symmetric cipher mode) |
| **CC** | Common Criteria (ISO/IEC 15408 - IT security evaluation standard) |
| **CCM** | Counter with CBC-MAC (authenticated encryption mode) |
| **ChipSHOUTER** | NewAE Technology electromagnetic fault injection (EMFI) tool |
| **ChipWhisperer** | NewAE Technology open-source side-channel analysis and fault injection tool |
| **CLKSCREW** | CLKscrew - voltage/frequency fault injection attack exploiting DVFS |
| **CPA** | Correlation Power Analysis (Brier, Clavier, Olivier 1998) |
| **CPU** | Central Processing Unit |
| **CRC** | Cyclic Redundancy Check (error detection code) |
| **CRT** | Chinese Remainder Theorem (used in fast RSA decryption) |
| **CSP** | Chip Scale Package |
| **CVSS** | Common Vulnerability Scoring System |
| **CWSS** | Common Weakness Scoring System |
| **dB** | Decibel (logarithmic unit for signal ratios) |
| **DC** | Direct Current |
| **DFA** | Differential Fault Analysis |
| **DFU** | Some devices support the USB Direct Firmware Update |
| **DMM** | Digital Multimeter |
| **DoM** | will be called taking the difference of means |
| **DPA** | Differential Power Analysis |
| **DRAM** | Dynamic Random Access Memory |
| **EAL** | Evaluation Assurance Level (CC security assurance level 1–7) |
| **ECB** | Electronic Code Book (insecure symmetric cipher mode) |
| **ECC** | Elliptic Curve Cryptography |
| **ECDSA** | Elliptic Curve Digital Signature Algorithm |
| **EEPROM** | Electrically Erasable Programmable Read-Only Memory |
| **EM** | Electromagnetic |
| **EM-FI** | Electromagnetic Fault Injection (hyphenated form) |
| **EMA** | Electromagnetic Analysis |
| **EMFI** | Electromagnetic Fault Injection |
| **eMMC** | embedded MultiMediaCard |
| **EMV** | Europay, Mastercard, Visa (chip payment card standard) |
| **EMVCo** | EMVco - organization managing EMV chip card standards |
| **FaceDancer** | USB emulation and fuzzing tool (Great Scott Gadgets) |
| **FCC** | Federal Communications Commission |
| **FI** | Fault Injection |
| **FIB** | Focused Ion Beam (nanoscale milling/imaging tool) |
| **FIPS** | Federal Information Processing Standard (NIST standard series) |
| **FiSim** | Fault Injection Simulator (research fault simulation tool) |
| **FTDI** | Future Technology Devices International (USB interface chips) |
| **GDB** | the GNU Debugger |
| **GNU** | GNU's Not Unix (free software operating system/tools) |
| **GSR** | will first refer to the global success rate |
| **HD** | Another commonly found leakage model is Hamming distance |
| **HID** | An example is the USB Human Interface Device |
| **HMAC** | Hash-based Message Authentication Code |
| **HTAB** | which just means the hash table |
| **HW** | number is referred to as the Hamming weight |
| **I2C** | Inter-Integrated Circuit |
| **IC** | Integrated Circuit |
| **ICB** | Indexed Code Block |
| **IDA** | running on the device using the Interactive Disassembler |
| **IEEE** | Institute of Electrical and Electronics Engineers |
| **IP** | Internet Protocol / Intellectual Property |
| **ISB** | support an instruction synchronization barrier |
| **ISO** | International Organization for Standardization |
| **IV** | Initialization Vector (random nonce for cipher modes) |
| **JHAS** | new attacks in the Joint Hardware Attack Subgroup |
| **JIL** | Joint Interpretation Library (CC community guidelines) |
| **JTAG** | Joint Test Action Group (IEEE 1149.1 boundary scan standard) |
| **LFI** | Laser Fault Injection |
| **LNA** | as it contains a low-noise amplifier |
| **LPC** | Low Pin Count bus (Intel platform bus replacing ISA) |
| **LUNA** | Great Scott Gadgets LUNA - USB protocol analyzer and debugger |
| **MBED-TLS** | ARM Mbed TLS - lightweight TLS/SSL library for embedded devices |
| **MMC** | MultiMediaCard |
| **OpenOCD** | Open On-Chip Debugger (open-source debug software) |
| **OpenSSH** | OpenSSH - open-source implementation of Secure Shell (SSH) protocol |
| **OTA** | just looking at sample implementations of Zigbee over-the-air |
| **OTG** | originally used for USB On-The-Go |
| **OTP** | One-Time Programmable (memory that can only be written once) |
| **PC** | Program Counter |
| **PCB** | Printed Circuit Board |
| **PCI** | Peripheral Component Interconnect |
| **PCIe** | Peripheral Component Interconnect Express |
| **PGE** | to represent this is the partial guessing entropy |
| **PhyWhisperer** | NewAE Technology USB protocol analyzer and glitcher |
| **PicoEVB** | Xilinx Artix-7 FPGA evaluation board in M.2 form factor |
| **PicoScope** | USB digital oscilloscope by Pico Technology |
| **PIN** | what happens when implementing a personal identification number |
| **PKCS** | Public Key Cryptography Standards (RSA Security standards series) |
| **PLL** | Phase-Locked Loop |
| **PMIC** | power management IC |
| **PQFP** | and plastic quad flat pack |
| **PSA** | has a certification program called Platform Security Architecture |
| **QFN** | Quad Flat No-lead package |
| **QFP** | Quad Flat Package |
| **ROM** | Read-Only Memory |
| **RS-232** | Recommended Standard 232 - serial communication standard |
| **RSA** | Rivest–Shamir–Adleman (public-key asymmetric encryption) |
| **RSM** | In Rotating S-boxes Masking |
| **SAD** | two arrays is a sum of absolute difference |
| **SAKURA** | Side-channel Attack standard evaluation board (AIST Japan) |
| **SASEBO** | Side-channel Attack Standard Evaluation Board (RCIS Japan) |
| **SD** | Secure Digital (memory card standard) |
| **SDIO** | Secure Digital Input/Output |
| **SEGGER** | SEGGER Microcontroller GmbH (maker of J-Link debug probes) |
| **SMD** | Surface Mount Device |
| **SoC** | System on Chip |
| **SOIC** | Small Outline Integrated Circuit |
| **SON** | No-lead package The small outline no-lead |
| **SOP** | Small Outline Package |
| **SPA** | Simple Power Analysis |
| **SPI** | Serial Peripheral Interface |
| **SWD** | Serial Wire Debug (ARM 2-pin debug interface) |
| **TA** | Trusted Application (runs inside TEE) |
| **TAs** | Trusted Applications (run inside TEE) |
| **TEE** | Trusted Execution Environment |
| **TEMPEST** | Transient ElectroMagnetic Pulse Emanation STandard - NSA program for EM emanation security |
| **TLS** | Transport Layer Security |
| **TQFP** | Thin Quad Flat Package |
| **TSON** | Thin Small Outline No-lead package |
| **TSOP** | pin thin small outline package |
| **TTL** | re likely to see is the transistor-transistor logic |
| **TVLA** | intermediate round variant of test vector leakage assessment |
| **UART** | Universal Asynchronous Receiver/Transmitter |
| **USB** | Universal Serial Bus |
| **VC** | Voltage Comparator / Version Control |
| **WLCSP** | such as the wafer-level CSP |
| **WSON** | wide small outline no lead |
| **XOR** | Exclusive OR (fundamental bitwise operation in cryptography) |
| **XY** | Two-axis coordinate system (X horizontal, Y vertical) |
