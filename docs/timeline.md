# Investigation Timeline

## Case Details

- **Case ID:** `LAB-NT-RECON-02`
- **Date:** 26 February 2026 (GMT)
- **Source:** `192.168.1.3`
- **Target:** `192.168.1.4`

## Events

| Time | Observation |
|---|---|
| 21:02 | TCP SYN activity begins from the source to multiple ports on the Windows endpoint. |
| 21:02–21:03 | Numerous short-lived connection attempts occur; closed ports respond with TCP RST packets. |
| 21:03 | Review confirms open Windows services including TCP 135, 139 and 445, with additional dynamic RPC ports. |

## Assessment

The traffic pattern is consistent with internal network service discovery. In a production environment, the next step would be to confirm whether the source was an approved scanner or an unauthorised system and then determine whether escalation was required.
