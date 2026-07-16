# Incident Response Playbooks, Detection to Containment

Three IR playbooks for the incidents a Tier 1 analyst actually works: brute force, phishing, and malware. Built on the NIST SP 800-61 lifecycle, mapped to MITRE ATT&CK, written to be followed at 3am.

## At a Glance

| Field | Detail |
| --- | --- |
| Deliverable | 3 incident response playbooks |
| Framework | NIST SP 800-61 Incident Handling Guide |
| Threat Model | MITRE ATT&CK Enterprise Matrix v14 |
| Coverage | Brute force, phishing, malware |
| Techniques Mapped | 9 |
| Type | Documentation build, not an investigation |

## What This Is

This is not an incident. It is the thing you write before the incident, so that when it happens nobody has to think from scratch.

Playbooks exist because analysts under pressure skip steps. They contain first and preserve evidence never. They reset the password and leave the session alive. They restore the endpoint before they know how it was compromised, and hand the attacker the host a second time.

A playbook removes the improvisation from the worst hour of someone's week.

Scope stated plainly: these are procedures authored against the NIST framework and validated on paper, not procedures battle tested against live incidents in a production SOC. What they demonstrate is framework fluency and response sequencing.

## The Framework

![IR Framework](./screenshots/ir_framework.png)

NIST SP 800-61 was used as the structural basis, with the lifecycle broken into seven operational steps and the terminology aligned to standard SOC vocabulary so a playbook reads the same way a handoff does.

| Step | Phase | Purpose |
| --- | --- | --- |
| 1 | Detection | Identify the incident via alert or user report |
| 2 | Triage | Assess severity and scope, validate the alert is real |
| 3 | Containment | Stop the spread, limit blast radius |
| 4 | Investigation | Understand what the attacker did and capture IOCs |
| 5 | Eradication | Remove the threat and every persistence artefact |
| 6 | Recovery | Restore operations with validation |
| 7 | Lessons Learned | Improve detection, share IOCs |

The order is the whole point. Containment before investigation limits damage. Investigation before eradication means you remove all of it rather than the part you saw. Recovery before either means you do the incident twice.

## Playbook 1, Brute Force

![Playbook 1 Brute Force](./screenshots/playbook1_brute_force.png)

Severity High. Technique T1110. Triggers on 5 or more failed logins within 60 seconds from a single source.

Detection. SIEM alert on the failed authentication threshold.

Triage. Validate the source IP, the account scope, and the time window.

Containment. Block the source at the firewall, disable the targeted account.

Investigation. Check the same log source for successful logins from that source. This is the step that decides whether this is noise or a breach.

Eradication. Reset credentials and revoke active sessions.

Recovery. Re enable the account with MFA enforced.

Lessons Learned. Tune the lockout threshold, feed the source IP to threat intel.

The step that gets skipped: session revocation. A password reset does not kill a session that is already open. The attacker keeps working while the analyst files the ticket as resolved.

## Playbook 2, Phishing

![Playbook 2 Phishing](./screenshots/playbook2_phishing.png)

Severity High. Technique T1566. Triggers on a user report or an email gateway alert.

Detection. Reported message or gateway flag.

Triage. Analyse sender, headers, links, attachments.

Containment. Purge the email from all mailboxes, block the sender domain at the gateway.

Investigation. Identify every recipient, who clicked, and whether credentials were submitted.

Eradication. Reset credentials for anyone who clicked, isolate affected endpoints.

Recovery. Restore access after verification, enforce MFA.

Lessons Learned. Awareness notice, gateway rule tuning, IOC sharing.

The step that gets skipped: click telemetry. Purging the email feels like resolution, but it only stops the next victim. It does nothing for the person who already typed their password into the landing page, and until you know who clicked, you do not know the size of the incident.

## Playbook 3, Malware

![Playbook 3 Malware](./screenshots/playbook3_malware.png)

Severity Critical. Technique T1204. Triggers on an antivirus or EDR alert.

Detection. Alert on suspicious file, hash, or behaviour.

Triage. Validate the alert, capture the hash, identify the endpoint.

Containment. Network isolate the endpoint immediately.

Investigation. Submit the hash to VirusTotal and Hybrid Analysis, identify persistence, map lateral movement.

Eradication. Remove the malware and clear every persistence artefact, cron, registry, services.

Recovery. Restore from clean backup, validate integrity before reconnecting.

Lessons Learned. Update detection rules, share IOCs, harden the endpoint.

The step that gets skipped: evidence preservation before remediation. Hash, memory, disk image, in that order, before anything is deleted. The instinct is to clean the machine. Clean it first and the investigation is over, because the evidence went with it.

## MITRE ATT&CK Mapping

![MITRE Mapping](./screenshots/mitre_mapping.png)

| Incident | Tactic | Technique | ID |
| --- | --- | --- | --- |
| Brute force | Credential Access | Brute force | T1110 |
| Brute force | Defence Evasion | Valid accounts | T1078 |
| Phishing | Initial Access | Phishing | T1566 |
| Phishing | Execution | User execution | T1204 |
| Phishing | Credential Access | Steal web session cookie | T1539 |
| Malware | Execution | User execution | T1204 |
| Malware | Persistence | Boot or logon autostart execution | T1547 |
| Malware | Command and Control | Application layer protocol | T1071 |
| Malware | Exfiltration | Exfiltration over C2 channel | T1041 |

Mapping a playbook to ATT&CK is not decoration. It turns a response procedure into a coverage question: if this technique has a playbook but no detection rule behind it, the gap is now visible on paper instead of discovered during an incident.

## Playbook Validation

![Final Playbooks](./screenshots/final_playbooks.png)

All three were reviewed against the seven step framework for completeness, ATT&CK coverage checked per incident type, and IOC capture, evidence preservation, and escalation paths confirmed present in each.

Playbooks are living documents. Quarterly review against current threat intelligence is standard practice, and a playbook that has not been reviewed in a year is a liability that looks like a control.

## IOC Categories by Playbook

| Playbook | Category | What to Capture |
| --- | --- | --- |
| Brute force | Network | Source IP, geolocation, ASN |
| Brute force | Authentication | Failed logon event IDs, target usernames, time window |
| Phishing | Email | Sender domain, subject, message hash, URLs |
| Phishing | Behavioural | Click events, credential submission, attachment hashes |
| Malware | File | MD5 and SHA256, filename, path |
| Malware | Network | C2 domains, IPs, beaconing intervals |
| Malware | Host | Registry persistence keys, scheduled tasks, services |

Every phase has to produce an artefact. A step with nothing to hand over did not happen.

## Recommended Deployment

Wire the trigger conditions into SIEM alert routing so the playbook opens with the alert.

Run tabletop exercises against each one, because a playbook nobody has walked through is untested.

Capture IOCs from every invocation into a central threat intel repository.

Use the ATT&CK mapping to find detection gaps and prioritise tuning against them.

Review quarterly against new threat intelligence.

## What This Lab Demonstrates

Applying NIST SP 800-61 to concrete incident types rather than citing it.

Sequencing containment, investigation, eradication, and recovery in the order that survives contact with a real incident.

Knowing the step each playbook exists to stop an analyst skipping.

Mapping response procedures to ATT&CK to surface detection gaps.

Documenting IOCs across host, network, identity, and email layers.

Treating evidence preservation as a precondition of remediation, not a nice to have.

## Repository Structure

```
incident-response-playbook-lab/
├── README.md
├── playbook1-brute-force.md
├── playbook2-phishing.md
├── playbook3-malware.md
├── mitre-attack-mapping.md
└── screenshots/
    ├── ir_framework.png
    ├── playbook1_brute_force.png
    ├── playbook2_phishing.png
    ├── playbook3_malware.png
    ├── mitre_mapping.png
    └── final_playbooks.png
```

---

[![LinkedIn](https://img.shields.io/badge/LinkedIn-WilliamInCyber-blue?style=flat&logo=linkedin)](https://linkedin.com/in/WilliamInCyber)
[![X](https://img.shields.io/badge/X-WilliamInCyber-black?style=flat&logo=x)](https://x.com/WilliamInCyber)
