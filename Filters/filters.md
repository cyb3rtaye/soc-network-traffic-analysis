# Wireshark Filters Used -- Run 02 (Reconnaissance Analysis)

This file documents the display filters used during packet analysis.\
These filters were used to isolate reconnaissance behaviour and validate
findings.

------------------------------------------------------------------------

## 1. Identify SYN Scan Activity

    tcp.flags.syn == 1 && tcp.flags.ack == 0

Purpose: - Isolate TCP SYN packets - Detect half open scan behaviour -
Identify port probing attempts

------------------------------------------------------------------------

## 2. Source-to-Destination Isolation

    ip.src == 192.168.1.3 && ip.dst == 192.168.1.4

Purpose: - Focus analysis on suspected scanning host - Remove unrelated
traffic

------------------------------------------------------------------------

## 3. SYN Scan (Scoped to Target)

    ip.src == 192.168.1.3 && ip.dst == 192.168.1.4 && tcp.flags.syn == 1 && tcp.flags.ack == 0

Purpose: - Identify reconnaissance attempts specifically targeting the
Windows endpoint

------------------------------------------------------------------------

## 4. RST Response Detection

    tcp.flags.reset == 1

Purpose: - Confirm closed port responses - Validate active probing
behaviour

------------------------------------------------------------------------

## 5. Follow TCP Stream

Right click packet → Follow → TCP Stream

Purpose: - Inspect application layer interaction - Determine if activity
progressed beyond scanning

------------------------------------------------------------------------

## 6. Protocol Hierarchy Review

Statistics → Protocol Hierarchy

Purpose: - Identify traffic distribution - Confirm TCP dominance
consistent with scan behaviour
