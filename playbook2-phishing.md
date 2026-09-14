# Playbook 2, Phishing Email Response

## Incident Overview

- Type: Phishing Email
- Initial severity: determined during triage
- MITRE ATT&CK: T1566, Phishing

## Step 1, Detection

- User reports a suspicious email
- Email gateway flags the message as malicious
- Attachment or link flagged by an AV engine

## Step 2, Triage

- Identify the sender email address
- Check sender domain reputation on VirusTotal
- Identify all recipients of the same email
- Determine how widely the message was delivered and whether it warrants further investigation

## Step 3, Containment

- Pull the email from all affected mailboxes and block the sender domain at the gateway, these are low cost actions that do not need to wait for full investigation
- Stronger actions, resetting credentials or isolating an endpoint, wait for investigation to confirm they are warranted
- A click alone does not confirm credential submission

## Step 4, Investigation

- Analyse email headers for spoofing indicators
- Submit the attachment hash to VirusTotal
- Check proxy logs for outbound connections to the phishing URL
- Review endpoint logs for file drops or process execution
- For each recipient, establish where they fall on this chain: received, clicked, submitted credentials, or executed a payload

## Step 5, Eradication

- Remove any dropped files from affected endpoints
- Revoke and reset credentials confirmed compromised
- Block the phishing URL at the web proxy and firewall if not already blocked during containment

## Step 6, Recovery

- Restore the endpoint from a clean backup if compromise is confirmed
- Enable the user account again after credential reset
- Monitor affected accounts for 48 hours
- Confirm no further phishing emails from the same campaign

## Step 7, Lessons Learned

- Document the full email header analysis
- Run a phishing awareness reminder for affected staff
- Update email gateway rules based on the attack pattern
- Report campaign IOCs to the threat intelligence team

## Evidence to Document

- Sender domain, subject, message hash, URLs
- Click events, credential submission, attachment hashes

## The Step That Gets Skipped

Click telemetry. Purging the email feels like resolution, but it only stops the next victim. It does nothing for the person who already typed their password into the landing page, and until you know who clicked, you do not know the size of the incident.
