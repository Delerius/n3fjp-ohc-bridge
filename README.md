N3FJP → OpenHamClock / Time Mapper UHD Bridge

This project provides a lightweight Python bridge that connects to the N3FJP Logger TCP API
and forwards newly logged QSOs to:

OpenHamClock via HTTP (map overlay ingest)

Time Mapper UHD via UDP (logger feed)

This bridge was originally created to feed Time Mapper UHD via UDP, and now also supports OpenHamClock via HTTP.

Features

Monitors N3FJP Entry fields (callsign / grid)

Detects log (ENTER) events

Reads band, mode, and frequency from N3FJP

Sends QSOs to OpenHamClock (optional)

Sends UDP logger packets to Time Mapper UHD

Designed for LAN use (bridge and targets on the same network)

Requirements

Windows 10 / 11

Python 3.9+

N3FJP Logger with TCP API enabled

OpenHamClock (optional)

Time Mapper UHD (optional)

Quick Start

Copy config.json.example → config.json

Edit values for your station and network

Run manually:

python n3fjp_to_timemapper_udp.py

Output Modes (Important)

The bridge supports two independent output paths.

1️⃣ UDP Output (Always Enabled)

The bridge sends a minimal N1MM-style <contactinfo> packet to:

UDP_DEST_IP : UDP_DEST_PORT


This packet includes:

Callsign

Band

Frequency

Mode

My Call

⚠️ Important:
The UDP payload does not include DX grid information.

If your OpenHamClock instance relies on grid or location data to plot QSOs, UDP alone may not be sufficient.

2️⃣ Optional HTTP POST to OpenHamClock

If your config.json contains:

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

Note: UDP output is still sent even when HTTP mode is enabled. Enabling HTTP allows OpenHamClock to receive additional fields (such as dx_grid when available) that may be required for plotting in some setups.

Troubleshooting: QSOs Not Plotting

If:

Frequency is non-zero in bridge.log

UDP packets are visible on the Pi (via tcpdump)

The "Logged QSOs" layer is enabled in OpenHamClock

But nothing plots:

Verify "ENABLE_OHC_HTTP": true in config.json

Restart the bridge

Look for this line in bridge.log:

[OHC] POST ok -> http://<OHC_BASE_URL>/api/n3fjp/qso

If you see an HTTP error instead, that message will indicate what needs adjustment.

Optional (Highly Recommended Addition)

I would also add this small clarification section — this prevents future CAT confusion:

Important: N3FJP Must Have Live Rig Frequency

The bridge reads live band/mode/frequency via N3FJP’s TCP API.

If N3FJP does not have rig control configured, the API may return:

FREQ=0.0


When this occurs, the bridge will skip sending the QSO.

Ensure N3FJP has active CAT/rig control configured at log time.

Copy config.json.example → config.json

Choose one of the example configs:
- config.ohc.example.json (recommended for OpenHamClock)
- config.udp-only.example.json (Time Mapper only)

Copy the file you want to use to:
config.json
