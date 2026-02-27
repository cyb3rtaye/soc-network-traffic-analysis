# MITRE ATT&CK Mapping – LAB-NT-RECON-02

This section maps the observed reconnaissance behaviour from Run 02 to MITRE ATT&CK techniques.

## Observed behaviour (summary)
- Source host (`192.168.1.3`) generated high-volume TCP SYN traffic to many destination ports on the Windows endpoint (`192.168.1.4`).
- Traffic pattern is consistent with **service/port discovery** prior to exploitation or lateral movement.

## Techniques
### T1046 – Network Service Discovery
**Why it applies:** Rapid multi-port probing and enumeration to identify exposed services (e.g., MSRPC/NetBIOS/SMB).  
**Evidence:** SYN scan pattern + multiple destination ports + TCP conversations showing many short-lived attempts.

### (Potential follow-on) T1021 – Remote Services
**Why it matters:** Discovered services (SMB/RPC/NetBIOS) are commonly used for lateral movement if an attacker gains credentials.  
**Note:** This was not observed in this capture (no authentication / interactive remote sessions detected), but it is a realistic next step in the attack chain.

## Defensive notes
- Alert on **one source hitting many destination ports** within a short window.
- Track **SYN-to-ACK ratio** and **RST spikes** as scan indicators.
- Treat internal recon as suspicious unless the source is a known/approved scanner.
