# N3FJP → OpenHamClock / Time Mapper UHD Bridge

This project provides a lightweight Python bridge that connects to the **N3FJP Logger TCP API**
and forwards newly logged QSOs to:

- **OpenHamClock** via HTTP (map overlay ingest)
- **Time Mapper UHD** via UDP (logger feed)

> This bridge was originally created to feed **Time Mapper UHD** via UDP, and now also supports
> OpenHamClock via HTTP.

---

## Features

- Monitors N3FJP Entry fields (callsign / grid)
- Detects log/enter events
- Reads band, mode, and frequency
- Sends QSOs to OpenHamClock (optional)
- Sends UDP logger packets to Time Mapper UHD (optional)
- Designed for LAN use (bridge and targets on the same network)

---

## Requirements

- Windows 10 / 11
- Python 3.9+
- N3FJP Logger with TCP API enabled
- OpenHamClock (optional)
- Time Mapper UHD (optional)

---

## Quick Start

1. Copy `config.json.example` → `config.json`
2. Edit values for your station and network
3. Run manually:

   ```bash
   python n3fjp_to_timemapper_udp.py
