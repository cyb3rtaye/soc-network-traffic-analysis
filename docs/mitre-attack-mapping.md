# MITRE ATT&CK Mapping

## Observed Technique

### T1046 — Network Service Discovery

The source host generated TCP SYN traffic across many destination ports on the Windows endpoint. This behaviour is consistent with identifying available network services before possible follow-on activity.

## Supporting Evidence

- repeated SYN packets from `192.168.1.3` to `192.168.1.4`
- multiple destination ports in a short period
- RST responses from closed ports
- open services identified on TCP 135, 139 and 445

## Mapping Boundary

The packet capture does not show authentication, exploitation, remote-service use or lateral movement. T1021 — Remote Services is therefore a possible follow-on technique, not an observed technique in this case.

## Defensive Use

- alert when one source contacts many destination ports on one host
- compare SYN counts with completed connections
- suppress approved vulnerability scanners by source and schedule
- investigate unexpected internal scanning from user workstations
