# network-traffic-analysis

## Quick links
- **Full report:** [`report.md`](./report.md)
- **Wireshark filters:** [`filters.md`](./filters.md)
- **Timeline (SOC ticket style):** [`timeline.md`](./timeline.md)
- **Evidence screenshots:** [`/screenshots`](./screenshots)

# SOC Network Traffic Analysis (Home Lab)

This project is part of my personal SOC portfolio where I practice real world blue team workflows: capturing traffic, identifying suspicious patterns, documenting findings and writing detection ideas.

## What this case study covers
In this investigation I captured internal network traffic between:
- **Ubuntu (SOC workstation):** `192.168.1.3`
- **Windows 10 (endpoint):** `192.168.1.4`

I then analyzed the capture in Wireshark and documented evidence of reconnaissance (TCP SYN scanning) and defensive detection recommendations.

## Evidence included
- `pcaps/recon-run02.pcap` – packet capture (Run 02)
- `screenshots/` – Wireshark evidence screenshots:
  - SYN scan pattern
  - Multiple destination ports
  - TCP conversations summary
  - Protocol hierarchy
  - TCP stream example
- `report.md` – Investigation write up
- `filters.md` – reusable Wireshark filter cheat sheet

## Key findings (Run 02 – Reconnaissance)
- High volume **TCP SYN** activity from `192.168.1.3` to `192.168.1.4` across many destination ports
- Closed port behaviour observed via **RST** responses
- Services on the Windows endpoint identified during enumeration:
  - `135/tcp` (MSRPC)
  - `139/tcp` (NetBIOS-SSN)
  - `445/tcp` (SMB)
  - additional dynamic RPC ports (ephemeral)

## Detection logic
If this happened in a real environment, I’d want monitoring that detects:
- One source contacting **many ports** on the same host in a short window
- High SYN-to-ACK ratio
- Many short lived TCP attempts with low bytes transferred

I included detection logic ideas in `report.md`.

## Skills demonstrated
- Packet capture scoping (`tcpdump`)
- Wireshark analysis and filtering
- TCP handshake (SYN / SYN-ACK / RST patterns)
- Reconnaissance identification and documentation
- Report reporting
- Defensive recommendations and detection thinking

## Files
- **Report:** [`report.md`](./report.md)
- **Filters:** [`filters.md`](./filters.md)
