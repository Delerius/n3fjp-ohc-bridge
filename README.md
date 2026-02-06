# N3FJP → OpenHamClock / Time Mapper UHD Bridge

This project provides a lightweight Python bridge that connects to the **N3FJP Logger TCP API**
and forwards newly logged QSOs to:

- **OpenHamClock** via HTTP (map overlay ingest)
- **Time Mapper UHD** via UDP (logger feed)

## Features

- Monitors N3FJP Entry fields (callsign / grid)
- Detects log/enter events
- Reads band, mode, and frequency
- Sends QSOs to OpenHamClock (optional)
- Sends UDP logger packets to Time Mapper UHD (optional)
- Designed for LAN use (bridge and targets on same network)

## Requirements

- Windows 10 / 11
- Python 3.9+
- N3FJP Logger with TCP API enabled
- OpenHamClock (optional)
- Time Mapper UHD (optional)

## Quick Start

1. Copy `config.json.example` → `config.json`
2. Edit values for your station and network
3. Run manually:
   python n3fjp_to_timemapper_udp.py
4. (Optional) Use the included installer scripts to run hidden at startup

See INSTALL_INSTRUCTIONS.txt for detailed setup steps.

OpenHamClock Integration

This bridge supports an OpenHamClock overlay that displays recently logged QSOs on the map.
The corresponding OpenHamClock changes are provided separately.

Credits

Time Mapper UHD — Original motivation for this bridge via UDP logger feed

OpenHamClock — HTTP ingest endpoint and map overlay support

N3FJP Logger — Source of QSO data via TCP API
