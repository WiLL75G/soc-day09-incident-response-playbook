# MITRE ATT&CK Mapping, Incident Response Playbook

## Overview

This document maps each incident type covered in this playbook set to the MITRE ATT&CK framework.

A scenario category does not prove every technique listed beneath it. This mapping separates the primary technique directly represented by each incident type from techniques that are only possible follow on activity and should be mapped once an investigation confirms them.

## Primary Mapping

| Playbook | Primary ATT&CK Reference | Why |
| --- | --- | --- |
| Brute Force | T1110, Brute Force | Repeated authentication attempts are the behavior covered by the playbook |
| Phishing | T1566, Phishing | Delivery of a phishing message is the behavior covered by the playbook |
| Malware | Evidence dependent | Malware is not one ATT&CK technique and requires observed behavior before additional techniques are assigned |

## Techniques to Investigate For, Not Assumed

| Playbook | Technique | ID | Why it is not automatic |
| --- | --- | --- | --- |
| Brute force | Valid Accounts | T1078 | Only applies if a login actually succeeded |
| Phishing | User Execution | T1204 | Only applies if the recipient interacted with a payload, not merely received the email |
| Phishing | Steal Web Session Cookie | T1539 | Only applies to a scenario involving session cookie theft specifically |
| Malware | User Execution | T1204 | Only applies once execution is confirmed, not assumed from the alert alone |
| Malware | Boot or Logon Autostart Execution | T1547 | Only applies if a persistence mechanism is actually found |
| Malware | Application Layer Protocol | T1071 | Only applies if outbound C2 traffic is actually observed |
| Malware | Exfiltration Over C2 Channel | T1041 | Only applies if data exfiltration is actually confirmed |

Each of these should be added to an incident's confirmed mapping only when the investigation establishes it, not applied automatically because the incident falls into that category.

## SOC Analyst Notes

- Map the primary technique at detection, based on the alert category
- Add secondary techniques only as investigation confirms them
- Use the ATT&CK Navigator to visualise coverage gaps across all three playbooks
- Reference confirmed technique IDs, not assumed ones, in incident reports
