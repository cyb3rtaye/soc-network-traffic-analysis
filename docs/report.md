# Network Traffic Analysis -- Run 02 (Reconnaissance Simulation)

## Case Information

-   **Case ID:** LAB-NT-RECON-02\
-   **Date:** 26 February 2026\
-   **Source Host:** 192.168.1.3\
-   **Target Host:** 192.168.1.4\
-   **Environment:** Isolated VirtualBox Internal Network
    (192.168.1.0/24)\
-   **Capture File:** `pcaps/recon-run02.pcap`

------------------------------------------------------------------------

# 1. Executive Summary

During internal traffic monitoring, a burst of TCP connection attempts
was observed originating from host `192.168.1.3` targeting multiple
destination ports on host `192.168.1.4`.

Packet level inspection revealed sequential TCP SYN packets across
numerous ports, consistent with reconnaissance behaviour used to
enumerate open services prior to exploitation.

The target Windows host was confirmed to expose several services,
including MSRPC, NetBIOS and SMB.

The activity reflects behaviour typically associated with the
reconnaissance phase of an intrusion lifecycle.

------------------------------------------------------------------------

# 2. Technical Analysis

## 2.1 Capture Statistics

From Protocol Hierarchy:

-   **Total Packets:** 136,038\
-   **TCP Packets:** 135,950 (99.9%)\
-   Minimal UDP/ICMP observed\
-   Heavy TCP dominance aligns with scanning activity

------------------------------------------------------------------------

## 2.2 SYN Scan Behaviour

Filter applied:

    tcp.flags.syn == 1 && tcp.flags.ack == 0

Findings:

-   High concentration of SYN packets
-   Source: `192.168.1.3`
-   Destination: `192.168.1.4`
-   Sequential port targeting
-   Minimal or no data payload
-   Many short-lived TCP attempts

Closed ports responded with TCP RST packets confirming active probing.

This behaviour is consistent with a TCP half open (SYN) scan.

------------------------------------------------------------------------

## 2.3 Open Services Identified

Nmap results confirmed the following open ports:

  Port               Service       Description
  ------------------ ------------- ------------------------------
  135/tcp            MSRPC         Microsoft RPC
  139/tcp            NetBIOS-SSN   NetBIOS Session Service
  445/tcp            SMB           Microsoft Directory Services
  5040/tcp           Unknown       Likely Windows Service
  49664--49670/tcp   Dynamic RPC   Windows ephemeral RPC ports

These services are frequently leveraged for:

-   Lateral movement
-   Credential harvesting
-   Remote code execution
-   SMB-based attacks
-   RPC abuse

------------------------------------------------------------------------

## 2.4 TCP Stream Observation

Inspection of a selected TCP stream revealed:

-   32 bytes of application-layer data
-   ASCII content (`version.bind`)
-   No sustained communication

This indicates probing of service availability rather than full session
establishment.

------------------------------------------------------------------------

# 3. Behavioural Assessment

-   Rapid sequential port access
-   Multiple destination ports in short time interval
-   SYN dominant traffic
-   Numerous incomplete TCP handshakes

This pattern aligns with internal reconnaissance and service enumeration
behaviour.

In a production environment, this activity would require validation of
authorization and potential escalation.

------------------------------------------------------------------------

# 4. MITRE ATT&CK Mapping

-   **T1046 -- Network Service Discovery**
-   **T1021 -- Remote Services (potential follow-on stage)**

The activity aligns with pre-exploitation reconnaissance tactics.

------------------------------------------------------------------------

# 5. Detection Logic

## 5.1 SIEM Detection Concept

Alert when:

-   A single source IP sends SYN packets to \>20 unique destination
    ports
-   Within a short time period e.g 60 seconds

Example detection logic:

    where tcp.flags.syn == 1
    and tcp.flags.ack == 0
    group by src_ip
    count distinct dest_port > 20
    within 60 seconds

------------------------------------------------------------------------

## 5.2 IDS Rule Concept (Snort-style Example)

    alert tcp any any -> 192.168.1.0/24 any (
        msg:"Possible TCP Port Scan Detected";
        flags:S;
        threshold:type both, track by_src, count 20, seconds 60;
        sid:1000001;
    )

------------------------------------------------------------------------

# 6. Forensic Observations

-   No payload-based exploitation observed
-   No credential exchange detected
-   No SMB authentication attempts seen
-   No file transfer indicators present

The activity appears limited to reconnaissance.

------------------------------------------------------------------------

# 7. Risk Assessment

**Risk Level:** Medium (Internal Reconnaissance)

If unauthorized, this activity could precede:

-   SMB exploitation
-   Credential relay attacks
-   Remote service abuse
-   Domain enumeration

Proper segmentation and monitoring are recommended.

------------------------------------------------------------------------

# 8. Skills Demonstrated

-   Packet capture scoping
-   TCP handshake analysis
-   Service enumeration interpretation
-   Protocol hierarchy analysis
-   IOC extraction
-   Detection engineering mindset
-   Threat modeling
-   Structured incident documentation
