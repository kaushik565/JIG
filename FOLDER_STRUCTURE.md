# JIG Repository - Folder Structure Documentation

## Overview

This repository contains a complete **QR Code Scanner and Validation System** for batch production testing. The system integrates hardware (PIC18F4550 microcontroller) with Raspberry Pi software for automated quality control in manufacturing.

**Total Size:** ~5.5 MB (excluding .git)  
**Languages:** Python, C (embedded firmware)  
**Platform:** Raspberry Pi + PIC18F4550 Microcontroller  
**Purpose:** Automated QR code scanning and validation for batch cartridge testing

---

## Directory Structure

```
JIG/
├── ACTJv20(RJSR)/          # PIC18F4550 Embedded Firmware (3.9 MB)
├── SCANNER/                # Legacy Scanner Application (1.1 MB)
├── python/                 # Modern Batch Scanning App (556 KB)
└── README.md               # Basic project description
```

---

## 📁 ACTJv20(RJSR)/ - PIC18F4550 Firmware

**Size:** 3.9 MB  
**Language:** C (HI-TECH PICC-18 / Microchip C18)  
**Platform:** PIC18F4550 Microcontroller @ 48MHz  
**Purpose:** Cartridge advancement mechanism controller with Raspberry Pi integration

### Key Components

#### Source Files (.c / .h)
- **Main_PCR.c** - Main control loop and mechanism state machine
- **Functions.c / Functions.h** - Utility functions and system operations
- **service.c / service.h** - Mechanical service routines and error handling
- **SBC_Rpi.c / SBC_Rpi.h** - UART communication protocol with Raspberry Pi
- **LCD_module.c / LCD_module.h** - LCD display driver (I2C interface)
- **i2c_lcd.c / i2c_lcd.h** - I2C LCD library
- **uart_module.c / uart_module.h** - UART hardware abstraction
- **Pin_Definitions.h** - GPIO pin mapping and hardware definitions

#### Firmware Binaries (.hex)
- **TulabDX.hex** - Main firmware build (current version)
- **CPSR.hex** - Alternative build configuration
- **BNSR_FW_new_controller_board.hex** - Firmware for new PCB revision
- **BNSR_FW_old_controller_board.hex.hex** - Firmware for legacy PCB
- **QRSCANNER_old_CB_v13.hex** - Previous version backup

#### Build Configuration
- **TulabDX.mcp** - MPLAB IDE project file (primary)
- **CPSR.mcp** - Alternative project configuration
- **Makefile / NMakefile** - Command-line build scripts
- **BUILD.bat** - Windows build script
- **rm18f4550.lkr** - Linker script for PIC18F4550

#### Libraries & Object Files
- **p18f4550.lib** - PIC18F4550 standard library
- **clib.lib** - C runtime library
- **c018i.o** - C startup code
- **.o files** - Compiled object files (intermediate build artifacts)
- **.err files** - Compiler error/warning logs
- **.i files** - Preprocessor output

#### Documentation
- **BUILD_INSTRUCTIONS.txt** - Complete firmware build guide (4 methods)
- **FIX_COMPILER_PATH.txt** - Compiler installation troubleshooting
- **Modifications.txt** - Change log for firmware modifications
- **20D0478_20201208_1146.pdf** - Hardware schematic/datasheet

#### Map Files (.map, .cof, .mcs, .mcw)
- Debug symbols and memory mapping information

### Hardware Features
- **UART Communication:** 115200 bps with Raspberry Pi
- **GPIO Control Lines:**
  - RB6 (RASP_IN_PIC) - Pi status signal
  - RB5 (INT_PIC) - Interrupt control
  - RB7 (SHD_PIC) - Shutdown signal
- **I2C LCD Display:** Real-time status and error messages
- **Mechanical Control:** Stepper motors, sensors, solenoids

### Firmware Modifications
✅ Removed vacuum testing mechanism checks  
✅ Added button-based error clearing (SW_2/SW_3)  
✅ Enhanced error recovery (no infinite loops)  
✅ Integrated UART protocol for QR validation

---

## 📁 SCANNER/ - Legacy Scanner Application

**Size:** 1.1 MB  
**Language:** Python 3  
**Platform:** Raspberry Pi (Linux)  
**Purpose:** Original standalone QR scanner with database backend

### Key Components

#### Core Application
- **matrixux.py** - Main GUI application (PyQt5/Tkinter)
- **logic.py** - QR validation and batch processing logic
- **matrix.py** - Matrix/grid-based scanning interface
- **scannerclient.py** - Scanner hardware client interface
- **duplicate_tracker.py** - Duplicate QR detection system
- **settings.py** - Configuration settings module
- **mxsr_client_api.py** - API client for external systems

#### UI Definition
- **matrixux.ui** - Qt Designer UI definition file
- **settings.ui** - Settings dialog UI definition

#### Configuration
- **location.json** - Site/facility location configuration
- **crlf.json** - Line ending configuration
- **matrix.txt** - Matrix layout template
- **help.txt** - User help documentation

#### Databases
- **scanner.db** - SQLite database for scan records
- **scanner.dbbk** - Database backup

#### Scripts
- **Login.sh / LoginScreen.sh** - User authentication scripts
- **keyboard.sh** - Virtual keyboard launcher
- **root_init.sh** - System initialization (root permissions)
- **run.sh** - Application startup script

#### Logs & Results
- **Acc.csv** - Accepted scans log
- **Rej.csv** - Rejected scans log
- **mxsr_client_api.log** - API communication log
- **LOGS/** - Historical log directory
- **Batch_Setup_Logs/** - Batch configuration logs

#### Testing
- **test_batch_setup.py** - Batch setup unit tests
- **matrix_windows_test.py** - Windows compatibility testing
- **WINDOWS_TEST/** - Windows-specific test resources
  - **DISPLAY_COMPARISON.md** - Cross-platform display testing

#### C Integration (C_APPS/)
- **cat** - Native utility binary (Linux)
- C-based helper applications for hardware access

### Features
- Database-backed scan logging
- Batch validation and tracking
- Duplicate detection
- Multi-user login system
- Export to CSV
- External API integration

---

## 📁 python/ - Modern Batch Scanning Application

**Size:** 556 KB  
**Language:** Python 3  
**Platform:** Raspberry Pi with GPIO, UART  
**Purpose:** Integrated batch scanning with ACTJv20 firmware support

### Key Components

#### Main Application
- **main.py** - Primary GUI application with GPIO integration
- **jig.py** - JIG hardware abstraction
- **layout.py** - Tkinter GUI layout definitions
- **logic.py** - Batch validation and QR processing

#### ACTJv20 Integration (NEW!)
- **actj_legacy_integration.py** - Main integration module
- **actj_uart_protocol.py** - UART communication protocol
- **actj_integration.py** - Command/response definitions
- **actj_lcd_integration.py** - LCD display integration (future)

#### Hardware Control
- **hardware.py** - GPIO controller abstraction
- **plc_firmware.py** - PLC/firmware interface
- **lcd_display.py** - LCD display driver

#### Configuration
- **config.py** - Configuration file loader
- **settings.ini** - Main hardware configuration
- **settings_actj.ini** - ACTJv20-specific settings

#### Utilities
- **duplicate_tracker.py** - Duplicate detection (shared with SCANNER)
- **diagnose_scanner.py** - Hardware diagnostics tool
- **log_viewer.py** - Log file viewer utility

#### Testing
- **test_gpio.py** - GPIO functionality tests
- **test_uart.py** - UART communication tests
- **test_integration.py** - Full integration tests

#### Data Storage
- **scan_state.db** - SQLite database for scan state
- **batch_logs/** - Per-batch log files
- **__pycache__/** - Python bytecode cache

#### Documentation
- **DEPLOYMENT_GUIDE.md** - Complete deployment instructions
  - Hardware connection diagrams
  - UART configuration (115200 bps)
  - GPIO pin mapping
  - Installation procedures
  - Troubleshooting guide
  - Communication protocol specification

### Integration Architecture

```
┌─────────────────────┐    UART     ┌─────────────────────┐
│   Raspberry Pi      │◄──────────►│  PIC18F4550         │
│   main.py           │   115200    │  ACTJv20(RJSR)     │
│   + Integration     │   bps       │  Firmware           │
└─────────────────────┘             └─────────────────────┘
         │                                    │
         │ GPIO Control Lines                 │
         ├── GPIO 18 → RASP_IN_PIC (RB6)     │
         ├── GPIO 24 → INT_PIC (RB5)         │
         └── GPIO 25 → SHD_PIC (RB7)         │
```

### Communication Protocol

**PIC → Pi Commands:**
- `20` - QR scan request (with retry)
- `19` - Final QR scan attempt
- `0` - Stop/reset command

**Pi → PIC Responses:**
- `A` - Accept/Pass (GREEN light)
- `R` - Reject/Fail (RED light)
- `D` - Duplicate (YELLOW light)
- `S` - Scanner error
- `Q` - No QR detected
- `L` - Length error

### Features
- Real-time GPIO communication with PIC firmware
- UART-based command/response protocol
- Batch validation with mould range checking
- Duplicate detection across scans
- CSV logging per batch
- Error recovery and diagnostics
- Configuration-driven hardware setup

---

## 🔄 System Integration

### Two Operating Modes

#### 1. Standalone Mode (SCANNER)
- Independent QR scanning station
- Database-backed logging
- Manual batch management
- Suitable for QC stations without automation

#### 2. Integrated Mode (python + ACTJv20)
- Automated cartridge advancement
- PIC firmware controls mechanics
- Raspberry Pi validates QR codes
- Real-time hardware handshaking
- Production-ready automation

### Data Flow

```
User Input (Batch Config)
         ↓
    main.py (GUI)
         ↓
    logic.py (Validation)
         ↓
    UART → PIC Firmware
         ↓
    Mechanical Action
         ↓
    CSV Logs / Database
```

---

## 🛠️ Development Tools

### Required Software

**For Firmware (ACTJv20):**
- MPLAB X IDE or legacy MPLAB IDE
- HI-TECH PICC-18 or XC8 Compiler
- PIC programmer (PICkit 3/4, ICD3, etc.)

**For Python Applications:**
- Python 3.7+
- PyQt5 or Tkinter
- pyserial (UART communication)
- RPi.GPIO (Raspberry Pi GPIO)
- SQLite3

**Operating System:**
- Raspberry Pi OS (Raspbian) - recommended
- Ubuntu/Debian Linux - supported
- Windows - partial support (SCANNER only)

---

## 📊 File Statistics

| Category | Count | Notes |
|----------|-------|-------|
| Python Files | 28 | Application logic, GUI, hardware control |
| C Source Files | 14 | Embedded firmware |
| Header Files | 14 | C/C++ headers |
| Hex Files | 5 | Compiled firmware binaries |
| Documentation | 8 | MD, TXT guides |
| Configuration | 6+ | INI, JSON, UI files |
| Databases | 3 | SQLite, backups |
| Scripts | 6+ | Shell scripts |

---

## 🚀 Quick Start

### Deploy SCANNER (Standalone)
```bash
cd SCANNER
./run.sh
```

### Deploy Python App (Integrated)
```bash
cd python
python3 main.py
```

### Build Firmware
```bash
cd ACTJv20\(RJSR\)
# Use MPLAB IDE or see BUILD_INSTRUCTIONS.txt
```

---

## 📖 Key Documentation Files

1. **python/DEPLOYMENT_GUIDE.md** - Complete integration guide
2. **ACTJv20(RJSR)/BUILD_INSTRUCTIONS.txt** - Firmware build guide
3. **ACTJv20(RJSR)/Modifications.txt** - Firmware change log
4. **SCANNER/help.txt** - SCANNER app user guide

---

## 🎯 Use Cases

### Manufacturing QC Station
- Automated cartridge testing
- Pass/fail sorting based on QR validation
- Batch tracking and traceability
- Real-time error detection

### Laboratory Testing
- Sample validation
- Duplicate prevention
- Audit trail logging
- Batch lineage tracking

### Production Line Integration
- Inline quality control
- Automated reject handling
- Statistical process control data
- Shift/batch reporting

---

## ⚙️ Technical Specifications

### Hardware Requirements
- **Raspberry Pi:** Model 3B+ or newer
- **Microcontroller:** PIC18F4550 @ 48MHz
- **QR Scanner:** USB or serial barcode reader
- **Display:** I2C LCD 16x2 or 20x4
- **Power:** 5V regulated supply

### Communication Specs
- **UART:** 115200 bps, 8N1, no flow control
- **GPIO:** 3.3V logic levels
- **I2C:** 100 kHz (LCD)

### Performance
- **Scan Rate:** ~2-3 seconds per cartridge
- **Validation Time:** <100ms
- **Error Recovery:** <2 seconds
- **Batch Capacity:** Unlimited (database-backed)

---

## 📝 Notes

- The repository contains both legacy (SCANNER) and modern (python) implementations
- ACTJv20(RJSR) firmware is production-ready with error recovery
- UART protocol is well-documented and tested
- Cross-platform support varies by component
- Build artifacts (.o, .err) are committed (consider adding .gitignore)

---

## 🔗 Component Dependencies

```
python/main.py
    ├── logic.py (batch validation)
    ├── hardware.py (GPIO control)
    ├── actj_legacy_integration.py
    │   ├── actj_uart_protocol.py (UART)
    │   └── actj_integration.py (commands)
    └── config.py (settings loader)

ACTJv20(RJSR)/Main_PCR.c
    ├── Functions.c (utilities)
    ├── service.c (mechanics)
    ├── SBC_Rpi.c (UART protocol)
    ├── LCD_module.c (display)
    └── uart_module.c (hardware)
```

---

**Last Updated:** 2025-11-07  
**Repository:** kaushik565/JIG  
**Branch:** copilot/discuss-folder-structure
