# IR-001: Failed Logons (Event ID 4625)

## Summary
On 4th January, 2026, multiple failed authentication attempts were observed on the Windows endpoint. Activity resembles password guessing/brute force against a local account.

## Environment
- Lab: VirtualBox Windows 10 endpoint
- Telemetry: Windows Security Event Log + Sysmon

## Detection / Evidence
- Data source: Windows Security Log
- Event ID: 4625 (An account failed to log on)
- Time range: 12:25:46 AM to 12:29:15 AM
- Count: 10 failed logons
- Notes: Repeated failed attempts occurred within ~3.5 minutes, it was repeated for the same username and logon type is 2.

## Timeline (UTC or local time)
- 12:25:46 AM First 4625 observed
- 12:26 AM Peak / repeated attempts
- 12:29:15 AM Last 4625 observed

## Triage Actions Taken
- Filtered Security log for Event ID 4625 and reviewed the failed logon entries.
- Reviewed event fields (account, logon type, failure reason)
- Assessed whether successful logon occurred (checked Event ID 4624) during the same window; Observed Logon Type 5 / SYSTEM service logons only (not an interactive user logon).
- Opened Event Viewer during triage (Sysmon Event ID 1 shows `mmc.exe` with `eventvwr.msc`).


## Assessment
- Likely cause: simulated password guessing activity in a controlled lab environment.
- Impact: No evidence of successful authentication observed in this scenario.

## Recommended Remediation
- Implement/adjust account lockout policy (limit repeated failed attempts).
- Enforce strong passwords and remove/disable unused accounts.
- Enable MFA where applicable
- Alert on spikes in 4625 events and correlate with successful logons (4624) and account changes.

## Screenshots
- docs/04-security-4625-failed-logons.png
- docs/05-sysmon-triage-proof.png