# Network Traffic Analysis

## Overview

This project documents the investigation of controlled internal reconnaissance traffic captured in an isolated VirtualBox lab. Wireshark was used to identify a TCP SYN scanning pattern from an Ubuntu system to a Windows 10 endpoint and to record the supporting packet evidence.

## Case Summary

| Field | Value |
|---|---|
| Case ID | `LAB-NT-RECON-02` |
| Date | 26 February 2026 |
| Source | `192.168.1.3` |
| Target | `192.168.1.4` |
| Primary finding | Multi-port TCP SYN activity consistent with network service discovery |
| MITRE ATT&CK | T1046 — Network Service Discovery |

## Documentation

- [`docs/report.md`](docs/report.md) — complete investigation report
- [`docs/timeline.md`](docs/timeline.md) — event timeline
- [`docs/architecture.md`](docs/architecture.md) — lab topology
- [`docs/wireshark-filters.md`](docs/wireshark-filters.md) — filters used during analysis
- [`docs/mitre-attack-mapping.md`](docs/mitre-attack-mapping.md) — ATT&CK mapping and boundaries

## Evidence

- [`pcaps/baseline-run01.pcap`](pcaps/baseline-run01.pcap) — baseline capture
- [`pcaps/recon-run02.pcap`](pcaps/recon-run02.pcap) — reconnaissance capture
- [`screenshots/`](screenshots/) — Wireshark evidence

## Key Findings

- high-volume TCP SYN traffic from one source to many destination ports
- numerous short-lived connection attempts
- RST responses from closed ports
- open Windows services including TCP 135, 139 and 445
- no evidence in the capture of authentication, exploitation or file transfer

## Detection Opportunity

A useful monitoring rule would identify one source contacting many destination ports on the same host within a short time window, supported by a high SYN-to-successful-connection ratio.

## Security Scope

All testing was performed inside a controlled, non-production lab.
