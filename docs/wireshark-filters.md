# Wireshark Filters

These display filters were used during the Run 02 reconnaissance analysis.

## TCP SYN Packets Without ACK

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

Use this to isolate initial TCP connection attempts and identify possible SYN scanning.

## Source-to-Target Traffic

```wireshark
ip.src == 192.168.1.3 && ip.dst == 192.168.1.4
```

Use this to remove unrelated traffic and focus on the documented systems.

## SYN Scan Scoped to the Target

```wireshark
ip.src == 192.168.1.3 && ip.dst == 192.168.1.4 && tcp.flags.syn == 1 && tcp.flags.ack == 0
```

Use this to count and review the destination ports contacted by the source.

## TCP Reset Responses

```wireshark
tcp.flags.reset == 1
```

Use this to identify closed-port responses and incomplete connection attempts.

## Useful Wireshark Views

- **Statistics → Conversations:** compare connection counts by source and target
- **Statistics → Protocol Hierarchy:** identify protocol distribution
- **Follow → TCP Stream:** inspect a selected stream for application data

Filters should be interpreted together with timestamps, port distribution and connection outcomes.
