# iRemote — USB IR Blaster (Python Backend)

Universal IR blaster interface with a Python backend for USB communication.  
Works with **any browser** — no WebUSB/WebHID required.

## Supported Devices

| Device | VID | PID | Backend |
|--------|-----|-----|---------|
| **Tiqiaa** | 0x10C4 | 0x8468 | Python `hidapi` |
| **Ocrustar / ElkSmart** | 0x045C | 0x02AA + others | Python `pyusb` |

## Quick Start

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Windows only (Ocrustar): install WinUSB driver via Zadig
#    Download from https://zadig.akeo.ie/
#    Select SMART device → Replace with WinUSB
#    (Tiqiaa needs no driver changes on any OS)

# 3. Run the server
python server.py

# 4. Open in any browser
#    http://localhost:5000
```

## Architecture

```
┌──────────────────┐     WebSocket      ┌──────────────────┐
│   Browser (any)  │ ◄──────────────── │  Python Server    │
│   index.html     │    Socket.IO       │  server.py        │
│   - UI/Remote    │                    │  - pyusb (Ocrustar)│
│   - IR protocols │                    │  - hidapi (Tiqiaa) │
│   - File import  │                    │  - Huffman/LEB128  │
│   - IRDB search  │                    │  - Learn/Transmit  │
└──────────────────┘                    └──────────────────┘
```

**Frontend** (index.html): UI, IR protocol encoders (NEC, Samsung, Sony, RC5),
file import (Flipper .ir, IRDB .csv), IRDB GitHub search, saved remotes.

**Backend** (server.py): USB device management, Ocrustar protocol engine
(pulse compression, Huffman encoding, Java PriorityQueue emulation, LEB128
with ÷16 prescaling, byte mangling, framing), Tiqiaa HID protocol, learn mode.

## Features

- **Universal TV remote** — multi-blast Samsung + LG + Sony + RC5
- **AC / Fan remotes** — common NEC codes
- **IR learning** — capture signals from physical remotes
- **File import** — Flipper Zero .ir, IRDB .csv, raw pulse data
- **IRDB search** — browse probonopd/irdb directly from the UI
- **Saved remotes** — persist custom remotes in browser localStorage
- **Protocol sender** — manual NEC/Samsung32/RC5/Sony with address + command
- **Full logging** — see every USB frame in the log tab

## Credits

- Protocol reverse engineering: [deadboy18](https://github.com/deadboy18)
- [Ocrustar-USB-IR](https://github.com/deadboy18/Ocrustar-USB-IR)
- [Tiqiaa-USB-IR-Windows](https://github.com/deadboy18/Tiqiaa-USB-IR-Windows)
- IR Database: [probonopd/irdb](https://github.com/probonopd/irdb)
