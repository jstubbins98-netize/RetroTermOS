# RetroTermOS

A bare-metal terminal operating system for Raspberry Pi 4B and Pi 5, featuring a custom kernel with a retro computing aesthetic.

```
  ____      _            _____                    ___  ____  
 |  _ \ ___| |_ _ __ ___|_   _|__ _ __ _ __ ___  / _ \/ ___| 
 | |_) / _ \ __| '__/ _ \ | |/ _ \ '__| '_ ` _ \| | | \___ \ 
 |  _ <  __/ |_| | | (_) || |  __/ |  | | | | | | |_| |___) |
 |_| \_\___|\__|_|  \___/ |_|\___|_|  |_| |_| |_|\___/|____/ 
```

---

## Table of Contents

1. Overview
2. Features
3. Hardware Requirements
4. Quick Start
5. Building from Source
6. Shell Commands
7. BASIC Interpreter
8. USB Storage
9. Cassette Tape Interface
10. Terminal Chat
11. HDMI Display
12. Audio Feedback
13. Hardware Verification
14. Copier GUI Application
15. Project Structure
16. Technical Specifications
17. Troubleshooting
18. License

---

## Overview

RetroTermOS is a completely custom operating system written from scratch for the Raspberry Pi 4B and Pi 5. Unlike Linux distributions, this is a true bare-metal kernel that takes complete control of the hardware, providing:

- A nostalgic 1970s/1980s computing experience
- Direct hardware access without operating system overhead
- Educational value for understanding low-level systems programming
- Unique storage options including cassette tape support

The system boots directly into a command-line shell with amber-on-black text, reminiscent of classic terminal computers.

---

## Features

| Feature | Description |
|---------|-------------|
| Custom Kernel | Bare-metal kernel written in C and ARM64 Assembly |
| Terminal Shell | Interactive command-line interface |
| BASIC Interpreter | Built-in BASIC programming language |
| USB Storage | Read/write support for USB flash drives |
| Cassette Tape | FSK-encoded audio storage using standard cassette tapes |
| Terminal Chat | LAN-based chat using UDP broadcast |
| Dual Display | Simultaneous UART serial and HDMI output |
| Audio Feedback | Beep tones for system events |
| Hardware Check | Automatic Pi 4B/5 hardware verification |
| Platform Abstraction | Runtime detection of Pi 4B vs Pi 5 |
| Copier GUI | Cross-platform companion app for SD card setup |

---

## Hardware Requirements

### Required Components

| Component | Specification |
|-----------|---------------|
| Computer | Raspberry Pi 4B or Pi 5 (4GB or 8GB RAM) |
| Storage | MicroSD card (8GB minimum, 16GB+ recommended) |
| Display | HDMI monitor (any resolution) |
| Keyboard | USB keyboard |
| Power | Official Raspberry Pi 5 power supply (27W USB-C) |

### Optional Components

| Component | Purpose |
|-----------|---------|
| USB Flash Drive | Additional program/data storage |
| Cassette Recorder | Tape-based storage (requires 3.5mm audio cables) |
| Ethernet Cable | Terminal Chat networking |
| USB-UART Adapter | Serial console access |
| Speaker/Buzzer | Audio feedback (connect to GPIO18) |

---

## Quick Start

### Automated Setup (Linux/macOS)

```bash
# Clone the repository
git clone <repository-url> retrotermos
cd retrotermos

# Run the automated boot script
./scripts/boot.sh
```

The script will:
1. Detect your platform (Linux, macOS, or WSL)
2. Install required dependencies and ARM64 cross-compiler
3. Build the kernel
4. Download Raspberry Pi firmware (for both Pi 4 and Pi 5)
5. Create a `copy/` directory with all files ready to copy to SD card
6. Build the Copier GUI application (auto-installs Qt 6 and CMake)
7. Optionally launch Copier to copy files to your SD card

**Options:**
```bash
./scripts/boot.sh --skip-copier  # Skip building the Copier GUI
./scripts/boot.sh --help         # Show usage information
```

### Manual Setup

1. Build the kernel (see [Building from Source](#building-from-source))
2. Format a MicroSD card as FAT32
3. Copy all files from the `copy/` directory to your SD card root
4. Rename the appropriate config file:
   - For Pi 4: rename `config-pi4.txt` to `config.txt`
   - For Pi 5: rename `config-pi5.txt` to `config.txt`
5. Insert SD card into your Pi and power on

**Files in the `copy/` directory after running boot.sh:**
| File | Description |
|------|-------------|
| `kernel8.img` | RetroTermOS kernel |
| `bootcode.bin` | GPU bootloader |
| `start4.elf` | GPU firmware |
| `fixup4.dat` | Memory configuration |
| `bcm2711-rpi-4-b.dtb` | Device tree for Pi 4B |
| `bcm2712-rpi-5-b.dtb` | Device tree for Pi 5 |
| `config-pi4.txt` | Boot config for Pi 4B |
| `config-pi5.txt` | Boot config for Pi 5 |

---

## Building from Source

### Prerequisites

- Linux host system (Ubuntu 22.04+, Debian, Fedora, or Arch)
- ARM64 cross-compiler toolchain
- Git, Make, and standard build tools

### Step-by-Step Build

```bash
# Install dependencies (Ubuntu/Debian)
sudo apt update
sudo apt install -y build-essential git wget curl make parted dosfstools

# Install ARM64 cross-compiler
wget https://developer.arm.com/-/media/Files/downloads/gnu/13.3.rel1/binrel/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-elf.tar.xz
sudo mkdir -p /opt/arm-toolchain
sudo tar -xf arm-gnu-toolchain-*.tar.xz -C /opt/arm-toolchain --strip-components=1
export PATH=/opt/arm-toolchain/bin:$PATH

# Build the kernel
cd retrotermos/src
make clean
make

# Output: kernel8.img
```

### Build Options

```bash
make              # Build kernel8.img
make clean        # Remove build artifacts
make disasm       # Generate disassembly listing
make help         # Show build options
```

---

## Shell Commands

### File Management

| Command | Alias | Description |
|---------|-------|-------------|
| `ls` | `dir` | List directory contents |
| `cd <path>` | - | Change current directory |
| `cat <file>` | `type` | Display file contents |

### Storage Commands

| Command | Description |
|---------|-------------|
| `mount` | Mount USB drive |
| `umount` | Safely unmount USB drive |
| `cwrite <name>` | Write data to cassette tape |
| `claunch <name>` | Launch/load from cassette tape |

### System Commands

| Command | Description |
|---------|-------------|
| `help` or `?` | Display available commands |
| `clear` or `cls` | Clear the screen |
| `echo <text>` | Print text to screen |
| `info` | Display system information |
| `mem` | Show memory information |
| `reboot` | Restart the system |
| `shutdown` or `shut down` | Shut down the computer |

### Applications

| Command | Description |
|---------|-------------|
| `basic` | Enter BASIC interpreter |
| `chat` | Start Terminal Chat |

### Network Commands

| Command | Description |
|---------|-------------|
| `netinit` | Initialize ethernet networking |
| `ifconfig [ip]` | Show or set network configuration |
| `nick <name>` | Set chat nickname |

---

## BASIC Interpreter

RetroTermOS includes a built-in BASIC programming language interpreter.

### Entering BASIC

```
retro/> basic

Entering BASIC interpreter...
Type 'EXIT' to return to shell.

>
```

### BASIC Commands

| Command | Description |
|---------|-------------|
| `RUN` | Execute the current program |
| `LIST` | Display program listing |
| `NEW` | Clear program from memory |
| `WRITE "name"` | Write program to USB storage |
| `LAUNCH "name"` | Launch program from USB storage |
| `CWRITE "name"` | Write program to cassette tape |
| `CLAUNCH "name"` | Launch program from cassette tape |
| `EXIT` or `QUIT` | Return to shell |

### BASIC Statements

| Statement | Example | Description |
|-----------|---------|-------------|
| `PRINT` | `PRINT "Hello"` | Display output |
| `INPUT` | `INPUT "Name?"; N$` | Read user input |
| `LET` | `LET X = 10` | Assign value (LET is optional) |
| `IF-THEN` | `IF X>5 THEN PRINT "BIG"` | Conditional execution |
| `GOTO` | `GOTO 100` | Jump to line number |
| `FOR-NEXT` | `FOR I=1 TO 10` | Counting loop |
| `GOSUB` | `GOSUB 500` | Call subroutine |
| `RETURN` | `RETURN` | Return from GOSUB |
| `REM` | `REM Comment` | Program comment |
| `END` | `END` | End program execution |
| `CLS` | `CLS` | Clear screen |

### Example Program

```basic
>10 REM HELLO WORLD PROGRAM
>20 PRINT "HELLO, WORLD!"
>30 INPUT "WHAT IS YOUR NAME"; N$
>40 PRINT "NICE TO MEET YOU, "; N$
>50 END
>RUN
```

---

## USB Storage

RetroTermOS supports USB flash drives for storing programs and data.

### Mounting a USB Drive

```
retro/> mount
Mounting USB drive 0...
Drive mounted successfully.

retro/> ls /usb
programs/
data/
readme.txt
```

### Unmounting

Always unmount before removing the drive:

```
retro/> umount
Drive unmounted.
```

### Saving Programs (from BASIC)

```basic
>WRITE "USB:MYPROGRAM"
Saving to USB...
Save complete!
```

### Loading Programs (from BASIC)

```basic
>LAUNCH "USB:MYPROGRAM"
Loading from USB...
Load complete!
```

---

## Cassette Tape Interface

RetroTermOS supports saving and loading data using standard audio cassette tapes, just like computers from the 1970s and 1980s.

### How It Works

Data is encoded using Frequency Shift Keying (FSK):
- Binary 0: 1200 Hz (4 cycles)
- Binary 1: 2400 Hz (8 cycles)
- Baud rate: 300 bps (~30 bytes/second)

### Required Equipment

1. Standard cassette tape recorder with LINE IN and LINE OUT jacks
2. Two 3.5mm audio cables
3. Blank audio cassettes (C-60 or C-90 recommended)

### Connections

| Raspberry Pi 5 | Direction | Cassette Recorder |
|----------------|:---------:|-------------------|
| Audio OUT (GPIO17) | ---> | LINE IN (or MIC) |
| Audio IN (GPIO27) | <--- | LINE OUT (or EARPHONE) |

### Writing to Tape (Shell)

```
retro/> cwrite mydata
Writing to cassette tape...
Sending leader tone...
Sending data: mydata
████████████████████ 100%
Write complete!
Press STOP on your cassette recorder.
```

### Writing to Tape (BASIC)

```basic
>CWRITE "MYPROG"
```

### Launching from Tape (Shell)

```
retro/> claunch mydata
Launching from cassette tape...
Searching...
Found: mydata
Receiving data...
████████████████████ 100%
Launch complete!
```

### Launching from Tape (BASIC)

```basic
>CLAUNCH "MYPROG"
```

### Troubleshooting Tape Operations

| Problem | Solution |
|---------|----------|
| Sync failed | Adjust volume, clean tape heads |
| Checksum error | Use higher quality tape, re-record |
| Data not found | Rewind tape, check tape index |

---

## Terminal Chat

Terminal Chat allows real-time text communication with other RetroTermOS users on your local network.

### Network Setup

```
retro/> netinit
Initializing network...
Network initialized.
Link: UP (1000 Mbps)

retro/> ifconfig 192.168.1.100
IP address set to 192.168.1.100
```

### Setting Your Nickname

```
retro/> nick JOHNDOE
Nickname set to: JOHNDOE
```

### Starting Chat

```
retro/> chat

╔══════════════════════════════════════════════════════════════╗
║                    TERMINAL CHAT v1.0                        ║
║          Type /help for commands, /quit to exit             ║
╚══════════════════════════════════════════════════════════════╝

[12:34:56] *** JOHNDOE has joined the chat ***
> Hello everyone!
[12:35:02] <JOHNDOE> Hello everyone!
```

### Chat Commands

| Command | Description |
|---------|-------------|
| `/help` | Display command help |
| `/nick NAME` | Change nickname |
| `/me ACTION` | Send action message |
| `/who` | List active users |
| `/ip` | Show your IP address |
| `/quit` or `/exit` | Exit chat |

### Technical Details

- Protocol: UDP broadcast
- Port: 5555
- Scope: Local network only (not routable)
- Security: Cleartext (suitable for trusted LANs only)

---

## HDMI Display

RetroTermOS outputs to both UART serial console and HDMI display simultaneously.

### Display Specifications

| Setting | Value |
|---------|-------|
| Resolution | 1024x768 |
| Color Depth | 32-bit |
| Font | 8x8 bitmap |
| Theme | Amber on black (retro terminal style) |

### Implementation Details

- **Mailbox Driver**: GPU communication at 0x107C013880
- **Framebuffer**: Direct GPU memory access
- **Font Rendering**: MSB-first 8x8 bitmap font
- **Console Layer**: Unified UART + HDMI output

### Required config.txt Settings

```
arm_64bit=1
kernel=kernel8.img
enable_uart=1
gpu_mem=64
pciex4_reset=0
hdmi_group=2
hdmi_mode=87
hdmi_cvt=800 600 60 1 0 0 0
```

---

## Audio Feedback

The system provides audio feedback for various events using simple beep tones.

### Sound Events

| Event | Sound | Frequencies |
|-------|-------|-------------|
| Successful boot | Rising 3-tone melody | 440Hz, 660Hz, 880Hz |
| Unknown command | Two low beeps | 220Hz, 220Hz |
| Data error | Two low beeps | 220Hz, 220Hz |
| Successful operation | Two high beeps | 880Hz, 1760Hz |

### Hardware Setup

Connect a small speaker or piezo buzzer to:
- GPIO18 (physical pin 12)
- Ground (physical pin 6)

Optional: Add a simple RC low-pass filter for smoother audio.

---

## Hardware Verification

RetroTermOS verifies it's running on genuine Raspberry Pi hardware at boot.

### Verification Process

1. Queries board model via VideoCore mailbox
2. Reads hardware revision code
3. Decodes processor type (BCM2711 for Pi 4B, BCM2712 for Pi 5)
4. Selects correct peripheral base addresses via platform abstraction
5. Halts with error if verification fails

### Error Messages

| Error | Meaning |
|-------|---------|
| "Cannot communicate with VideoCore" | Not a Raspberry Pi |
| "Unknown hardware detected" | Unsupported Pi model (not Pi 4B or Pi 5) |

This prevents the OS from running on unsupported hardware and ensures driver compatibility.

---

## Copier GUI Application

RetroTermOS includes **Copier**, a cross-platform graphical companion application that simplifies copying boot files to your SD card.

### Features

| Feature | Description |
|---------|-------------|
| Device Detection | Automatically finds external SD cards and USB drives |
| Pi Model Selection | Choose between Raspberry Pi 4B or Pi 5 |
| One-Click Copy | Copies all required files with correct config.txt |
| Image Creation | Create SD card image files (.img) on macOS/Linux |
| Progress Tracking | Visual progress bar and status updates |

### Supported Platforms

| Platform | Copy to Device | Create Image |
|----------|:--------------:|:------------:|
| macOS    | Yes           | Yes          |
| Linux    | Yes           | Yes          |
| Windows  | Yes           | No           |

### Using Copier

After running `./scripts/boot.sh`, you'll be prompted to launch Copier:

```
Would you like to launch the Copier application now? (y/N):
```

Or launch manually:
```bash
# macOS
open copier/build/Copier.app

# Linux
./copier/build/Copier
```

### Workflow

1. Insert your SD card (formatted as FAT32)
2. Launch Copier
3. Select the SD card from the device list
4. Choose your Pi model (Pi 4B or Pi 5)
5. Click "Copy Files"
6. Eject and insert into your Raspberry Pi

---

## Project Structure

```
retrotermos/
├── README.md                # This file
├── build.md                 # Detailed build instructions
├── manual.md                # Complete user manual (1970s style)
├── basics of BASIC.md       # BASIC programming guide
├── docs/
│   └── index.html           # Documentation viewer
├── src/
│   ├── boot/
│   │   ├── boot.S           # ARM64 boot assembly
│   │   └── link.ld          # Linker script
│   ├── kernel/
│   │   ├── kernel.c         # Main kernel entry
│   │   └── kernel.h         # Kernel definitions
│   ├── drivers/
│   │   ├── uart.c           # Serial console
│   │   ├── gpio.c           # GPIO control
│   │   ├── timer.c          # System timer
│   │   ├── mailbox.c        # GPU mailbox
│   │   ├── framebuffer.c    # HDMI display
│   │   ├── console.c        # Unified console
│   │   ├── audio.c          # Audio beeps
│   │   ├── hwcheck.c        # Hardware verification
│   │   ├── platform.c       # Pi 4B/5 platform abstraction
│   │   └── usb/             # USB storage driver
│   ├── shell/
│   │   ├── shell.c          # Command shell
│   │   └── shell.h
│   ├── basic/
│   │   ├── basic.c          # BASIC interpreter
│   │   └── basic.h
│   ├── cassette/
│   │   ├── cassette.c       # Tape interface
│   │   └── cassette.h
│   ├── net/
│   │   ├── net.c            # Network stack
│   │   ├── genet.c          # Ethernet driver
│   │   └── *.h
│   ├── apps/
│   │   ├── chat.c           # Terminal Chat
│   │   └── chat.h
│   └── Makefile
├── scripts/
│   └── boot.sh              # Automated build script
├── copier/                  # Copier GUI application
│   ├── src/
│   │   ├── main.cpp         # Application entry point
│   │   ├── ui/              # Qt UI components
│   │   └── platform/        # Platform-specific code
│   ├── CMakeLists.txt       # CMake build configuration
│   └── README.md            # Copier documentation
├── copy/                    # Generated boot files (after build)
├── schematics/
│   └── cassette_interface.txt
└── boot/
    ├── config-pi4.txt       # Boot configuration for Pi 4B
    └── config-pi5.txt       # Boot configuration for Pi 5
```

---

## Technical Specifications

### System Architecture

| Component | Specification |
|-----------|---------------|
| Target CPU | ARM Cortex-A76 (AArch64) |
| Target SoC | Broadcom BCM2712 |
| Memory Model | Bare-metal (no MMU) |
| Boot Method | ARM64 stub → kernel8.img |

### Memory Map

| Address | Usage |
|---------|-------|
| 0x80000 | Kernel entry point |
| 0x107C000000 | RP1 peripheral base |
| 0x107C013880 | VideoCore mailbox |

### Performance

| Metric | Value |
|--------|-------|
| Boot time | < 2 seconds |
| Kernel size | < 100KB |
| RAM usage | < 1MB |

---

## Troubleshooting

### Boot Issues

| Symptom | Solution |
|---------|----------|
| No display output | Check HDMI cable, verify config.txt |
| Red LED only | SD card not readable, reformat as FAT32 |
| Rainbow screen | kernel8.img missing or corrupt |
| "Hardware check failed" | Only runs on Raspberry Pi 4B or Pi 5 |

### Display Issues

| Symptom | Solution |
|---------|----------|
| No HDMI output | Ensure gpu_mem=64 in config.txt |
| Garbled text | Check HDMI cable connection |
| Blank after boot | Verify framebuffer initialization |

### Storage Issues

| Symptom | Solution |
|---------|----------|
| USB not detected | Try different USB port |
| Mount failed | Check USB drive format (FAT32) |
| Tape errors | Adjust recorder volume, clean heads |

### Network Issues

| Symptom | Solution |
|---------|----------|
| No link | Check ethernet cable connection |
| Chat not working | Ensure both systems on same subnet |
| IP not set | Run ifconfig to set IP address |

---

## License

RetroTermOS is provided for educational and hobbyist use.

---

## Acknowledgments

- Raspberry Pi Foundation for excellent hardware documentation
- The bare-metal programming community
- Classic computer enthusiasts everywhere

---

*"Commandline for the home (again)" - Intelix Software Corporation*
