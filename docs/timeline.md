# Timeline – LAB-NT-RECON-02 (SOC Ticket Style)

> **Case ID:** LAB-NT-RECON-02  
> **Source:** 192.168.1.3 (Ubuntu SOC workstation)  
> **Target:** 192.168.1.4 (Windows endpoint)  
> **Date:** 26 February 2026 (GMT)

## Key events
- **21:02** — TCP SYN scan activity begins from `192.168.1.3` to `192.168.1.4` (multiple destination ports observed).
- **21:02–21:03** — High volume of short-lived TCP attempts; closed ports respond with **RST**; scan behavior consistent with reconnaissance/service discovery.
- **21:03** — Enumeration confirms exposed services on the Windows endpoint (MSRPC/NetBIOS/SMB and dynamic RPC ports).

## Observations
- SYN-dominant traffic pattern across numerous destination ports
- Minimal data transfer and incomplete handshakes in many streams
- Open services identified: **135/tcp**, **139/tcp**, **445/tcp** (+ dynamic RPC ports)

## Conclusion
Activity is consistent with **internal reconnaissance (network service discovery)**. In a production environment this would warrant validation of authorization (approved scanner vs. unauthorized host) and potential escalation, with monitoring tuned to detect rapid multi-port SYN activity and high SYN-to-ACK ratios.