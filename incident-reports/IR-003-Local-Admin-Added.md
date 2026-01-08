# IR-003: New Local User Added to Administrators

## Summary
On 8 January 2026, a new local user account was created and added to the local Administrators group. This pattern is commonly investigated as potential privilege escalation or persistence.

## Environment
- Lab: VirtualBox Windows 10 endpoint
- Telemetry: Windows Security Event Log + Sysmon

## Detection / Evidence
- Security Event ID 4720 (user account created) — 8 Jan 2026 2:55:06 PM
- Security Event ID 4732 (member added to security-enabled local group) — 8 Jan 2026 2:55:45 PM
-**Note:** In Event ID 4732, the “Member → Account Name” field shows `-` (blank). This can occur after the user account is deleted, leaving the Member SID without name resolution. The change was correlated to `labadmin` using Event ID 4720 (account creation) and Sysmon EID 1 showing the exact command `net1 localgroup administrators labadmin /add`.
- Sysmon Event ID 1 (process create) showing `net1.exe localgroup administrators labadmin /add` — 8 Jan 2026 2:55:45 PM
- Security Event ID 4726 (user account deleted) — 8 Jan 2026 3:43:00 PM
- User: `labadmin`
- Group: `Administrators`

## Timeline (local time)
- 2:55:06 PM — Local user created (4720)
- 2:55:45 PM — Added to Administrators (4732) + command observed (Sysmon EID 1)
- 3:43:00 PM — User deleted (4726)

## Triage Actions Taken
- Filtered Security log for 4720/4732/4726 and validated the user and group involved.
- **Note** Correlated Security Event ID 4732 (Builtin\Administrators membership change) with Sysmon EID 1 (`net1.exe`) showing the command used to add `labadmin` to Administrators, and with Security Event ID 4720 confirming the `labadmin` account creation.
- Reviewed Sysmon EID 1 to confirm the exact command used to add the user to Administrators (`net1.exe ... /add`).

## Assessment
- Likely cause: simulated administrative change in a controlled lab environment.
- Impact: A new account gained local admin privileges during the time window.
- **Note** Although the 4732 event did not resolve the member name, the combined evidence (4720 + Sysmon command line) confirms `labadmin` was added to the local Administrators group during the window.

## Recommended Remediation
- Alert on new local user creation and additions to local Administrators.
- Restrict who can create local users / modify local groups; use least privilege.
- Monitor for `net.exe/net1.exe` usage modifying local groups and investigate initiating user context.
- Regularly audit local Administrators group membership.

## Screenshots
- docs/07-security-4720-user-created.PNG
- docs/08-security-4732-added-to-admins.PNG
- docs/09-sysmon-net-user-admin.PNG
- docs/10-security-4726-user-deleted.PNG
