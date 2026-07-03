# Network Traffic Analysis Report

## Case Information

| Field | Details |
|---|---|
| Case ID | `LAB-NT-RECON-02` |
| Date | 26 February 2026 |
| Source host | `192.168.1.3` |
| Target host | `192.168.1.4` |
| Environment | Isolated VirtualBox network, `192.168.1.0/24` |
| Capture | `pcaps/recon-run02.pcap` |

## Executive Summary

A burst of TCP connection attempts was observed from `192.168.1.3` to multiple destination ports on `192.168.1.4`. Packet review showed a SYN-dominant pattern, short-lived attempts and reset responses from closed ports. The behaviour is consistent with controlled network service discovery.

The capture did not show authentication, exploitation, file transfer or an interactive remote session.

## Capture Statistics

The Wireshark Protocol Hierarchy view recorded approximately 136,038 packets, of which 135,950 were TCP. This strong TCP concentration was consistent with the controlled scan activity performed during the capture.

## SYN Scan Analysis

The following filter isolated initial SYN packets:

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

Observed characteristics included:

- one source contacting many destination ports
- repeated SYN packets with little or no payload
- numerous incomplete handshakes
- TCP RST responses for closed ports

These characteristics support a TCP SYN scan assessment.

## Services Identified

| Port | Service | Interpretation |
|---:|---|---|
| 135/tcp | MSRPC | Microsoft Remote Procedure Call |
| 139/tcp | NetBIOS-SSN | NetBIOS session service |
| 445/tcp | SMB | Windows file and service communication |
| 5040/tcp | Unidentified in this analysis | Required further validation |
| 49664–49670/tcp | Dynamic RPC | Windows ephemeral RPC services |

Open services increase the value of the reconnaissance because they identify possible follow-on targets. The capture itself does not prove that those services were abused.

## TCP Stream Review

A selected stream contained limited application data, including a `version.bind` string. No sustained session or payload transfer was identified from that stream.

## MITRE ATT&CK

- **Observed:** T1046 — Network Service Discovery
- **Not observed:** T1021 — Remote Services

## Detection Concept

Generate an investigation lead when one source sends SYN packets to more than 20 unique destination ports on the same host within 60 seconds.

```text
source = same IP
TCP SYN = true
TCP ACK = false
unique destination ports > 20
window = 60 seconds
```

The threshold must be tuned for approved scanners and normal management tools.

## Risk Assessment

**Assessment:** Medium-priority reconnaissance in an internal network context.

If unauthorised, the activity could precede credential attacks, service exploitation or lateral movement. During this controlled exercise, no follow-on compromise was observed.

## Recommended Actions

1. Confirm whether the scanning source is authorised.
2. Review endpoint process telemetry for the tool and user responsible.
3. Search for similar scanning against other internal hosts.
4. Monitor identified services for authentication or exploitation attempts.
5. Tune detections to exclude approved scanners without suppressing unexpected internal activity.

## Evidence

- [SYN pattern](../screenshots/recon-syn-pattern.png)
- [Destination ports](../screenshots/recon-destination-ports.png)
- [Conversations](../screenshots/recon-conversations.png)
- [Protocol hierarchy](../screenshots/recon-protocol-hierarchy.png)
- [TCP stream](../screenshots/recon-tcp-stream.png)
