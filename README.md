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

Output Modes: UDP vs HTTP (Important)

The bridge supports two independent output paths:
1) UDP (Always Enabled)
The bridge sends a minimal N1MM-style <contactinfo> packet to:
UDP_DEST_IP : UDP_DEST_PORT

This packet includes:
Callsign
Band
Frequency
Mode
My Call

Note: The UDP payload does not include DX grid information.
If your OpenHamClock instance relies on grid or location data to plot QSOs, UDP alone may not be sufficient.

2) Optional HTTP POST to OpenHamClock
If:

"ENABLE_OHC_HTTP": true

The bridge will also POST structured JSON directly to:
http://<OHC_BASE_URL>/api/n3fjp/qso

This payload includes:
dx_call
band_mhz
freq_khz
mode
dx_grid (when available)
de_call (if configured)

If QSOs are not plotting even though UDP packets are arriving, enabling HTTP mode is recommended.

Troubleshooting: QSOs Not Plotting

If:
Frequency is non-zero
UDP packets are visible on the Pi
The Logged QSOs layer is enabled

But nothing plots:

Verify "ENABLE_OHC_HTTP": true

Restart the bridge

Look for [OHC] POST ok in bridge.log
