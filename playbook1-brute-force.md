# Playbook 1, Brute Force Attack Response

## Incident Overview

- Type: Brute Force Attack
- Initial severity: determined during triage
- MITRE ATT&CK: T1110, Brute Force

## Step 1, Detection

- Alert fires for 5 or more failed logins in 60 seconds
- Source: Splunk alert or SIEM rule
- Log source: /var/log/auth.log or Windows Event ID 4625
- This threshold is a playbook example and would need tuning against the environment where it is deployed

## Step 2, Triage

- Identify the source IP address
- Check whether the IP is internal or external
- Check whether the target account is privileged
- Search VirusTotal for source IP reputation
- Determine what containment step the current evidence justifies right now

## Step 3, Containment

Proportionate to what triage established, not automatic for every alert.

- If the source is confirmed malicious or the risk is unclear, block it at the firewall
- If the targeted account is privileged or a successful login is suspected, disable the account
- Revoke active sessions rather than resetting the password alone, a reset does not end a session that is already open
- Notify the account owner

## Step 4, Investigation

- Pull all logs from the source IP for the last 24 hours
- Check whether any login succeeded after the failures. This is the finding that decides whether this is noise or a breach.
- Identify every account targeted

## Step 5, Eradication

- Reset the password of any targeted account where compromise is suspected or confirmed
- Review firewall rules
- Enable account lockout policy if not already set

## Step 6, Recovery

- Enable the account again once password reset and session revocation are both confirmed
- Monitor the account for 24 hours post incident
- Confirm no further failed attempts from the blocked IP

## Step 7, Lessons Learned

- Document the timeline of events
- Update detection rule thresholds if needed
- Report to Tier 2 if a successful login was detected

## Evidence to Document

- Source IP address
- Target username
- Authentication timestamps
- Number of failed attempts
- Successful authentication events if present
- Containment actions taken

## The Step That Gets Skipped

Session revocation. A password reset does not kill a session that is already open. The attacker keeps working while the analyst files the ticket as resolved.
